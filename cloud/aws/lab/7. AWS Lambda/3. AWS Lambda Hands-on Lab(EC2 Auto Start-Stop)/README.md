# 3. AWS Lambda Hands-on Lab (Tự động bật/tắt máy chủ EC2 để tiết kiệm chi phí) - Hướng dẫn chi tiết

👉 **[Xem Đề bài / Yêu cầu bài Lab](3.%20AWS%20Lambda%20Hands-on%20Lab%28EC2%20Auto%20Start-Stop%29.md)**

## II. Các bước thực hiện chi tiết

### Bước 1: Tạo IAM Policy cấp quyền điều khiển EC2

Hàm Lambda cần được cấp quyền gửi lệnh Bật/Tắt tới máy chủ EC2.

1. Truy cập **IAM Console** $\rightarrow$ **Policies** $\rightarrow$ **Create policy**.
2. Nhập nội dung JSON sau:
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents"
               ],
               "Resource": "arn:aws:logs:*:*:*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:StartInstances",
                   "ec2:StopInstances"
               ],
               "Resource": "*"
           }
       ]
   }
   ```
3. Đặt tên Policy là `LambdaEC2ControlPolicy` và lưu lại.
4. Chuyển sang mục **Roles** $\rightarrow$ Tạo một Role mới tên là `LambdaEC2ControlRole` gắn kèm Policy `LambdaEC2ControlPolicy` trên.

---

### Bước 2: Tạo Lambda Function

1. Truy cập **AWS Lambda Console** $\rightarrow$ Chọn **Create function**.
2. Thiết lập cấu hình:
   * **Function name**: `auto-start-stop-ec2`.
   * **Runtime**: Chọn **Python 3.12** (hoặc mới nhất).
   * **Execution role**: Chọn *Use an existing role*, chọn Role `LambdaEC2ControlRole` đã tạo ở Bước 1.
3. Nhấn **Create function**.

---

### Bước 3: Viết mã nguồn Python điều khiển EC2 theo tham số

1. Thay thế mã nguồn mặc định của file `lambda_function.py` bằng đoạn mã sử dụng `boto3` dưới đây (đồng bộ với file [auto-start-stop-ec2.py](auto-start-stop-ec2.py)):
   ```python
   import json
   import boto3
   import botocore

   # Khởi tạo client kết nối tới dịch vụ EC2
   # region_name có thể cấu hình phù hợp với vùng chứa EC2 của bạn (ví dụ: ap-southeast-1)
   ec2 = boto3.client("ec2", region_name="ap-southeast-1")

   def lambda_handler(event, context):
       # Lấy tham số truyền vào từ Event payload
       instance_id = event['instance_id']
       action = event['action']
       
       if action == 'START':
           try:
               response = ec2.start_instances(InstanceIds=[instance_id], DryRun=False)
               print(response)
           except Exception as e:
               print(e)
       if action == 'STOP':
           try:
               response = ec2.stop_instances(InstanceIds=[instance_id], DryRun=False)
               print(response)
           except Exception as e:
               print(e)
               
       return {
           'statusCode': 200,
           'body': json.dumps('Complete modify EC2 status as your requested!')
       }
   ```
2. Nhấn nút **Deploy** để lưu và sẵn sàng chạy thử.

---

### Bước 4: Chạy thử nghiệm trực tiếp trên Lambda Console (Test)

Để đảm bảo hàm Lambda chạy chính xác, bạn cần có sẵn một máy chủ EC2 bất kỳ để thử nghiệm.

1. Vào **EC2 Console**, tìm đến máy chủ thử nghiệm của bạn và sao chép **Instance ID** của nó (ví dụ: `i-08364cc10f525e95c`).
2. Quay lại Lambda Console, nhấp chọn mũi tên bên cạnh nút **Test** $\rightarrow$ **Configure test event**.
3. Tạo test event tên là `test-stop-ec2` với cấu trúc JSON (thay thế ID thật của bạn):
   ```json
   {
     "instance_id": "i-08364cc10f525e95c",
     "action": "STOP"
   }
   ```
4. Nhấn **Save** và click **Test**.
5. Kiểm tra trạng thái máy chủ EC2 trên console, bạn sẽ thấy máy chủ đó tự động chuyển trạng thái sang **Stopping** và dừng hoàn toàn.
6. Tạo thêm test event `test-start-ec2` với JSON:
   ```json
   {
     "instance_id": "i-08364cc10f525e95c",
     "action": "START"
   }
   ```
7. Click **Test** và quan sát EC2 tự động bật trở lại trạng thái **Running**.

---

### Bước 5: Cấu hình lập lịch tự động qua Amazon EventBridge

Chúng ta sẽ sử dụng EventBridge Scheduler (hoặc EventBridge Rules) để tự động hóa kích hoạt định kỳ kèm việc truyền 2 tham số cần thiết:

#### 1. Cấu hình Rule Tự động Tắt máy chủ (19:00 tối hàng ngày từ Thứ 2 đến Thứ 6)
1. Truy cập **Amazon EventBridge Console** $\rightarrow$ **Rules** (hoặc **Schedules**) $\rightarrow$ **Create rule** (hoặc **Create schedule**).
2. Cấu hình thông tin cơ bản:
   * **Name**: `stop-ec2-daily-rule`.
   * **Rule type**: Chọn **Schedule** (Lập lịch).
3. Thiết lập lịch biểu:
   * **Cron expression**: Nhập `0 19 ? * MON-FRI *` (Chạy vào 19:00 UTC, từ thứ 2 đến thứ 6. *Lưu ý quy đổi múi giờ địa phương sang UTC*).
4. Thiết lập Target:
   * **Target**: Chọn **Lambda function**.
   * **Function**: Chọn `auto-start-stop-ec2`.
   * **Additional settings** $\rightarrow$ **Configure target input**: Chọn **Constant (JSON text)** và điền payload:
     ```json
     {
       "instance_id": "i-08364cc10f525e95c",
       "action": "STOP"
     }
     ```
5. Nhấp chọn **Create rule**.

#### 2. Cấu hình Rule Tự động Bật máy chủ (07:00 sáng hàng ngày từ Thứ 2 đến Thứ 6)
1. Tạo một Rule thứ hai tên là `start-ec2-daily-rule`.
2. Chọn **Schedule** với **Cron expression**: `0 7 ? * MON-FRI *` (Chạy vào 07:00 UTC).
3. Chọn Target là Lambda function `auto-start-stop-ec2`.
4. Trong mục **Configure target input**, chọn **Constant (JSON text)** và điền payload:
   ```json
   {
     "instance_id": "i-08364cc10f525e95c",
     "action": "START"
   }
   ```
5. Nhấp chọn **Create rule**.

---

* **Bài trước**: [2. AWS Lambda Hands-on Lab(Resize Image on S3) (Lab Resize ảnh trên S3)](../2.%20AWS%20Lambda%20Hands-on%20Lab%28Resize%20Image%20on%20S3%29/README.md)
* **Bài tiếp theo**: [4. AWS Lambda Hands-on Lab(Read CSV and Save to DynamoDB) (Lab đọc CSV lưu vào DynamoDB)](../4.%20AWS%20Lambda%20Hands-on%20Lab%28Read%20CSV%20and%20Save%20to%20DynamoDB%29/README.md)

---

👉 **[Quay lại Đề bài](3.%20AWS%20Lambda%20Hands-on%20Lab%28EC2%20Auto%20Start-Stop%29.md)**
