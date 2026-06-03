# 📁 Các Case Studies Thực Tế (System Design Case Studies)

Đây là nơi tổng hợp các bài toán thiết kế kiến trúc hệ thống thực tế từ các bài phỏng vấn kiến trúc sư hệ thống hoặc các kịch bản thực tế khi vận hành dự án lớn.

---

## 🎯 Danh Sách Các Case Studies Để Nghiên Cứu

1. **Thiết Kế Hệ Thống E-Commerce (Thương Mại Điện Tử)**:
   * **Yêu cầu**: Xử lý lượng truy cập lớn trong giờ vàng săn sale, quản lý tồn kho nhất quán (concurrency control), thanh toán bảo mật.
   * **Tài nguyên DevOps liên quan**: [Triển khai dự án Full-stack mẫu](../../on-premise/kubernetes/full-stack/).

2. **Thiết Kế Hệ Thống Giám Sát Sức Khỏe Máy Chủ (Monitoring & Alerting)**:
   * **Yêu cầu**: Thu thập Log, Metric từ hàng nghìn máy chủ theo thời gian thực, tự động cảnh báo qua Slack/Telegram khi CPU > 90%.
   * **Tài nguyên DevOps liên quan**: [Triển khai Prometheus & Grafana](../../on-premise/setup/monitoring/setup-kube-prometheus-guide.md) và [Uptime Kuma](../../on-premise/setup/monitoring/setup-uptime-kuma-guide.md).

3. **Thiết Thiết Kế Hệ Thống Chat Thời Gian Thực (Real-time Messaging)**:
   * **Yêu cầu**: Kết nối WebSocket duy trì liên tục giữa hàng triệu Client, truyền tải tin nhắn độ trễ thấp, lưu trữ lịch sử hội thoại hiệu quả.
   * **Tài nguyên DevOps liên quan**: [Cấu hình Load Balancer hỗ trợ WebSocket/Reverse Proxy](../../on-premise/nginx/).

---

## 🚀 Cách Viết Một Case Study Chuẩn System Architect

Khi bạn phân tích thiết kế cho bất kỳ hệ thống nào, hãy tuân theo quy trình 4 bước sau:

1. **Bước 1: Làm rõ yêu cầu (Scope & Requirements)**:
   * Yêu cầu chức năng (User làm được gì?)
   * Yêu cầu phi chức năng (Quy mô hệ thống? Số lượng DAU/MAU? QPS đọc/ghi? RTO/RPO?).
2. **Bước 2: Thiết kế sơ đồ tổng quan (High-Level Design)**:
   * Vẽ sơ đồ luồng dữ liệu (Data Flow Diagram).
   * Xác định các thành phần chính (Client, DNS, CDN, LB, Web Service, Cache, DB, Message Queue).
3. **Bước 3: Đi sâu thiết kế chi tiết (Deep Dive Design)**:
   * Cách thiết kế Schema Database, khóa chính, đánh Index.
   * Cách tối ưu hóa tầng đệm (Caching Strategy: Cache-Aside, Write-Through).
   * Cơ chế phòng vệ (Rate Limiter, Circuit Breaker).
4. **Bước 4: Xác định giới hạn và đánh đổi (Bottlenecks & Trade-offs)**:
   * Hệ thống sẽ bị nghẽn ở đâu nếu tải tăng gấp 10 lần?
   * Tính nhất quán dữ liệu có bị suy giảm khi phân tán không? (Định lý CAP).
