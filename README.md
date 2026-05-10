# 面向codex的AI 产品开发技能流程

这是一个帮助你用 AI 做产品、工具、网站、应用或游戏开发的技能仓库。

它解决的核心问题不是“让 AI 多写代码”，而是让 AI 在开发前先理解你的方向，在开发中不随意扩展功能，在验收时不能用半成品糊弄完成。

这些技能主要面向 Codex 使用；其他 AI 工具可能无法完整适配其中的技能调用、子代理编排和验收流程。

仓库内置了 `bdd-before-planning` 技能。它会在工程计划开始前，根据你的设计文档、UI 原型、当前代码事实和目标，先做一次 BDD 行为定向：锁定每个 Task 真正要达成的用户/玩家行为、验收证据和禁止臆想项。这样 AI 后续写计划和执行时，会围绕你的产品方向推进，而不是把“它觉得合理”的功能自动加入项目。

当前提供的核心技能：

- `main-agent-subagent-guardrails`：只适合作为 GPT-5.5 的主代理技能使用，用于把 GPT-5.5 主代理锁定为“顾问 / 调度 / 验收”角色，并统一 GPT-5.x 子代理的边界、证据、返工和验收规则。
- `bdd-before-planning`：在 `superpowers:writing-plans` 写工程实施计划前，先根据 UI 原型、设计文档、当前代码事实和用户目标生成 BDD 行为定向，锁住 Task 行为闭环、范围边界、验收证据和禁止臆想项。

## 推荐产品开发流程

推荐按下面顺序使用技能。这个流程适合单人用 AI 推进真实产品或游戏开发。

```text
[你的想法]
    |
    v
[superpowers:brainstorming]      把想法变成设计文档
    |
    v
[web-design-engineer]            把 UI 原型搭出来
    |
    v
[bdd-before-planning]            在工程计划前锁住行为方向
    |
    v
[superpowers:writing-plans]      写工程实施计划
    |
    v
[main-agent-subagent-guardrails] 按计划执行与验收
```

### 1. 把想法变成设计文档

先使用 [`superpowers:brainstorming`](C:/Users/xinme/.codex/superpowers/skills/brainstorming/SKILL.md)。

这一阶段用来把一个模糊想法整理成产品或游戏设计文档。重点是确定目标用户、核心体验、功能边界、数据流、错误处理和验收方向。不要一开始就让 AI 写工程计划，否则它容易在需求还没稳定时提前脑补实现细节。

### 2. 把 UI 原型搭出来

再使用 [`web-design-engineer`](C:/Users/xinme/.codex/skills/web-design-engineer/SKILL.md)。

这一阶段把设计文档转成可看的 UI 原型。原型不是装饰品，它是后续 BDD 和工程计划的真源之一。页面结构、关键交互、状态反馈和用户路径都应该尽量在原型里显出来，方便后续约束 AI 不偏离方向。

### 3. 在工程计划前做 BDD 行为定向

然后使用 [`bdd-before-planning`](./bdd-before-planning/SKILL.md)。

这一阶段专门防止 AI 偏离你的方向。它会读取设计文档、UI 原型、当前代码事实和你的明确要求，把每个 Task 前面都加上 BDD 行为定向：

- 这个 Task 要让用户/玩家完成什么行为。
- 哪些依据来自设计文档、UI 原型或当前代码。
- 哪些只是保守推断，不能自动升级成必做。
- 哪些功能属于 AI 臆想，禁止加入第一版。
- 正常路径、失败路径和边界路径是什么。
- 需要什么真实验收证据才算完成。

这个阶段不做工程设计，不拆文件，不写接口，不写测试代码。它只负责把方向锁住。

### 4. 写工程实施计划

接着使用 [`superpowers:writing-plans`](C:/Users/xinme/.codex/superpowers/skills/writing-plans/SKILL.md)。

这一阶段根据 BDD 行为定向写工程实施计划。文件结构、模块边界、接口契约、状态设计、测试代码、命令和提交粒度都在这里处理。工程计划必须映射到前一步的 BDD Task，不能新增 BDD 没有授权的功能。

### 5. 用主代理执行计划

最后使用 [`main-agent-subagent-guardrails`](./main-agent-subagent-guardrails)。

这一阶段让 GPT-5.5 主代理按已经确认的计划执行。主代理只做顾问、调度和验收，不自己下场改代码；子代理按 Task/step 边界执行；每个 Task 都要提交真实证据，缺证据、假数据、只做表面 UI、只做后端但用户路径走不通，都不能验收通过。

## 这个技能是做什么的

### `bdd-before-planning`

如果你准备让 AI 根据 UI 原型、设计文档或当前代码开始写工程实施计划，这个技能会先做一次 BDD 行为门禁。

它不写工程设计，也不替代 `superpowers:writing-plans`。它只回答：

- 当前 Task 要达成什么用户/玩家行为。
- 哪些行为来自原型、设计文档、代码事实或用户明确要求。
- 哪些只是保守推断，不能自动升级成必做。
- 哪些是 AI 容易臆想的扩展功能，必须禁止进入第一版。
- 每个 Task 需要什么真实验收证据，才能进入工程计划。

用法上，它应该排在 `superpowers:writing-plans` 之前：

```text
UI 原型 / 设计文档 / 当前代码事实 / 用户目标
-> bdd-before-planning
-> superpowers:writing-plans
-> 执行
```

### `main-agent-subagent-guardrails`

如果你已经有计划、Task 或明确的交付边界，这个技能会帮你把 GPT-5.5 主代理管住，不让它越界乱做。

你用它时，得到的不是一份“帮你现写的主代理提示词”，而是一套只面向 GPT-5.5 主代理的派工约束：

- 这个技能只适合作为 GPT-5.5 的主代理技能，不适合其他主代理。
- GPT-5.5 主代理只能做顾问、派发子代理和验收。
- GPT-5.5 主代理不能自己下场实现，也不能把整个 Task 一股脑外包给单个子代理。
- 子代理必须按 Task/step 边界接活，而不是自己重拆任务体系。
- 子代理要按统一规则提交证据、日志、阻塞项和返工结果。
- 前端、真实浏览器验证、代码实现这些场景，都能挂上对应前置技能要求。

## 什么时候适合用 main-agent-subagent-guardrails

如果你遇到下面这些情况，就适合使用 `main-agent-subagent-guardrails`：

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

## 什么时候不建议用 main-agent-subagent-guardrails

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

### 5. `superpowers`

适用场景：

- 当任务需要先做产品/实施规划、把大目标拆成可执行计划、或按计划推进实现时使用。
- 当后端和前端逻辑需要 TDD/测试先行时使用，例如先写失败测试、再实现、再跑回归。
- 当一个 Task 完成后需要最后的子代理双审时使用，包括规格符合性审查和必要的代码质量审查。
- 在本技能里，`superpowers` 主要负责规划、TDD 流程和最终审查方法；具体派发边界、模型路由、真源下发和验收口径仍由 `main-agent-subagent-guardrails` 约束。

GitHub 原技能地址：

- 仓库首页：[obra/superpowers](https://github.com/obra/superpowers)

## 使用特点

- 默认使用中文组织主代理约束和子代理交接。
- 只面向 GPT-5.5 主代理，不把适用范围泛化到其他主代理。
- 优先以当前用户消息、当前点名计划、规格和代码为真源。
- 不会把旧记忆、旧模板或过期路线放在当前真源之前。
- 强调主代理不越界、子代理不吞 Task、验收不放水。

## 想看具体规则

- BDD 计划前行为定向：[`bdd-before-planning/SKILL.md`](./bdd-before-planning/SKILL.md)
- BDD Task 模板：[`bdd-before-planning/references/task-bdd-template.md`](./bdd-before-planning/references/task-bdd-template.md)
- BDD 到 writing-plans 交接约束：[`bdd-before-planning/references/writing-plans-handoff.md`](./bdd-before-planning/references/writing-plans-handoff.md)
- 技能主规则：[`main-agent-subagent-guardrails/SKILL.md`](./main-agent-subagent-guardrails/SKILL.md)
- 默认展示配置：[`main-agent-subagent-guardrails/agents/openai.yaml`](./main-agent-subagent-guardrails/agents/openai.yaml)
- 子代理日志规范：[`main-agent-subagent-guardrails/references/subagent-log.md`](./main-agent-subagent-guardrails/references/subagent-log.md)
