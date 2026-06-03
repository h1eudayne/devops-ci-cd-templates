# 🚀 Thiết Kế Khả Năng Co Giãn (Scaling System Design)

Co giãn (Scaling) là khả năng mở rộng dung lượng của hệ thống để xử lý lượng tải lớn hơn (ví dụ: lượng request tăng đột biến trong các chiến dịch khuyến mãi hoặc lượng người dùng tăng trưởng).

---

## 🎯 Phân Loại Chiến Lược Co Giãn

1. **Mở rộng theo chiều dọc (Vertical Scaling - Scale Up)**:
   * **Nguyên lý**: Tăng thêm tài nguyên phần cứng (CPU, RAM, Disk) cho máy chủ hiện tại.
   * **Ưu điểm**: Dễ thực hiện, không cần thay đổi kiến trúc code của ứng dụng.
   * **Nhược điểm**: Có giới hạn vật lý phần cứng và gây Downtime khi nâng cấp.

2. **Mở rộng theo chiều ngang (Horizontal Scaling - Scale Out)**:
   * **Nguyên lý**: Thêm nhiều máy chủ mới vào cụm chạy song song để chia sẻ tải.
   * **Ưu điểm**: Không giới hạn năng lực mở rộng, tính sẵn sàng cao.
   * **Nhược điểm**: Đòi hỏi ứng dụng phải thiết kế không trạng thái (Stateless), cần hệ thống Load Balancer và phức tạp hơn khi đồng bộ dữ liệu.

---

## 🚀 Các Điểm Mấu Chốt Khi Thiết Kế Hệ Thống Tải Cao

*   **Caching**: Đưa dữ liệu thường xuyên truy xuất lên RAM (sử dụng Redis) để giảm tải trực tiếp cho Database.
*   **Database Read/Write Splitting**: Định hướng luồng ghi đến Database Master, và phân phối luồng đọc đến các Database Slave để chia sẻ gánh nặng.
*   **Auto-scaling**: Tự động tăng giảm số lượng ứng dụng dựa trên mức sử dụng CPU/RAM thực tế.

---

## 🔗 Liên Kết Thực Hành DevOps
Hãy tham khảo cách cấu hình co giãn tự động và caching trực tiếp trong repo này:

*   **Auto-scaling**: [Kubernetes Horizontal Pod Autoscaler (HPA)](../../on-premise/kubernetes/hpa/) (Cấu hình tự động scale số lượng Pod dựa theo thông số tải CPU/RAM).
*   **Caching Layer**: [Cài đặt Redis Sentinel HA Cluster](../../on-premise/kubernetes/redis/) (Sử dụng Helm Chart để deploy cụm Redis chịu tải cao).
