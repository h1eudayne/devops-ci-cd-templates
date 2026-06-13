# 4. AWS Lambda Hands-on Lab (Đọc tệp CSV từ S3 và lưu vào DynamoDB) - Hướng dẫn chi tiết

 **[Xem Đề bài / Yêu cầu bài Lab](4.%20AWS%20Lambda%20Hands-on%20Lab%28Read%20CSV%20and%20Save%20to%20DynamoDB%29.md)**

## II. Các bước thực hiện chi tiết

### Bước 1: Tạo bảng DynamoDB

1. Truy cập **Amazon DynamoDB Console** $\rightarrow$ **Tables** $\rightarrow$ **Create table**.
2. Thiết lập thông số:
   * **Table name**: `employee`
   * **Partition key**: `id` (Kiểu dữ liệu: **String**)
3. Nhấp chọn **Create table** và đợi bảng chuyển sang trạng thái *Active*.

---

### Bước 2: Tạo IAM Policy & Role cho Lambda

Lambda cần quyền đọc tệp từ S3 và ghi bản ghi (PutItem) vào bảng DynamoDB.

1. Truy cập **IAM Console** $\rightarrow$ **Policies** $\rightarrow$ **Create policy**.
2. Sử dụng trình chỉnh sửa **JSON** nhập policy sau:
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
                   "s3:GetObject"
               ],
               "Resource": "arn:aws:s3:::*-source/*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "dynamodb:PutItem"
               ],
               "Resource": "arn:aws:dynamodb:*:*:table/employee"
           }
       ]
   }
   ```
3. Lưu Policy với tên `LambdaCSVToDynamoDBPolicy`.
4. Tạo Role mới tên là `LambdaCSVToDynamoDBRole` gắn kèm Policy vừa tạo.

---

### Bước 3: Tạo Lambda Function

1. Truy cập **AWS Lambda Console** $\rightarrow$ Chọn **Create function**.
2. Thiết lập:
   * **Function name**: `s3-csv-to-dynamodb`.
   * **Runtime**: Chọn **Python 3.12** (hoặc mới nhất).
   * **Execution role**: Chọn *Use an existing role*, chọn Role `LambdaCSVToDynamoDBRole`.
3. Nhấn **Create function**.

---

### Bước 4: Viết mã nguồn xử lý CSV trong Lambda

Chúng ta sẽ sử dụng thư viện xử lý chuỗi cơ bản của Python để tách các hàng dữ liệu từ tệp CSV và đưa vào DynamoDB.

1. Thay thế mã nguồn trong tệp `lambda_function.py` bằng đoạn code sau (đồng bộ hoàn toàn với tệp [s3-csv-to-dynamodb.py](s3-csv-to-dynamodb.py)):
   ```python
   import json
   import boto3
   from csv import reader

   s3 = boto3.client('s3')
   dynamodb = boto3.resource('dynamodb')
   table = dynamodb.Table('employee')

   def lambda_handler(event, context):
       # Lấy thông tin bucket và key của file CSV từ event object
       bucket = event['Records'][0]['s3']['bucket']['name']
       key = event['Records'][0]['s3']['object']['key']

       # Tải tệp CSV từ S3 xuống
       response = s3.get_object(Bucket=bucket, Key=key)
       content = response['Body'].read().decode('utf-8')
       
       # Tách file thành từng hàng dựa trên ký tự xuống dòng
       rows = content.split("\n")
       
       # Lọc bỏ các dòng trống
       users = list(filter(None, rows))
       
       # Duyệt qua từng dòng và lưu vào bảng DynamoDB
       for user in users:
           user_data = user.split(",")
           table.put_item(Item = {
               "id" : user_data[0],
               "name" : user_data[1],
               "birthday": user_data[2],
               "salary" : user_data[3]
           })
       print('Finished insert data to DynamoDB')
       return {
           'statusCode': 200,
           'body': json.dumps('Successfully to processed!')
       }
   ```
2. Nhấn nút **Deploy** để cập nhật mã nguồn.

---

### Bước 5: Cấu hình S3 Event Trigger

1. Mở **S3 Console** $\rightarrow$ Click vào Bucket nguồn của bạn (ví dụ: `h1eudayne-images-source`).
2. Chọn tab **Properties**, cuộn xuống phần **Event notifications** $\rightarrow$ Chọn **Create event notification**.
3. Cấu hình:
   * **Event name**: `csv-upload-trigger`.
   * **Suffix**: `.csv` (chỉ kích hoạt khi upload file CSV).
   * **Event types**: Tích chọn **All object create events**.
   * **Destination**: Chọn **Lambda function**, chọn hàm `s3-csv-to-dynamodb` đã tạo ở Bước 3.
4. Nhập chọn **Save changes**.

---

## III. Xác minh kết quả thực hành (Validation)

1. Tạo một tệp tin trên máy tính local của bạn tên là `employee.csv` có nội dung sau (hoặc sử dụng tệp [employee.csv](employee.csv) có sẵn trong dự án):
   ```csv
   emp-001,Linh,1992/05/06,5000000
   emp-002,Trang,1993/06/22,6000000
   emp-003,Thu,1994/07/18,7000000
   emp-004,Bao,1995/08/30,5500000
   emp-005,Duong,1996/09/28,4500000
   ```
2. Tải tệp `employee.csv` lên S3 Bucket nguồn của bạn.
3. Chuyển sang **DynamoDB Console** $\rightarrow$ Chọn bảng **employee** $\rightarrow$ Nhấp chọn **Explore table items**.
4. Bạn sẽ thấy các bản ghi nhân viên từ file CSV đã được tự động phân tách và lưu trữ chính xác trong bảng DynamoDB.

---

* **Bài trước**: [3. AWS Lambda Hands-on Lab(EC2 Auto Start-Stop) (Lab bật tắt EC2 tự động)](../3.%20AWS%20Lambda%20Hands-on%20Lab%28EC2%20Auto%20Start-Stop%29/README.md)
* **Bài tiếp theo**: [8. EKS (Elastic Kubernetes Service)](../../../services/8.%20EKS.md)

---

 **[Quay lại Đề bài](4.%20AWS%20Lambda%20Hands-on%20Lab%28Read%20CSV%20and%20Save%20to%20DynamoDB%29.md)**
