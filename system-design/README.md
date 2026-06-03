# 🏛️ Thiết Kế Hệ Thống (System Design for System Architect)

Chào mừng bạn đến với chuyên mục **Thiết Kế Hệ Thống (System Design)**. Thư mục này được thiết kế đặc biệt dành riêng cho lộ trình học tập và làm việc hướng tới vị trí **System Architect (Kiến trúc sư hệ thống)**.

Trong System Architecture, cơ sở hạ tầng (Kubernetes, Docker, Cloud, CI/CD) đóng vai trò là **phần vật lý thực thi (Physical Implementation)**, còn System Design đóng vai trò là **bản vẽ kiến trúc (Blueprint)**. Thư mục này sẽ giúp bạn kết nối hai mảnh ghép đó lại với nhau.

---

## 🗺️ Bản Đồ Kiến Thức System Design

Dưới đây là sơ đồ luồng đi tiêu chuẩn của một hệ thống quy mô lớn (High-Level Architecture) thể hiện mối liên quan giữa Thiết kế và Triển khai:

```mermaid
graph TD
    User([Người dùng]) --> |HTTPS| DNS[Route53 / Cloudflare DNS]
    DNS --> CDN[CDN: Cloudfront / Akamai]
    DNS --> WAF[Web Application Firewall - WAF]
    WAF --> LB[Load Balancer: Nginx / ALB]
    
    subgraph K8s Cluster [Kubernetes Control Plane & Workers]
        LB --> Ingress[Ingress Controller]
        Ingress --> Gateway[API Gateway / BFF Service]
        Gateway --> Auth[Authentication Service: OAuth2/OIDC]
        Gateway --> Microservices[Microservices Pods]
        
        Microservices --> Cache[(Redis Cache Cluster)]
        Microservices --> DB[(Database: MariaDB StatefulSet)]
        Microservices --> MQ[(Message Queue: Kafka / RabbitMQ)]
    end
    
    MQ --> AsyncWorker[Async Workers]
    
    subgraph Storage & Backup
        DB --> StorageClass[NFS PV/PVC Storage]
        StorageClass --> BackupSystem[Velero Backup to S3 / MinIO]
    end

    style K8s Cluster fill:#f5f8ff,stroke:#4f46e5,stroke-width:2px
    style Storage & Backup fill:#fefafd,stroke:#db2777,stroke-width:1px
```

---

## 📁 Cấu Trúc Các Chuyên Đề

| Chuyên Đề | Nội Dung Mô Tả | Tài Nguyên Hiện Thực Hóa (DevOps) |
| :--- | :--- | :--- |
| 📁 [1. High-Availability](./1.%20High-Availability/README.md) | Thiết kế tính sẵn sàng cao, chịu lỗi (Fault-Tolerance), thiết kế cụm Active-Active, Active-Passive. | [Kubernetes Deployment/StatefulSet](../on-premise/kubernetes/deployment/), [Load Balancer Nginx](../on-premise/kubernetes/load-balancer/nginx/k8s-loadbalancer.conf) |
| 📁 [2. Scaling](./2.%20Scaling/README.md) | Chiến lược co giãn hệ thống, Load Balancing, Caching, CDN, thiết kế Rate Limiter phòng chống DDOS. | [HPA Manifest](../on-premise/kubernetes/hpa/), [Redis Sentinel](../on-premise/kubernetes/redis/) |
| 📁 [3. Disaster-Recovery](./3.%20Disaster-Recovery/README.md) | Chiến lược sao lưu và phục hồi thảm họa, đo lường các chỉ số RTO & RPO, mô hình Multi-region. | [Velero & MinIO Backup setup](../on-premise/setup/kubernetes/setup-velero-minio-backup.md) |
| 📁 [4. Security-Architecture](./4.%20Security-Architecture/README.md) | Kiến trúc mạng bảo mật bảo vệ tài nguyên hạ tầng, phân vùng mạng (VPC Subnets), IAM, Secret Management. | [AWS IAM Configuration Guides](../cloud/aws/services/2.%20IAM/), [Secret Management](../on-premise/kubernetes/secret/) |
| 📁 [5. Database-Architecture](./5.%20Database-Architecture/README.md) | Thiết kế lưu trữ dữ liệu, replication (Master-Slave, Multi-Master), CSDL SQL vs NoSQL, Sharding và Partitioning. | [MariaDB StatefulSet](../on-premise/kubernetes/statefulset/), [Persistent Volume & NFS Storage](../on-premise/kubernetes/storage/) |
| 📁 [6. Case-Studies](./6.%20Case-Studies/README.md) | Phân tích thiết kế chi tiết cho các bài toán thực tế (Hệ thống E-commerce, Chat thời gian thực, Hệ thống thông báo). | [Dự án mẫu Fullstack](../on-premise/kubernetes/full-stack/) |

---

## 🎯 Cách Học & Áp Dụng Cho System Architect

1. **Đọc hiểu Blueprint (Thiết kế khái niệm)**: Bắt đầu từ mỗi chuyên đề trong `system-design/` để nắm vững nguyên lý hoạt động, ưu nhược điểm và các điểm trade-off (sự đánh đổi) của từng giải pháp.
2. **Đối chiếu hiện thực (Physical Implementation)**: Click vào các liên kết sang phần `on-premise/` hoặc `cloud/` để xem cách triển khai thực tế bằng các file cấu hình YAML, shell script hoặc Terraform.
3. **Thực hành vẽ sơ đồ & viết tài liệu**: Tự vẽ lại sơ đồ kiến trúc hệ thống bạn đang làm việc bằng công cụ Mermaid hoặc Draw.io rồi lưu trữ trực tiếp vào thư mục `6. Case-Studies`.
