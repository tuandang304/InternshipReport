---
title: "Proposal"
date: "2025-10-22"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Slothub - Agentic Learning & Management Platform

---

## 1. Executive Summary

We are a team of 5 Information Technology students working together to develop a project called **EduCare**. EduCare is an innovative online learning platform targeting the Vietnamese market, integrating **multi-layered AI** to deliver a deeply personalized and intelligent educational experience. The platform combines traditional learning management features with cutting-edge AI capabilities — from automated content generation to a conversational AI tutor.

The project's goal is to revolutionize the way students learn online by providing AI-powered tools that adapt to each learner's needs. The system is built on a **microservice architecture** consisting of 4 independent services: a React frontend, a Spring Boot backend API, a FastAPI-based AI service, and an agentic AI tutor core powered by **AWS Bedrock**. This architecture ensures scalability, maintainability, and the ability to evolve each component independently.

EduCare's core features include: AI-generated educational content (documents, exercises, learning roadmaps) from uploaded PDF materials, a comprehensive classroom management system (assignments, submissions, grading, attendance), and **Slozy** — an intelligent AI tutor built with **LangGraph ReAct Agent** architecture that can search theory databases via RAG, suggest personalized learning roadmaps, and recommend quizzes through natural conversation.

To achieve this goal, our team is responsible for the entire development lifecycle: from system design, full-stack development, AI integration using **OpenAI API** and **AWS Bedrock AgentCore**, to infrastructure deployment with **Docker Compose** and **AWS S3** for file storage.

---

## 2. Problem Statement

### What's the Problem?

- Traditional online learning platforms offer a **one-size-fits-all** approach, lacking the ability to adapt content and learning paths to individual student needs, learning pace, and knowledge gaps.
- Teachers spend excessive time on repetitive tasks such as **manually creating exercises, generating learning materials, and building study roadmaps** from existing documents, reducing their capacity for actual teaching and mentoring.
- Existing e-learning solutions lack **intelligent conversational interfaces** that can understand student queries in context, retrieve relevant theory, and provide personalized guidance — forcing students to navigate through static content on their own.
- The disconnect between **content management, assessment, and AI-assisted learning** across multiple tools creates a fragmented experience for both educators and learners.

### The Solution

EduCare employs a multi-layered AI architecture to address these challenges:

- **AI Content Generation Service (FastAPI + OpenAI)**: Automatically generates exercises, learning materials, and structured roadmaps from uploaded PDF documents. Teachers simply upload course materials, and the AI produces ready-to-use educational content, stored and served via **AWS S3**.
- **Slozy AI Tutor (LangGraph + AWS Bedrock AgentCore)**: A conversational AI tutor built on **ReAct Agent** architecture with 3 integrated tools — `search_theory_database` (RAG-based theory retrieval via pgvector), `suggest_roadmap` (personalized learning path generation), and `suggest_quiz` (adaptive quiz recommendations). Slozy returns special UI widgets that the frontend renders as interactive components.
- **Comprehensive Learning Management (Spring Boot)**: Full-featured classroom lifecycle management including class creation, assignment distribution, submission handling, grading, attendance tracking, and hierarchical content organization (Book → Chapter → Section → Subsection → Lesson).
- **Unified Platform (React + TypeScript)**: A single responsive interface that seamlessly integrates all services, providing a cohesive experience for both teachers and students.

### Benefits and ROI

- **Teacher Productivity**: Dramatically reduces time spent on content creation by automating exercise and roadmap generation from existing materials.
- **Personalized Learning**: AI-driven tutoring adapts to each student's level, providing contextual guidance through natural conversation.
- **Comprehensive Management**: End-to-end classroom lifecycle support eliminates the need for multiple disconnected tools.
- **Scalability**: Microservice architecture allows independent scaling of each component based on demand.
- **Intelligent Interaction**: RAG-based knowledge retrieval ensures accurate, curriculum-aligned responses from the AI tutor.
- **Cost Efficiency**: Leveraging Docker Compose for local development and AWS services for production keeps infrastructure costs manageable.stems.

---

## 3. Solution Architecture

### Overview

User Prompt + Context → **Bedrock Titan Embedding Model** → **Structured Query** → **RDS (PostgreSQL) Search** → **Rank & Cache** → **Web UI Display** → **User Feedback Loop**.  
![Solution Architecture](/images/2-Proposal/4N1D-Architechture.drawio.png)

### AWS Services Used

| Service                   | Function                              |
|---------------------------|---------------------------------------|
| Amazon Route 53           | Domain routing                        |
| AWS Certificate Manager   | SSL/TLS certificates                  |
| AWS WAF                   | Web application firewall              |
| Amazon CloudFront         | Global CDN for static assets          |
| Amazon API Gateway        | Secure RESTful API endpoints           |
| AWS Lambda                | Intent parsing, search, and ranking logic |
| Amazon RDS (PostgreSQL)   | Geo-indexed place data and query caching |
| Amazon S3                 | Storage for photos, logs, and assets   |
| Amazon Cognito            | User authentication and authorization |
| Amazon Bedrock            | Titan embedding model and Claude LLM model for intent parsing and summarization |
| Amazon Rekognition        | AI-driven content moderation for user uploads |
| Amazon Textract           | Extract text from images and documents (menus, signs, etc.) |
| Amazon EventBridge        | Scheduled analytics and badge updates  |
| Amazon CloudWatch         | Monitoring and logging                |

### Component Design

- **Frontend**: Responsive web app (Next.js, bilingual VI/EN, hybrid search UI).
- **Data Ingestion**: Prompts and context processed via API Gateway; user uploads (reviews, photos) moderated by Rekognition’s AI.
- **Data Storage**: RDS (PostgreSQL) for place data with relational database design (ERD), vector support features, and indexing for optimized query speed; S3 for photos and logs.
- **Data Processing**: Lambda microservices handle Bedrock (Titan embedding model and Claude LLM model) calls, query execution, and result ranking.
- **User Management**: Cognito for JWT-based authentication (email/social login); guest users access limited features.
- **Output**: Displays place cards with AI-generated summaries, ratings, photos, and CTAs (e.g., Get Directions, Call).

---

## 4. Technical Implementation

### Implementation Phases

| Phase | Description                                          | Duration   |
|-------|------------------------------------------------------|------------|
| 1     | Define architecture, Bedrock Titan embedding schema, and RDS (PostgreSQL) schema with ERD design | 2 weeks |
| 2     | Estimate costs and optimize caching strategy          | 1 week |
| 3     | Build backend (Lambda, RDS PostgreSQL, Bedrock Titan embedding, Rekognition)| 3 weeks |
| 4     | Develop frontend (Next.js, bilingual, responsive UI)  | 3 weeks |
| 5     | Test and optimize for <10s latency and scalability    | 2 weeks |
| 6     | Launch MVP, deploy via CI/CD (Terraform, GitLab), collect feedback        | 2 weeks |

### Technical Requirements

- **Edge Devices**: Modern browsers (Chrome, Safari, Firefox) with PWA-ready responsive UI.
- **Cloud**: AWS Route 53, ACM, WAF, CloudFront, API Gateway, Lambda, RDS (PostgreSQL with vector support), S3, Cognito, Bedrock (Titan embedding, Claude LLM), Rekognition, Textract, EventBridge, CloudWatch.
- **Tools & Frameworks**: Next.js (App Router), TypeScript, Terraform for infrastructure-as-code, GitLab for CI/CD.

---

## 5. Timeline & Milestones

| Period              | Activities                                                  |
|---------------------|-------------------------------------------------------------|
| Pre-Development (Month 0 - Sept 2025) | Research Ho Chi Minh City venue datasets for RDS (PostgreSQL)       |
| Month 1 (Oct 2025) | Build backend MVP with Bedrock Titan embedding model and RDS (PostgreSQL)            |
| Month 2 (Nov 2025)  | Implement caching, develop frontend integration            |
| Month 3 (Nov 2025)  | Launch public beta, optimize performance, collect feedback  |
| Post-Launch (Dec 2025) | Add advanced features (e.g., ML-based ranking, offline mode)|

---

## 6. Budget Estimation

### Resource Investment Time Estimation

We estimate that each team member will contribute approximately 20 hours per week to the project. With each phase lasting 2 weeks, the total time investment is estimated as follows:

| Project Phase | Total Estimated Hours |
|:--:|:--:|
| Phase 1: Platform Setup | 200 hours (5 people * 20 hours/week * 2 weeks) |
| Phase 2: Core Feature Development | 200 hours (5 people * 20 hours/week * 2 weeks) |
| Phase 3: Completion and Deployment | 200 hours (5 people * 20 hours/week * 2 weeks) |
| **Total Hours** | **600 hours** |
| **Total Financial Cost** | **$0** |

### Resource Contribution Distribution

The direct financial cost of the project is negligible and fully covered by credits. The main contribution comes from the team's time and support from AWS.

| Participant | Contribution | % Contribution (Total Value) |
|:--|:--|:--|
| Client (Internship Program) | - Learning opportunities and practical experience. | - |
| Partner (Student Team) | - Time and effort (estimated 600 hours). | - |
| AWS | - $200 credit. <br> - Services in Free Tier package. | 100% (financial cost) |

### Cost Optimization Measures
- **Free-Tier Utilization**: Leverage AWS free tiers for Lambda, RDS, S3, CloudFront, Rekognition, and Cognito to minimize costs.
- **Aggressive Caching for Bedrock**: Achieve a high cache hit rate to reduce AI token costs.
- **Batch Rekognition Processing**: Non-real-time image checks to save costs.
- **Optimized RDS Usage**: Use PostgreSQL with proper indexing to minimize query costs.
- **Static Asset Caching**: Minimize outbound data transfer costs through CloudFront.

### Budget Control
- The entire project operates within the **$200 credit from AWS Free Tier**.
- All infrastructure costs are covered by AWS credits and free tier services.
- No direct financial costs are incurred by the team.

---

## 7. Risk Assessment

| Risk                            | Impact | Probability | Mitigation                              |
|---------------------------------|--------|-------------|----------------------------------------|
| RDS data inconsistency          | High   | Medium      | Regular data validation and backups    |
| Inaccurate Titan embedding parsing (VN/EN)  | Medium | Low         | Predefined prompt templates, validation |
| Scalability under high load     | Medium | Medium      | Serverless auto-scaling, caching       |
| Privacy concerns (location data)| High   | Low         | Explicit user consent, anonymized queries |
| Budget overrun                  | High   | Medium      | Strict monitoring, use of free tiers, cost alerts |
| AWS Bedrock service dependency (Titan, Claude)  | High   | Low         | Alternative fallback mechanisms, cached results |
| GitLab platform dependency      | Medium | Low         | Regular backups, alternative repository options |
| Initial data quality            | Medium | Medium      | Data validation processes, user feedback integration |

**Contingency Plans**: Use cached RDS results or local JSON fallback for demos. Implement IP-based rate limits for unauthenticated users. Monitor AWS costs weekly to prevent budget overruns.

---

## 8. Expected Outcomes

### Project Success Criteria

To ensure the MapVibe project succeeds, we have set specific, measurable success criteria:

- **Complete Infrastructure Deployment**: Successfully and securely deploy the entire system architecture to AWS, including all planned services such as VPC, Subnet, RDS, Lambda, S3, CloudFront, API Gateway, and Cognito.
- **Core Search Feature Works Effectively**: The natural language search feature using AWS Bedrock (Titan embedding model, Claude LLM model) and RDS PostgreSQL must be able to understand and return relevant, meaningful results for at least **90%** of common user queries.
- **Cost Optimization**: Total AWS service usage costs throughout development and initial deployment do not exceed the $200 credit from AWS Free Tier package.
- **Product Launch (MVP)**: Complete and launch a minimum viable product (MVP) version of the MapVibe website for the target user group (GenZ) on both **desktop and iOS, Android platforms**. Additionally, include an **administrator dashboard** for managing content and users.
- **Collect Positive Feedback**: After launch, collect at least 50 new location suggestions from users in the first month, demonstrating community interaction and engagement.

### Technical Improvements
- **Mood-Based Conversational Search**: Natural-language support for Vietnamese and English with <10s latency, powered by Bedrock (Titan embedding model and Claude LLM model), understanding user emotions and moods.
- **AI Summaries**: Bedrock-generated place overviews, refreshed every 7 days or after 10 new reviews.
- **Scalability**: Serverless architecture with global CDN delivery via CloudFront.
- **Moderation**: Rekognition's AI ensures safe user-generated content (reviews, photos).
- **Relational Database**: PostgreSQL with optimized indexing for fast query performance.

### Long-Term Value
- **Personalization**: ML-based re-ranking and user behavior analysis based on mood and preferences.
- **Offline Support**: PWA for offline shortlisting of venues.
- **Extensibility**: Potential integration with internal booking systems.
- **Contextual Expansion**: Recommendations based on weather, events, social trends, and user emotions.

---

### Attachments / References
- [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=b574c31813807522d949cf5adca64b41612e12c7)
- [GitLab Repository](https://gitlab.com/4n1d/mapvibe)

## 9. IMPORTANT: Our Proposal Docs Version 
- [Proposal Docs Version](https://docs.google.com/document/d/10YCwRMZcYZ6tFfRam9gASe-DpZf98DxW/edit)
- [Related Documents](https://drive.google.com/drive/u/0/folders/1cdBYgwzBA4Vl9ww_fCPXmbsKToTKgH4L)
