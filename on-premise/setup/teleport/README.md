# Cài Đặt và Cấu Hình Teleport (Teleport Setup)

Thư mục này chứa tài liệu và cấu hình phục vụ việc triển khai cổng quản lý truy cập máy chủ Teleport tập trung trên môi trường On-Premise.

## Danh Sách Hướng Dẫn

1. **[Bài 5: Triển khai công cụ quản lý truy cập máy chủ (Teleport)](./05-deploy-server-access-management.md)**
   * Đánh giá ưu/nhược điểm khi chạy Teleport On-Premise.
   * Tạo bản ghi DNS, thiết lập Nginx Load Balancer.
   * Cấu hình Port Forwarding bằng Cloudflare Tunnel (Zero Trust).
   * Tải và cài đặt các tệp tin thực thi binary của Teleport.
   * Khởi tạo tệp tin cấu hình `teleport.yaml` cho Teleport Server.
   * Cấu hình file systemd service quản lý dịch vụ Teleport (`teleport.service`).


