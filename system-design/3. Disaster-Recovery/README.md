# 🛡️ Khôi Phục Thảm Họa (Disaster Recovery Design)

Phòng chống và khôi phục thảm họa (Disaster Recovery - DR) là chiến lược lập kế hoạch ứng phó để khôi phục lại hệ thống hoạt động bình thường sau sự cố lớn (cháy trung tâm dữ liệu, đứt cáp quang, lỗi xóa nhầm dữ liệu diện rộng,...).

---

## 🎯 Các Chỉ Số DR Cốt Lõi

1. **RPO (Recovery Point Objective - Điểm khôi phục mục tiêu)**:
   * Đo lường lượng dữ liệu tối đa chấp nhận mất mát (tính theo thời gian). 
   * *Ví dụ*: Nếu RPO = 4 giờ, tức là hệ thống phải được backup tối thiểu 4 tiếng một lần, để khi có sự cố, dữ liệu bị mất nhiều nhất chỉ trong vòng 4 tiếng.
   
2. **RTO (Recovery Time Objective - Thời gian khôi phục mục tiêu)**:
   * Đo lường khoảng thời gian tối đa để hệ thống hoạt động trở lại sau khi xảy ra sự cố.
   * *Ví dụ*: Nếu RTO = 1 giờ, tức là quản trị viên hoặc hệ thống tự động phải hồi sinh dịch vụ trở lại trong vòng 1 tiếng.

---

## 🗺️ Các Chiến Lược Triển Khai DR phổ biến

| Chiến lược | RPO / RTO | Chi phí | Cách thực hiện |
| :--- | :---: | :---: | :--- |
| **Backup & Restore** | Cao (Vài giờ đến vài ngày) | Thấp | Sao lưu dữ liệu định kỳ và lưu trữ ở một nơi an toàn khác (Object Storage S3/MinIO). Khi có sự cố thì dựng lại server mới rồi khôi phục dữ liệu lên. |
| **Pilot Light** | Trung bình (Vài chục phút) | Trung bình | Cơ sở dữ liệu luôn chạy song song và đồng bộ liên tục (Replicated). Ứng dụng Backend được tắt sẵn và chỉ khởi động lên khi có sự cố tại Site chính. |
| **Warm Standby** | Thấp (Vài phút) | Cao | Phiên bản thu nhỏ của hệ thống luôn chạy song song ở Site phụ với công suất thấp hơn, sẵn sàng nâng scale khi có sự cố. |
| **Multi-Site (Active-Active)** | Gần như bằng 0 | Rất cao | Hệ thống chạy song song toàn công suất tại hai hoặc nhiều trung tâm dữ liệu khác nhau. User được điều phối thông qua DNS phân vùng địa lý. |

---

## 🔗 Liên Kết Thực Hành DevOps
Tham khảo cách thực hiện quy trình sao lưu và khôi phục hạ tầng trong cụm Kubernetes bằng công cụ Velero:

*   **Setup Velero Client**: [Hướng dẫn cài đặt Velero CLI](../../on-premise/setup/kubernetes/install-velero-client-guide.md)
*   **Backup & Restore Lab**: [Cấu hình Velero backup K8s cluster sang Object Storage MinIO](../../on-premise/setup/kubernetes/setup-velero-minio-backup.md)
