# AI Agent 路由与约束总入口 (AGENTS.md)

本项目受 Harness 约束工程管理。任何 AI Agent 在处理任务时必须严格执行协同闭环流程。

## 1. 协同闭环铁律

```
User Input → AGENTS.md → 任务分析 → Agent选择 → 约束查阅 → TDD编码/实现 → 测试验证 → 产出记录
```

### 强制执行要求
1. **开头显式声明**：回复开头必须注明任务类型、类别（P0/P1/P2）、选用 Agent 与查阅的约束文件路径。
2. **禁止直接改代码**：禁止未做任务分析与约束查阅直接修改代码。
3. **禁止跨领域越权**：严格遵循选用 Agent 的职责边界。
4. **禁止跳过测试与校验**：代码提交/完成前必须通过类型检查、Lint、构建与关键测试。
5. **冲突处理**：用户指令与工程约束冲突时，优先遵守约束并向用户说明原因。

---

## 2. 任务分类与 Agent 路由表

| 任务类型 | 优先级 | 负责 Agent | 对应约束与规范 |
|----------|--------|------------|----------------|
| 后端 API / 鉴权 / 路由 / 业务逻辑 | P0/P1/P2 | `backend-agent` | `harness/rules/backend/*`, `docs/AI-Document-Hub-TDD.md` |
| 异步任务队列 / Worker / Chunk / Embedding | P0/P1 | `worker-agent` | `harness/rules/backend/worker.md`, `docs/AI-Document-Hub-TDD.md` |
| 前端 UI / 交互 / 状态 / SSE 对接 | P0/P1/P2 | `frontend-agent` | `harness/rules/frontend/*`, `frontend/` |
| 数据库 Schema / 向量检索 / 索引迁移 | P0/P1 | `db-agent` | `harness/rules/backend/database.md`, `harness/knowledge/decisions/*` |
| 架构设计 / 技术选型 / 接口契约 | P0/P1 | `arch-agent` | `harness/rules/architecture/*`, `docs/*` |
| 测试设计 / 质量门禁 / 回归验证 | P0/P1/P2 | `test-agent` | `harness/rules/code-quality/*`, `harness/checklists/*` |
| 代码审查 / 安全与性能审计 | P0/P1 | `review-agent` | `harness/rules/code-quality/security.md`, `harness/checklists/review.md` |

---

## 3. 标准目录索引

- **Agent 角色定义**：`harness/agents/`
- **全局约束规则**：`harness/rules/`
- **执行工作流**：`harness/workflows/`
- **验收检查清单**：`harness/checklists/`
- **项目决策与架构知识**：`harness/knowledge/`
- **产出模板**：`harness/templates/`
