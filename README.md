# main-agent-subagent-guardrails

这是一个给你控制“GPT-5.5 主代理怎么派子代理”的技能仓库。

当前提供的核心技能是：

- `main-agent-subagent-guardrails`：只适合作为 GPT-5.5 的主代理技能使用，用于把 GPT-5.5 主代理锁定为“顾问 / 调度 / 验收”角色，并统一 GPT-5.x 子代理的边界、证据、返工和验收规则。

## 这个技能是做什么的

### `main-agent-subagent-guardrails`

如果你已经有计划、Task 或明确的交付边界，这个技能会帮你把 GPT-5.5 主代理管住，不让它越界乱做。

你用它时，得到的不是一份“帮你现写的主代理提示词”，而是一套只面向 GPT-5.5 主代理的派工约束：

- 这个技能只适合作为 GPT-5.5 的主代理技能，不适合其他主代理。
- GPT-5.5 主代理只能做顾问、派发子代理和验收。
- GPT-5.5 主代理不能自己下场实现，也不能把整个 Task 一股脑外包给单个子代理。
- 子代理必须按 Task/step 边界接活，而不是自己重拆任务体系。
- 子代理要按统一规则提交证据、日志、阻塞项和返工结果。
- 前端、真实浏览器验证、代码实现这些场景，都能挂上对应前置技能要求。

## 什么时候适合用

如果你遇到下面这些情况，就适合用这个技能：

- 你已经有实施计划、规格或明确的 Task 边界，需要主代理按既定计划稳定派工。
- 你希望 GPT-5.5 主代理只做顾问、调度、验收，不要自己写实现。
- 你希望多个 GPT-5.x 子代理都按同一套交接规则工作。
- 你要给大任务加上并行/串行、双审、真实浏览器验证、真实链路验收和反缩水交付的硬约束。
- 你要保护前端原型 UI，不允许以“去硬编码”或“为了快”为理由破坏既定界面和交互。

## 用了之后你会得到什么

通常你会得到这些结果：

- 一套明确的 GPT-5.5 主代理职责边界。
- 一份份可执行的 GPT-5.x 子代理交接单。
- 清晰的 step/子步骤边界，而不是把整个 Task 模糊外包出去。
- 统一的完成标准、证据要求、停止条件和阻塞上报方式。
- 更稳定的大任务推进节奏：当前 Task 完成开发、集成、验收证据、双审和集中验收后，再进入下一个 Task。

## 什么时候不建议用

下面这些场景不建议引入这套约束：

- 你只是要做普通代码实现，不需要主代理调度多个子代理。
- 你只是要润色一段普通提示词，不需要 Task/step、验收门、返工和证据规则。
- 任务很小，直接执行更快，不值得引入整套子代理编排约束。
- 你不是要让 GPT-5.5 当主代理，而是想给别的主代理模型复用这套合同。

## 前置技能

这个技能在子代理执行不同类型任务时，会要求启用对应前置技能。
具体安装或接入方式请直接看各自原仓库文档。

### 1. `web-design-engineer`

适用场景：

- 当子代理涉及 UI 实现、前端视觉调整、截图后修正、设计系统对齐、交互细节调整或视觉审查时使用。

GitHub 原技能地址：

- 仓库首页：[ConardLi/garden-skills](https://github.com/ConardLi/garden-skills)
- 技能目录：[skills/web-design-engineer](https://github.com/ConardLi/garden-skills/tree/main/skills/web-design-engineer)

### 2. `karpathy-guidelines`

适用场景：

- 当子代理需要写代码、改代码、重构或修复问题时使用，用来约束实现范围和可验证交付。

GitHub 原技能地址：

- 仓库首页：[forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)
- 技能目录：[skills/karpathy-guidelines](https://github.com/forrestchang/andrej-karpathy-skills/tree/main/skills/karpathy-guidelines)

### 3. `playwright`

适用场景：

- 当子代理需要做真实浏览器验证、浏览器验收、页面交互复现、截图取证或终端驱动的页面调试时使用。

GitHub 原技能地址：

- 仓库首页：[openai/skills](https://github.com/openai/skills)
- 技能目录：[skills/.curated/playwright](https://github.com/openai/skills/tree/main/skills/.curated/playwright)

### 4. `graphify`

适用场景：

- 当主代理或子代理需要低成本获取项目上下文、模块关系、代码路径、文档关系或知识图谱索引时使用。
- `graphify` 用来把代码、文档、PDF、图片、视频等项目材料生成可查询的上下文知识图谱，常见产物包括 `graphify-out/GRAPH_REPORT.md`、`graph.json` 和 `graph.html`。
- 在本技能里，Graphify 只作为“上下文知识图谱”和“项目关系索引”使用，不作为产品行为、提示词模板、技能路由或并行上限的真源。

GitHub 原工具地址：

- 仓库首页：[safishamsi/graphify](https://github.com/safishamsi/graphify)

## 使用特点

- 默认使用中文组织主代理约束和子代理交接。
- 只面向 GPT-5.5 主代理，不把适用范围泛化到其他主代理。
- 优先以当前用户消息、当前点名计划、规格和代码为真源。
- 不会把旧记忆、旧模板或过期路线放在当前真源之前。
- 强调主代理不越界、子代理不吞 Task、验收不放水。

## 想看具体规则

- 技能主规则：[`main-agent-subagent-guardrails/SKILL.md`](./main-agent-subagent-guardrails/SKILL.md)
- 默认展示配置：[`main-agent-subagent-guardrails/agents/openai.yaml`](./main-agent-subagent-guardrails/agents/openai.yaml)
- 子代理日志规范：[`main-agent-subagent-guardrails/references/subagent-log.md`](./main-agent-subagent-guardrails/references/subagent-log.md)
