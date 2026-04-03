# Báo cáo Tổng quan Dự án EduCare

---

## 1. Giới thiệu

**EduCare** là nền tảng học trực tuyến dành cho thị trường Việt Nam, tích hợp AI để hỗ trợ học tập cá nhân hóa. Hệ thống gồm 4 dịch vụ độc lập, dùng chung một cơ sở dữ liệu PostgreSQL.

---

## 2. Kiến trúc Hệ thống

| Dịch vụ | Công nghệ | Cổng | Vai trò |
|---------|-----------|------|---------|
| `frontend/` | React 18 + TypeScript + Vite + Tailwind | 5173 | Giao diện người dùng (SPA) |
| `backend/` | Java 17 + Spring Boot 3.5 | 8080 | REST API chính |
| `AI/` | Python 3.12 + FastAPI + OpenAI | 8000 | Sinh tài liệu, bài tập, lộ trình AI |
| `agent-core/` | Python 3.10 + LangGraph + AWS Bedrock | — | Trợ lý AI Slozy |

**Database:** PostgreSQL (`EDU_CARE`), deploy qua Docker Compose.

---

## 3. Các Module Chính

### Backend (Spring Boot)

- **Xác thực:** JWT + Spring OAuth2 Resource Server, hỗ trợ logout (blacklist token), reset mật khẩu qua Gmail SMTP.
- **Domain chính:**
  - Người dùng: `User`, `Teacher`, `Student`
  - Lớp học: `Classroom`, `ClassMember`
  - Bài tập: `Assignment`, `Submission`
  - Ngân hàng câu hỏi: `Question`, `QuestionBank`
  - Nội dung: `Book` → `Chapter` → `Section` → `Subsection` → `Lesson`
  - Hỗ trợ: `Timetable`, `Attendance`, `Roadmap`, `AIChatSession`

### AI Service (FastAPI)

- Sinh bài tập tự động từ tài liệu PDF (upload lên S3).
- Tạo lộ trình học (`/roadmap`), quản lý nội dung sách/môn/lớp.
- Proxy tin nhắn chat đến agent Slozy.
- Dùng **OpenAI API** để embedding & generation.
- Lưu trữ file trên **AWS S3** (`educare-upload-2026`, region `ap-southeast-1`).
- Kết nối DB trực tiếp qua `psycopg2`, tự chạy migration lúc khởi động.

### Agent-Core — Slozy AI Tutor

- Kiến trúc **LangGraph ReAct Agent**, deploy trên **AWS Bedrock AgentCore**.
- 3 công cụ tích hợp:
  - `search_theory_database` — tìm kiếm lý thuyết qua pgvector (RAG)
  - `suggest_roadmap` — gợi ý lộ trình học
  - `suggest_quiz` — gợi ý bài kiểm tra
- Trả về widget đặc biệt (`>>>[UI_WIDGET:TYPE|args]<<<`) để frontend render thành UI component.
- Local dev dùng RAM (`MemorySaver`); production dùng `AgentCoreMemorySaver`.

### Frontend (React)

- Giao tiếp với backend qua `api.ts`, đọc URL từ biến môi trường (`VITE_API_BASE_URL`, `VITE_FAST_API_BASE_URL`).
- Quản lý auth bằng `AuthContext`, token lưu ở `localStorage`.
- Tự động đăng xuất khi nhận sự kiện `educare:session-expired` (401).

---

## 4. Hạ tầng & Triển khai

| Thành phần | Công nghệ |
|-----------|-----------|
| File storage | AWS S3 (`ap-southeast-1`) |
| AI Agent | AWS Bedrock AgentCore |
| Database local | Docker Compose (PostgreSQL :5432 + pgAdmin :5050) |
| Build tools | Maven (backend), npm/Vite (frontend), uv (AI services) |

**pgAdmin:** http://localhost:5050 — `admin@educare.com` / `admin`

---

## 5. Luồng Hoạt Động Chính

```
Người dùng → Frontend (React)
    → Backend (Spring Boot)  : xác thực, quản lý dữ liệu
    → AI Service (FastAPI)   : sinh nội dung AI, upload file
        → OpenAI API
        → AWS S3
    → Agent-Core (Slozy)     : chat AI cá nhân hóa
        → AWS Bedrock AgentCore
        → pgvector (RAG)
```

---

## 6. Biến Môi Trường

### Frontend (`frontend/.env`)
```
VITE_API_BASE_URL=http://localhost:8080/api
VITE_FAST_API_BASE_URL=http://localhost:8000/api/v1
```

### Backend (`backend/.env`)
```
DB_HOST, DB_NAME, DB_USERNAME, DB_PASSWORD
JWT_SIGNER_KEY
AWS_ACCESS_KEY, AWS_SECRET_KEY
MAIL_USERNAME, MAIL_PASSWORD
```

### AI / Agent-Core (`.env` trong mỗi thư mục)
```
DB_HOST, DB_NAME, DB_USERNAME, DB_PASSWORD
AWS_ACCESS_KEY, AWS_SECRET_KEY, AWS_REGION, AWS_BUCKET_NAME
OPENAI_API_KEY
LOCAL_DEV=1   # agent-core: dùng in-memory checkpointer thay vì AgentCore
```

---

## 7. Điểm Nổi Bật

- Tích hợp AI đa tầng: sinh tài liệu (OpenAI), trợ lý hội thoại (Bedrock + LangGraph).
- Hệ thống nội dung phân cấp sâu: Sách → Chương → Mục → Tiểu mục → Bài học.
- Kiến trúc microservice, dễ mở rộng từng thành phần độc lập.
- Hỗ trợ đầy đủ vòng đời lớp học: tạo lớp, giao bài, nộp bài, chấm điểm, điểm danh.
- Frontend render UI động từ response của AI agent (widget protocol).

---

*Ngày lập báo cáo: 03/04/2026*