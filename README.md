<div align="center">
  <img src="frontend/src/assets/logo.png" alt="knowledge-live Logo" width="120" />
  <h1>knowledge-live</h1>
  <p><strong>轻量级、模块化的异步 AI 文档处理与 RAG 知识库系统</strong></p>
</div>

---

## 1. 软件定位

**knowledge-live** 是面向个人开发者与小团队设计的**超轻量级**知识库解决方案。

- **极简架构**：拒绝过度封装与复杂微服务，单机或单 Docker Compose 即可跑通全流程。
- **资源占用低**：核心逻辑由 Go + Redis + PostgreSQL 构建，启动迅速、内存占用极小。
- **非阻塞高效处理**：全链路采用轻量级异步任务队列（Asynq），长耗时文档解析与向量化不卡顿。

---

## 2. 核心功能

- **多格式文档解析**：支持 PDF、Markdown、DOCX、TXT 等格式文档上传与文本清洗。
- **异步处理流水线**：上传立即返回 `202 Accepted`，后台自动执行分块（Chunking）与批量向量化（Embedding）。
- **向量检索与知识库管理**：基于 PostgreSQL + `pgvector`（HNSW 索引）实现高效 Top-K 语义检索。
- **流式 RAG 问答**：支持基于上下文的流式问答（SSE），自动携带引用来源（Citations）。
- **任务可观测与可恢复**：任务失败自动指数退避重试，支持通过 Asynqmon 实时监控队列状态。

---

## 3. 技术栈

- **后端**：Go (Gin / Echo) + Asynq (Redis 异步任务队列)
- **数据存储与向量库**：PostgreSQL 16 + `pgvector`
- **对象存储**：本地文件系统 / MinIO（支持无缝切换 S3）
- **前端**：React 19 + Vite + TypeScript
- **部署编排**：Docker Compose

---

## 4. 架构图

```text
                ┌──────────────┐
                │   Frontend   │  (React + Vite)
                │  HTTP / SSE  │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │    Go API    │  (Gin / Echo)
                └──────┬───────┘
                       │
          ┌────────────┼─────────────┐
          ▼            ▼             ▼
      PostgreSQL     Redis        Storage
     (pgvector)   (Asynq Queue)  (Local/MinIO)
          │            │
          │            ▼
          │       ┌──────────┐
          │       │  Worker  │  (Asynq Worker)
          │       └────┬─────┘
          │            │
          │      ┌─────┼─────┐
          │      ▼     ▼     ▼
          │    Parse Chunk Embed
          │                  │
          └──────────────────┘
```

---

## 5. 快速启动

### 5.1 使用 Docker Compose 一键启动

```bash
# 启动所有基础服务与应用
docker compose up -d
```

### 5.2 本地分步开发启动

**启动基础中间件：**

```bash
# 启动 PG (含 pgvector) 与 Redis
docker run -d --name pgvector -p 5432:5432 -e POSTGRES_USER=app -e POSTGRES_PASSWORD=app -e POSTGRES_DB=knowledge_live pgvector/pgvector:pg16
docker run -d --name redis -p 6379:6379 redis:7-alpine
```

**后端启动：**

```bash
cd backend
# 运行 API
go run cmd/api/main.go
# 运行异步 Worker
go run cmd/worker/main.go
```

**前端启动：**

```bash
cd frontend
npm install
npm run dev
```

---

## 6. 项目结构

- `backend/`：Go 后端服务（API、Worker、数据模型、流水线任务）
- `frontend/`：React 前端轻量交互界面
- `docs/`：产品需求文档（PRD）与技术设计文档（TDD）
