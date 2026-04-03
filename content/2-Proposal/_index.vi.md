---
title: "Proposal"
date: "2025-10-22"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AntEdu - Nền tảng Học tập & Quản lý Thông minh với AI

---

## 1. Tóm tắt chung

Chúng tôi là một nhóm gồm 5 sinh viên Công nghệ Thông tin đang thực tập, cùng nhau phát triển một dự án mang tên **EduCare**. EduCare là một nền tảng học trực tuyến sáng tạo nhắm đến thị trường Việt Nam, tích hợp **AI đa tầng** để mang đến trải nghiệm giáo dục cá nhân hóa sâu sắc và thông minh. Nền tảng kết hợp các tính năng quản lý học tập truyền thống với các khả năng AI tiên tiến — từ tự động sinh nội dung giáo dục đến trợ lý AI hội thoại.

Mục tiêu của dự án là cách mạng hóa cách sinh viên học trực tuyến bằng cách cung cấp các công cụ AI tự thích ứng theo nhu cầu của từng người học. Hệ thống được xây dựng trên **kiến trúc microservice** gồm 4 dịch vụ độc lập: frontend React, backend API Spring Boot, dịch vụ AI dựa trên FastAPI, và lõi trợ lý AI agentic được vận hành bởi **AWS Bedrock**. Kiến trúc này đảm bảo khả năng mở rộng, dễ bảo trì, và có thể phát triển từng thành phần một cách độc lập.

Các tính năng cốt lõi của EduCare bao gồm: sinh nội dung giáo dục bằng AI (tài liệu, bài tập, lộ trình học) từ tài liệu PDF được tải lên, hệ thống quản lý lớp học toàn diện (giao bài, nộp bài, chấm điểm, điểm danh), và **Slozy** — trợ lý AI thông minh được xây dựng trên kiến trúc **LangGraph ReAct Agent** có khả năng tìm kiếm cơ sở dữ liệu lý thuyết qua RAG, gợi ý lộ trình học cá nhân hóa, và đề xuất bài kiểm tra thông qua hội thoại tự nhiên.

Để đạt được mục tiêu này, nhóm chúng tôi chịu trách nhiệm toàn bộ vòng đời phát triển: từ thiết kế hệ thống, phát triển full-stack, tích hợp AI sử dụng **OpenAI API** và **AWS Bedrock AgentCore**, đến triển khai hạ tầng với **Docker Compose** và **AWS S3** cho lưu trữ file.

---

## 2. Phát biểu Vấn đề

### Vấn đề là gì?

- Các nền tảng học trực tuyến truyền thống áp dụng phương pháp **đồng nhất cho tất cả**, thiếu khả năng thích ứng nội dung và lộ trình học theo nhu cầu cá nhân, tốc độ học, và lỗ hổng kiến thức của từng sinh viên.
- Giáo viên mất quá nhiều thời gian cho các tác vụ lặp đi lặp lại như **tạo bài tập thủ công, biên soạn tài liệu học tập, và xây dựng lộ trình học** từ các tài liệu có sẵn, làm giảm khả năng tập trung vào việc giảng dạy và hướng dẫn thực tế.
- Các giải pháp e-learning hiện tại thiếu **giao diện trò chuyện thông minh** có khả năng hiểu câu hỏi của sinh viên trong ngữ cảnh, truy xuất lý thuyết liên quan, và cung cấp hướng dẫn cá nhân hóa — buộc sinh viên phải tự tìm đường qua các nội dung tĩnh.
- Sự thiếu kết nối giữa **quản lý nội dung, đánh giá, và học tập hỗ trợ bởi AI** trên nhiều công cụ khác nhau tạo ra trải nghiệm phân mảnh cho cả giáo viên và người học.

### Giải pháp

EduCare sử dụng kiến trúc AI đa tầng để giải quyết những thách thức này:

- **Dịch vụ Sinh Nội dung AI (FastAPI + OpenAI)**: Tự động sinh bài tập, tài liệu học tập, và lộ trình học có cấu trúc từ tài liệu PDF được tải lên. Giáo viên chỉ cần tải lên tài liệu khóa học, và AI sẽ tạo ra nội dung giáo dục sẵn sàng sử dụng, được lưu trữ và phân phối qua **AWS S3**.
- **Trợ lý AI Slozy (LangGraph + AWS Bedrock AgentCore)**: Trợ lý AI hội thoại được xây dựng trên kiến trúc **ReAct Agent** với 3 công cụ tích hợp — `search_theory_database` (truy xuất lý thuyết dựa trên RAG qua pgvector), `suggest_roadmap` (sinh lộ trình học cá nhân hóa), và `suggest_quiz` (đề xuất bài kiểm tra thích ứng). Slozy trả về các widget UI đặc biệt mà frontend render thành các thành phần tương tác.
- **Quản lý Học tập Toàn diện (Spring Boot)**: Quản lý toàn bộ vòng đời lớp học bao gồm tạo lớp, phân phối bài tập, xử lý nộp bài, chấm điểm, điểm danh, và tổ chức nội dung phân cấp (Sách → Chương → Mục → Tiểu mục → Bài học).
- **Nền tảng Thống nhất (React + TypeScript)**: Một giao diện responsive duy nhất tích hợp mượt mà tất cả các dịch vụ, mang đến trải nghiệm liền mạch cho cả giáo viên và sinh viên.

### Lợi ích và ROI

- **Năng suất Giáo viên**: Giảm đáng kể thời gian tạo nội dung bằng cách tự động hóa việc sinh bài tập và lộ trình từ tài liệu có sẵn.
- **Học tập Cá nhân hóa**: Trợ lý AI thích ứng theo trình độ của từng sinh viên, cung cấp hướng dẫn theo ngữ cảnh thông qua hội thoại tự nhiên.
- **Quản lý Toàn diện**: Hỗ trợ toàn bộ vòng đời lớp học, loại bỏ nhu cầu sử dụng nhiều công cụ rời rạc.
- **Khả năng Mở rộng**: Kiến trúc microservice cho phép mở rộng độc lập từng thành phần theo yêu cầu.
- **Tương tác Thông minh**: Truy xuất kiến thức dựa trên RAG đảm bảo phản hồi chính xác, phù hợp với chương trình giảng dạy từ trợ lý AI.
- **Hiệu quả Chi phí**: Tận dụng Docker Compose cho phát triển local và các dịch vụ AWS cho production giúp quản lý chi phí hạ tầng hợp lý.

---

## 3. Kiến trúc Giải pháp

### Tổng quan

Gợi ý Người dùng + Ngữ cảnh → **Mô hình Titan Embedding của Bedrock** → **Truy vấn có Cấu trúc** → **Tìm kiếm RDS (PostgreSQL)** → **Xếp hạng & Bộ nhớ đệm** → **Hiển thị Web UI** → **Vòng lặp Phản hồi Người dùng**.  
![Solution Architecture](/images/2-Proposal/4N1D-Architechture.drawio.png)

### Dịch vụ AWS được sử dụng

| Service                   | Function                              |
|---------------------------|---------------------------------------|
| Amazon Route 53           | Định tuyến tên miền                   |
| AWS Certificate Manager   | Chứng chỉ SSL/TLS                     |
| AWS WAF                   | Tường lửa ứng dụng web                |
| Amazon CloudFront         | CDN toàn cầu cho tài sản tĩnh         |
| Amazon API Gateway        | Điểm cuối API RESTful an toàn         |
| AWS Lambda                | Logic phân tích ý định, tìm kiếm và xếp hạng |
| Amazon RDS (PostgreSQL)   | Dữ liệu địa điểm được lập chỉ mục địa lý và bộ nhớ đệm truy vấn |
| Amazon S3                 | Lưu trữ ảnh, nhật ký và tài sản       |
| Amazon Cognito            | Xác thực và ủy quyền người dùng       |
| Amazon Bedrock            | Mô hình Titan embedding và mô hình LLM Claude để phân tích ý định và tóm tắt    |
| Amazon Rekognition        | Kiểm duyệt nội dung driven by AI cho các tải lên của người dùng |
| Amazon Textract           | Trích xuất văn bản từ hình ảnh và tài liệu (menu, biển hiệu, v.v.) |
| Amazon EventBridge        | Phân tích theo lịch và cập nhật huy hiệu |
| Amazon CloudWatch         | Giám sát và ghi nhật ký               |

### Thiết kế Thành phần

- **Frontend**: Ứng dụng web responsive (Next.js, song ngữ VI/EN, UI tìm kiếm kết hợp).
- **Tiếp nhận Dữ liệu**: Gợi ý và ngữ cảnh được xử lý qua API Gateway; các tải lên của người dùng (đánh giá, ảnh) được kiểm duyệt bởi AI của Rekognition.
- **Lưu trữ Dữ liệu**: RDS (PostgreSQL) cho dữ liệu địa điểm với thiết kế cơ sở dữ liệu quan hệ (ERD), các tính năng hỗ trợ vector và lập chỉ mục để tối ưu hóa tốc độ truy vấn; S3 cho ảnh và nhật ký.
- **Xử lý Dữ liệu**: Microservices Lambda xử lý các cuộc gọi Bedrock (mô hình Titan embedding và mô hình LLM Claude), thực thi truy vấn và xếp hạng kết quả.
- **Quản lý Người dùng**: Cognito cho xác thực dựa trên JWT (đăng nhập email/mạng xã hội); người dùng khách truy cập các tính năng hạn chế.
- **Đầu ra**: Hiển thị thẻ địa điểm với tóm tắt được tạo bởi AI, đánh giá, ảnh và CTA (ví dụ: Chỉ đường, Gọi).

---

## 4. Triển khai Kỹ thuật

### Các giai đoạn Triển khai

| Phase | Description                                          | Duration   |
|-------|------------------------------------------------------|------------|
| 1     | Xác định kiến trúc, schema Titan embedding của Bedrock và schema RDS (PostgreSQL) với thiết kế ERD | 2 tuần |
| 2     | Ước tính chi phí và tối ưu hóa chiến lược bộ nhớ đệm  | 1 tuần |
| 3     | Xây dựng backend (Lambda, RDS PostgreSQL, Bedrock Titan embedding, Rekognition)| 3 tuần |
| 4     | Phát triển frontend (Next.js, song ngữ, UI responsive)  | 3 tuần |
| 5     | Kiểm tra và tối ưu hóa cho độ trễ <10s và khả năng mở rộng | 2 tuần |
| 6     | Ra mắt MVP, triển khai qua CI/CD (Terraform, GitLab), thu thập phản hồi    | 2 tuần |

### Yêu cầu Kỹ thuật

- **Thiết bị Edge**: Trình duyệt hiện đại (Chrome, Safari, Firefox) với UI responsive sẵn sàng PWA.
- **Cloud**: AWS Route 53, ACM, WAF, CloudFront, API Gateway, Lambda, RDS (PostgreSQL với hỗ trợ vector), S3, Cognito, Bedrock (Titan embedding, Claude LLM), Rekognition, Textract, EventBridge, CloudWatch.
- **Công cụ & Framework**: Next.js (App Router), TypeScript, Terraform cho infrastructure-as-code, GitLab cho CI/CD.

---

## 5. Lịch trình & Cột mốc

| Period              | Activities                                                  |
|---------------------|-------------------------------------------------------------|
| Trước khi Phát triển (Tháng 0 - 9/2025) | Nghiên cứu các bộ dữ liệu địa điểm tại Thành phố Hồ Chí Minh cho RDS (PostgreSQL) |
| Tháng 1 (10/2025) | Xây dựng backend MVP với mô hình Titan embedding của Bedrock và RDS (PostgreSQL)            |
| Tháng 2 (11/2025)  | Triển khai bộ nhớ đệm, phát triển tích hợp frontend            |
| Tháng 3 (11/2025)  | Ra mắt beta công khai, tối ưu hóa hiệu suất, thu thập phản hồi  |
| Sau khi Ra mắt (12/2025) | Thêm các tính năng nâng cao (ví dụ: xếp hạng dựa trên ML, chế độ ngoại tuyến)|

---

## 6. Ước tính Ngân sách

### Ước tính Thời gian Đầu tư theo Giai đoạn

Chúng tôi ước tính mỗi thành viên sẽ đóng góp khoảng 20 giờ mỗi tuần cho dự án. Với mỗi giai đoạn kéo dài 2 tuần, tổng thời gian đầu tư được ước tính như sau:

| Giai đoạn dự án | Tổng số giờ ước tính |
|:--:|:--:|
| Giai đoạn 1: Thiết lập nền tảng | 200 giờ (5 người * 20 giờ/tuần * 2 tuần) |
| Giai đoạn 2: Xây dựng tính năng lõi | 200 giờ (5 người * 20 giờ/tuần * 2 tuần) |
| Giai đoạn 3: Hoàn thiện và Triển khai | 200 giờ (5 người * 20 giờ/tuần * 2 tuần) |
| **Tổng số giờ** | **600 giờ** |
| **Tổng chi phí tài chính** | **$0** |

### Phân bố Đóng góp cho Dự án

Chi phí tài chính trực tiếp của dự án là không đáng kể và được trang trải hoàn toàn bởi các khoản tín dụng. Sự đóng góp chính đến từ thời gian của đội ngũ và sự hỗ trợ từ AWS.

| Bên tham gia | Đóng góp | % Đóng góp (Tổng giá trị) |
|:--|:--|:--|
| Khách hàng (Chương trình thực tập) | - Cơ hội học tập và kinh nghiệm thực tế. | - |
| Đối tác (Nhóm sinh viên) | - Thời gian và công sức (ước tính 600 giờ). | - |
| AWS | - $200 tín dụng (credit). <br> - Các dịch vụ trong Gói Miễn phí (Free Tier). | 100% (chi phí tài chính) |

### Biện pháp Tối ưu hóa Chi phí
- **Sử dụng Free-Tier**: Tận dụng các miễn phí của AWS cho Lambda, RDS, S3, CloudFront, Rekognition và Cognito để giảm thiểu chi phí.
- **Bộ nhớ đệm Aggressive cho Bedrock**: Đạt tỷ lệ truy cập bộ nhớ đệm cao để giảm chi phí token AI.
- **Xử lý Batch Rekognition**: Các kiểm tra hình ảnh không theo thời gian thực để tiết kiệm chi phí.
- **Sử dụng RDS Tối ưu**: Sử dụng PostgreSQL với lập chỉ mục phù hợp để giảm thiểu chi phí truy vấn.
- **Bộ nhớ đệm Tài sản Tĩnh**: Tối thiểu hóa chi phí truyền dữ liệu đi ra thông qua CloudFront.

### Kiểm soát Ngân sách
- Toàn bộ dự án hoạt động trong phạm vi **$200 tín dụng từ AWS Free Tier**.
- Tất cả chi phí cơ sở hạ tầng được trang trải bởi tín dụng AWS và các dịch vụ free tier.
- Không có chi phí tài chính trực tiếp nào được phát sinh bởi nhóm.

---

## 7. Đánh giá Rủi ro

| Risk                            | Impact | Probability | Mitigation                              |
|---------------------------------|--------|-------------|----------------------------------------|
| Dữ liệu RDS không nhất quán      | High   | Medium      | Xác thực và sao lưu dữ liệu thường xuyên    |
| Phân tích Titan embedding không chính xác (VN/EN)  | Medium | Low         | Mẫu gợi ý được xác định trước, xác thực |
| Khả năng mở rộng dưới tải cao     | Medium | Medium      | Tự động mở rộng serverless, bộ nhớ đệm       |
| Mối quan tâm về quyền riêng tư (dữ liệu vị trí)| High   | Low         | Sự đồng ý rõ ràng của người dùng, các truy vấn ẩn danh |
| Vượt quá ngân sách               | High   | Medium      | Giám sát chặt chẽ, sử dụng free tiers, cảnh báo chi phí |
| Phụ thuộc vào dịch vụ AWS Bedrock (Titan, Claude)  | High   | Low         | Cơ chế dự phòng thay thế, kết quả được bộ nhớ đệm |
| Phụ thuộc vào nền tảng GitLab    | Medium | Low         | Sao lưu thường xuyên, tùy chọn repository thay thế |
| Chất lượng dữ liệu ban đầu       | Medium | Medium      | Quy trình xác thực dữ liệu, tích hợp phản hồi người dùng |

**Kế hoạch dự phòng**: Sử dụng kết quả RDS được bộ nhớ đệm hoặc dự phòng JSON cục bộ cho các demo. Triển khai giới hạn tốc độ dựa trên IP cho người dùng chưa xác thực. Giám sát chi phí AWS hàng tuần để ngăn chặn vượt quá ngân sách.

---

## 8. Kết quả Dự kiến

### Tiêu chí Thành công Dự án

Để đảm bảo dự án MapVibe thành công, chúng tôi đặt ra các tiêu chí thành công cụ thể, có thể đo lường được như sau:

- **Hoàn thành triển khai hạ tầng**: Triển khai thành công và an toàn toàn bộ kiến trúc hệ thống lên AWS, bao gồm các dịch vụ đã định sẵn như VPC, Subnet, RDS, Lambda, S3, CloudFront, API Gateway, và Cognito.
- **Tính năng tìm kiếm cốt lõi hoạt động hiệu quả**: Tính năng tìm kiếm bằng ngôn ngữ tự nhiên sử dụng AWS Bedrock (mô hình Titan embedding, mô hình LLM Claude) và RDS PostgreSQL phải có khả năng hiểu và trả về kết quả phù hợp, có ý nghĩa cho ít nhất **90%** các câu truy vấn thông thường từ người dùng.
- **Tối ưu chi phí**: Toàn bộ chi phí sử dụng dịch vụ AWS trong suốt quá trình phát triển và triển khai ban đầu không vượt quá 200 đô la tín dụng từ gói AWS Free Tier.
- **Ra mắt sản phẩm (MVP)**: Hoàn thiện và cho ra mắt phiên bản sản phẩm khả dụng tối thiểu (MVP) của website MapVibe cho nhóm người dùng mục tiêu (GenZ) trên cả **desktop và nền tảng iOS, Android**. Ngoài ra, còn có **trang tổng quan (Dashboard) dành cho quản trị viên** để quản lý nội dung và người dùng.
- **Thu thập phản hồi tích cực**: Sau khi ra mắt, thu thập được ít nhất 50 đề xuất địa điểm mới từ người dùng trong tháng đầu tiên, cho thấy sự tương tác và hưởng ứng của cộng đồng.

### Cải tiến Kỹ thuật
- **Tìm kiếm Trò chuyện dựa trên Tâm trạng**: Hỗ trợ ngôn ngữ tự nhiên cho tiếng Việt và tiếng Anh với độ trễ <10s, powered by Bedrock (mô hình Titan embedding và mô hình LLM Claude), hiểu được cảm xúc và tâm trạng của người dùng.
- **Tóm tắt AI**: Tổng quan địa điểm được tạo bởi Bedrock, làm mới mỗi 7 ngày hoặc sau 10 đánh giá mới.
- **Khả năng mở rộng**: Kiến trúc serverless với phân phối CDN toàn cầu qua CloudFront.
- **Kiểm duyệt**: AI của Rekognition đảm bảo nội dung do người dùng tạo an toàn (đánh giá, ảnh).
- **Cơ sở dữ liệu Quan hệ**: PostgreSQL với lập chỉ mục được tối ưu hóa cho hiệu suất truy vấn nhanh.

### Giá trị Dài hạn
- **Cá nhân hóa**: Xếp hạng lại dựa trên ML và phân tích hành vi người dùng dựa trên tâm trạng và sở thích.
- **Hỗ trợ Ngoại tuyến**: PWA để liệt kê các địa điểm ngoại tuyến.
- **Khả năng mở rộng**: Tiềm năng tích hợp với các hệ thống đặt phòng nội bộ.
- **Mở rộng Ngữ cảnh**: Đề xuất dựa trên thời tiết, sự kiện, xu hướng xã hội và cảm xúc người dùng.

---

### Tài liệu đính kèm / Tham khảo
- [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=b574c31813807522d949cf5adca64b41612e12c7)
- [GitLab Repository](https://gitlab.com/4n1d/mapvibe)

## 9. QUAN TRỌNG: Proposal bản docs
- [Proposal Docs Version](https://docs.google.com/document/d/1-wM11mgTNaL8gvbMjAmukW9ZKzqGtHMp/edit)
- [Related Documents](https://drive.google.com/drive/u/0/folders/1cdBYgwzBA4Vl9ww_fCPXmbsKToTKgH4L)

