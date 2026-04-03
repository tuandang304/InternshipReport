---
title: "Proposal — Slothub"
date: "2026-04-03"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SLOTHUB — Nền tảng hỗ trợ học tập và giảng dạy cho giáo viên và học sinh
*(Quản lý lớp học, bài tập, lịch học, tài liệu và trợ lý AI tích hợp Amazon Bedrock)*

---

## 1. Tóm tắt chung

**Slothub** là một hệ thống web hướng tới giáo viên, học sinh và quản trị viên, nhằm đồng bộ hoạt động dạy–học trên một nền tảng duy nhất: từ quản lý lớp, thời khóa biểu, điểm danh, giao và chấm bài, đến tài liệu, thông báo và **trợ lý học tập thông minh** (gợi ý lộ trình ôn tập, trắc nghiệm, trả lời theo kiến thức trong cơ sở dữ liệu giáo trình). Dự án được triển khai theo hướng **đám mây AWS** tại khu vực **ap-southeast-1 (Singapore)** với thiết kế **đa vùng sẵn sàng (Multi-AZ)** nhằm tăng tính sẵn sàng và khả năng phục hồi.

**Frontend** được xây dựng bằng **React, TypeScript và Vite**, giao diện responsive, hỗ trợ đa ngôn ngữ (vi/en). **Backend** dùng **Java Spring Boot** (REST API, bảo mật OAuth2/JWT), kết nối **Amazon RDS for PostgreSQL** cho dữ liệu quan hệ (người dùng, lớp, bài giao, bài nộp, thời khóa biểu, điểm danh, phiên chat…). **Dịch vụ AI** gồm một **API FastAPI** (Python) xử lý sinh đề, lộ trình, trích xuất tài liệu, tích hợp **Amazon S3** cho tệp tài liệu; và **Amazon Bedrock AgentCore** triển khai agent **gia sư AI (Slozy)** với các công cụ tìm kiếm lý thuyết (vector trên PostgreSQL), gợi ý lộ trình và quiz — phù hợp với kiến trúc “Fargate Backend + Fargate AI” và **Amazon Bedrock** trong sơ đồ hệ thống.

Mục tiêu là giảm thiểu gánh nặng quản trị hành chính cho giáo viên, giúp học sinh chủ động ôn tập có định hướng, đồng thời tận dụng các dịch vụ AWS (Cognito, WAF, Amplify, ALB, Fargate, ECR, RDS, S3, Bedrock, CloudWatch, GuardDuty…) để triển khai và vận hành có kiểm soát.

---

## 2. Phát biểu vấn đề

### Vấn đề là gì?

- Giáo viên thường phải dùng nhiều công cụ rời (bảng điểm, nhóm chat, file tài liệu, form bài tập), dễ mất thời gian và thiếu nhất quán dữ liệu.
- Học sinh khó theo dõi tiến độ, lịch học và bài tập; thiếu phản hồi cá nhân hóa kịp thời ngoài giờ lên lớp.
- Các nền tảng học tập truyền thống ít tích hợp **AI an toàn trong ngữ cảnh giáo dục** (hướng dẫn từng bước, không thay thế hoàn toàn việc tự suy nghĩ của học sinh).
- Yêu cầu **bảo mật, mở rộng và khả dụng cao** khi triển khai thật cho nhiều lớp, nhiều người dùng đồng thời.

### Giải pháp

Slothub tập trung dữ liệu và quy trình trên một stack thống nhất: API Spring Boot + PostgreSQL RDS; dịch vụ AI tách riêng (container) gọi mô hình **Amazon Bedrock** và lưu trữ tài liệu trên **S3**; trợ lý gia sư **AgentCore** truy vấn nội dung lý thuyết từ cơ sở dữ liệu (tìm kiếm vector) thay vì “bịa” kiến thức. Người dùng xác thực qua **Amazon Cognito**; truy cập web qua **Route 53**, **AWS WAF**, **AWS Amplify** (hosting frontend) và **ACM** cho HTTPS; **Application Load Balancer** phân phối tới **AWS Fargate**; hình ảnh container lưu tại **Amazon ECR**.

### Lợi ích và ROI

- **Tập trung**: Một lộ trình học tập và quản lý lớp, thời khóa biểu, bài tập trên một nền tảng.
- **Cá nhân hóa**: Lộ trình ôn tập, quiz và chat gia sư hỗ trợ theo từng môn/ngữ cảnh.
- **Tin cậy vận hành**: Multi-AZ cho RDS PostgreSQL, Fargate trải trên nhiều AZ, giám sát **CloudWatch**, bảo mật **GuardDuty**.
- **Khả năng mở rộng**: Container hóa, CI/CD từ GitHub Actions, tách dịch vụ AI và backend.
- **Kiểm soát chi phí**: Tối ưu theo tần suất sử dụng (Fargate, Bedrock), caching và giới hạn tài nguyên phù hợp môi trường học tập/thực tập.

---

## 3. Kiến trúc giải pháp

### Tổng quan

Luồng chính: **Người dùng → Cognito (xác thực) → Route 53 → WAF → Amplify (frontend) / ACM**; request API đi qua **Internet Gateway → ALB → Fargate (Backend hoặc AI)**; dữ liệu lưu tại **RDS PostgreSQL (Primary/Standby, Multi-AZ)** và tệp tại **S3 (documents bucket)**; dịch vụ AI tương tác **Amazon Bedrock (AgentCore)**; triển khai **GitHub Actions** đẩy image lên **ECR** và cập nhật Fargate; **CloudWatch** và **GuardDuty** phục vụ giám sát và an ninh.

![Kiến trúc giải pháp Slothub](/images/2-Proposal/slothub-architecture.png)

*(Đặt file sơ đồ kiến trúc tại `themes/.../static/images/2-Proposal/slothub-architecture.png` hoặc tương đương theo cấu hình Hugo của báo cáo.)*

### Dịch vụ AWS được sử dụng

| Dịch vụ | Vai trò |
|--------|---------|
| **Amazon Cognito** | Xác thực và phân quyền người dùng |
| **Amazon Route 53** | DNS và định tuyến tên miền |
| **AWS WAF** | Bảo vệ ứng dụng web khỏi các khai thác phổ biến |
| **AWS Amplify** | Hosting và quản lý frontend |
| **AWS Certificate Manager (ACM)** | Chứng chỉ SSL/TLS |
| **Amazon VPC + Internet Gateway** | Mạng riêng, kết nối internet có kiểm soát |
| **Application Load Balancer (ALB)** | Cân bằng tải tới dịch vụ container |
| **Amazon ECS on AWS Fargate** | Chạy backend (Spring Boot) và dịch vụ AI (FastAPI) không quản lý server |
| **Amazon ECR** | Lưu trữ Docker image backend và AI |
| **Amazon RDS (PostgreSQL)** | Cơ sở dữ liệu quan hệ, cấu hình Primary/Standby Multi-AZ |
| **Amazon S3** | Bucket tài liệu / tệp đính kèm |
| **Amazon Bedrock (AgentCore)** | Agent gia sư AI và mô hình ngôn ngữ |
| **Amazon CloudWatch** | Log và giám sát tài nguyên |
| **Amazon GuardDuty** | Phát hiện mối đe dọa bảo mật |

### Thiết kế thành phần

- **Frontend (Amplify)**: React + TypeScript + Vite; giao diện giáo viên, học sinh, quản trị; tích hợp lịch học, bài tập, tài liệu, thống kê, chat, roadmap, cài đặt tài khoản.
- **Backend API (Fargate)**: Spring Boot — REST cho lớp học, người dùng, bài giao, bài nộp, thời khóa biểu, điểm danh, phiên chat, thông báo, xuất báo cáo; JWT/OAuth2 resource server.
- **Dịch vụ AI (Fargate)**: FastAPI — sinh bài tập/lộ trình, xử lý tài liệu, upload liên quan S3; gọi backend dữ liệu khi cần.
- **Agent gia sư (Bedrock AgentCore)**: Agent Slozy — hướng dẫn từng bước, công cụ tìm lý thuyết (vector trên PostgreSQL), gợi ý roadmap/quiz qua API nội bộ.
- **Dữ liệu**: RDS PostgreSQL cho metadata và nghiệp vụ; S3 cho object lớn; tách subnet public/private theo thực hành tốt trên AWS.

---

## 4. Triển khai kỹ thuật

### Các giai đoạn triển khai

| Giai đoạn | Nội dung | Thời gian gợi ý |
|-----------|----------|------------------|
| 1 | Chuẩn hóa kiến trúc AWS (VPC, ALB, Fargate, ECR, RDS PostgreSQL Multi-AZ, S3, Cognito), IaC/CI cơ bản | 2–3 tuần |
| 2 | Hoàn thiện schema RDS, API Spring Boot, bảo mật và tích hợp Cognito | 3–4 tuần |
| 3 | Triển khai dịch vụ AI FastAPI, pipeline S3, kết nối Bedrock | 2–3 tuần |
| 4 | Triển khai AgentCore (Slozy), nối công cụ tới PostgreSQL và API AI | 2 tuần |
| 5 | Frontend build production, Amplify, WAF, ACM, Route 53 | 2 tuần |
| 6 | Kiểm thử tải, quan sát CloudWatch, tinh chỉnh chi phí và bảo mật (GuardDuty) | 1–2 tuần |

### Yêu cầu kỹ thuật

- **Phía máy khách**: Trình duyệt hiện đại; giao diện responsive.
- **Backend**: Java 17, Spring Boot 3.x, PostgreSQL driver, MapStruct, Spring Security.
- **AI**: Python 3.x, FastAPI, tích hợp AWS SDK (S3, Bedrock theo từng mô-đun).
- **Agent**: Bedrock AgentCore SDK, LangGraph/LangChain, container hóa theo `Dockerfile` của dự án.
- **CI/CD**: GitHub Actions build Docker, push ECR, cập nhật ECS/Fargate.

---

## 5. Lịch trình và cột mốc

| Giai đoạn | Hoạt động |
|-----------|-----------|
| Chuẩn bị | Thống nhất ERD/API, quyền vai trò (giáo viên, học sinh, admin), chính sách dữ liệu |
| Phát triển lõi | Lớp học, bài tập, lịch, điểm danh, người dùng trên RDS |
| Tích hợp AI | FastAPI + S3 + Bedrock; AgentCore gia sư |
| Triển khai | ECR, Fargate, ALB, Amplify, Cognito, WAF |
| Vận hành | Giám sát, backup RDS, xử lý sự cố |

---

## 6. Ước tính ngân sách

### Thời gian đầu tư (tham khảo)

Ước tính nhóm phát triển có thể phân bổ theo sprint (ví dụ mỗi thành viên 15–20 giờ/tuần). Tổng giờ phụ thuộc quy mô nhóm và phạm vi MVP; có thể mô phỏng theo mẫu MapVibe (ví dụ ~600 giờ cho ba giai đoạn 2 tuần × 5 người nếu áp dụng đội 5 người).

### Chi phí đám mây

- **RDS PostgreSQL Multi-AZ**, **Fargate**, **ALB**, **data transfer**, **Bedrock** và **S3** là các hạng mục cần theo dõi sát bằng **CloudWatch** và **AWS Cost Explorer**.
- Cân nhắc tầng **free tier / credits** cho môi trường thử nghiệm; tách môi trường dev/staging/prod để tránh chi phí không cần thiết.

### Tối ưu chi phí

- **Right-sizing** task CPU/RAM Fargate; tự động scale theo tải.
- **Connection pooling** và chỉ mục DB hợp lý trên RDS.
- **Giới hạn token** và cache kết quả Bedrock khi phù hợp.
- **Lifecycle policy** trên S3 nếu có tệp tạm.

---

## 7. Đánh giá rủi ro

| Rủi ro | Mức độ ảnh hưởng | Khả năng | Giảm thiểu |
|--------|------------------|----------|------------|
| Lỗi đồng bộ dữ liệu giữa backend và AI | Cao | Trung bình | API contract, idempotent upload, transaction rõ ràng |
| Chi phí Bedrock vượt dự kiến | Cao | Trung bình | Giới hạn quota, cache, prompt ngắn gọn |
| Sự cố RDS (failover) | Cao | Thấp | Multi-AZ, backup tự động, kiểm tra DR định kỳ |
| Rủi ro bảo mật (API lộ, bucket S3) | Cao | Trung bình | IAM least privilege, WAF, mã hóa, GuardDuty |
| Phụ thuộc dịch vụ AWS | Trung bình | Thấp | Thiết kế tách lớp, abstraction cho storage/AI |
| Chất lượng phản hồi AI không phù hợp lứa tuổi | Trung bình | Trung bình | Prompt, kiểm duyệt nội dung, giới hạn chủ đề học thuật |

---

## 8. Kết quả dự kiến

### Tiêu chí thành công

- Triển khai **end-to-end** theo kiến trúc: Cognito, Amplify, ALB, Fargate, ECR, **RDS PostgreSQL**, S3, Bedrock AgentCore, CloudWatch.
- **MVP** cho phép: quản lý lớp, giao bài, nộp bài, lịch và điểm danh cơ bản, tài liệu, và ít nhất một luồng AI (chat gia sư hoặc lộ trình/quiz).
- **Độ trễ** API chấp nhận được cho lớp học trực tuyến (mục tiêu cụ thể do nhóm đo trong giai đoạn kiểm thử).
- **An toàn**: HTTPS, WAF, không lộ secret; log tập trung CloudWatch.

### Giá trị dài hạn

- Mở rộng ngân hàng câu hỏi, báo cáo học tập nâng cao, tích hợp thêm công cụ AI có kiểm soát.
- Khả năng mở rộng theo trường/lớp nhờ kiến trúc container và managed database.

---

## 9. Tài liệu tham khảo

- [AWS Pricing Calculator](https://calculator.aws/)
- Tài liệu Amazon Bedrock AgentCore và Amazon ECS Fargate
- Mã nguồn nội bộ: thư mục `frontend`, `backend`, `AI`, `agent-core` trong workspace dự án
