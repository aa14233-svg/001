> **👨‍🏫 教授说**
> 前置条件：Phase 2（语义空间）+ Phase 3 AG-01~AG-06 + Vol 08
> 概念阶梯：原创智能体概念(★) → System Prompt 设计模式(★★) → Skill路由架构(★★★) → MCP跨工具适配(★★★)
>
> **🧑‍💻 TA说**
> 动手入口 → 无配套实验。各工具配置示例见章节内，可直接对照你的 IDE 环境进行实操。

---

# Vol AG-07：跨AI编程工具原创智能体设计与System Prompt路由架构

## 开章

前六卷构建了完整的 Agent 知识体系——从模型选型、场景设计、部署优化到持久记忆与智能编排。但所有这些能力，最终都运行在某一个具体的工具或 IDE 中。

如果你正在使用 VS Code、JetBrains、Cursor 或 Trae，你实际上已经有了一位"AI 助手"。但问题在于：**这些助手是通用的，不是为你定制的**。它们不知道你的代码规范、你的私有 API 设计模式、你的团队命名约定、你的数据库连接池配置偏好。

**原创智能体（Custom Agent）** 的价值就在这里——你不是在调用一个现成的 AI 助手，而是在一个具体的工具环境中，利用其扩展机制（Extension / Plugin / API）自研一个**属于你自己的 AI Agent**。它运行在你的工具本地，知道你的代码库，遵循你的规范，使用你的私有数据管道。

这一卷横跨 7 大类工具——VS Code（Cline/Continue）、QwenCoder、OpenCode、Claude Code、Trae（King Agent IDE）、Hermes、以及其他工具（Cursor/Windsurf/Aider/JetBrains）——系统讲解在每个工具中如何设计原创智能体的 System Prompt、Skill 路由和 MCP 集成。

---

## 1.1 为什么需要"原创智能体"

**定义：** 原创智能体（Custom Agent）是指针对特定工具/IDE 环境、利用其扩展机制（Extension / Plugin / API）自研的 AI Agent，而非调用现成助手。它运行在工具本地，通过 System Prompt 约束行为，通过 Skill 路由执行场景化任务，通过 MCP 协议对接外部工具。

**与"调用现成 AI 助手"的本质区别：**

| 维度 | 调用现成助手 | 原创智能体 |
|------|-------------|-----------|
| **可控性** | 黑盒交互，无法修改行为逻辑 | 全量 System Prompt 可定制，行为可预测 |
| **定制深度** | 仅通过对话指导，无法固化模式 | 可注册私有 Skill，固化工作流与规范 |
| **私有数据管道** | 需手动粘贴/上传数据 | 直接挂载本地代码库、数据库、配置文件 |
| **记忆持久化** | 依赖工具方提供的记忆 | 可对接 AG-06 四层记忆架构 |
| **路由策略** | 单一模型处理所有请求 | 可按任务类型路由到不同模型/Skill |

**与"远程 Agent"的区别：** 原创智能体在工具本地运行和注册，不是通过网络调用远端 Agent。它直接利用工具自身的进程空间、文件系统和扩展 API。远程 Agent（如 AWS Bedrock Agent、Dialogflow CX）运行在云端，通过网络 API 调用，不直接访问本地工具环境。

**拓扑关联：**
- → AG-06 四层记忆（原创智能体的记忆层来源，情景/语义/程序记忆可直接注入 System Prompt 上下文）
- → Vol 08 ReAct/FSM（原创智能体的编排基础，决定智能体如何做决策循环）
- → King-Skill 系统（原创智能体的顶层路由参照，L0→L3 路由层级可适配至各工具）

---

## 1.2 原创智能体的通用架构骨架

所有原创智能体，无论运行在哪个工具中，都遵循一个共通的四层架构：

```
┌──────────────────────────────────────────────────┐
│    System Prompt（宪法）—— 不可变行为约束           │
├──────────────────────────────────────────────────┤
│    Skill（部门法）—— 场景化调用规则                  │
├──────────────────────────────────────────────────┤
│    路由（司法）—— 请求特征 → Skill 选择             │
├──────────────────────────────────────────────────┤
│    MCP（对外接口）—— 标准化工具协议                  │
└──────────────────────────────────────────────────┘
```

**四者分层对比：**

| 层级 | 类比 | 定义域 | 变更频率 | 跨工具迁移性 |
|------|------|--------|---------|-------------|
| **System Prompt** | 宪法 | 抽象层 | 低频 | 高（核心逻辑可复用，语法需适配） |
| **Skill** | 部门法 | 逻辑层 | 中频 | 中（触发条件需适配工具机制） |
| **路由** | 司法 | 控制层 | 高频 | 低（各工具路由机制差异大） |
| **MCP** | 对外接口 | 物理层 | 中频 | 高（MCP 协议标准化） |

**核心理解：** System Prompt 是"你是什么"，Skill 是"你会做什么"，路由是"你什么时候做什么"，MCP 是"你用什么工具做"。四者缺一不可，共同构成一个完整的原创智能体。

---

## 2.1 VS Code 生态：Cline / Continue 原创智能体设计

### Cline 的 System Prompt 注入方式

Cline（原 Claude Dev）会自动读取项目根目录下的 `.clinerules` 文件，将其内容追加到 System Prompt 末尾。这是一个 Markdown 文件，支持任意自定义指令。

```markdown
# .clinerules — 代码审查 Agent 示例
## 角色定义
你是一个严格的代码审查专家，专注于 TypeScript/React 项目。
## 审查规则
1. 所有函数必须有 TypeScript 类型注解
2. React 组件必须使用函数组件 + Hooks，禁止 class 组件
3. 禁止使用 `any` 类型，必须使用 `unknown` 或泛型替代
4. 每个文件不得超过 300 行
5. 错误处理必须使用 Result 模式，禁止裸 throw
```

### Continue 的 config.json 自定义 Agent 模式

Continue 使用 `~/.continue/config.json` 配置自定义 Agent，支持 `customInstructions` 和 `rules` 字段。Skill 设计方面，可借助 VS Code 的 Task Provider API 注册 VS Code Task，通过 LSP 获取代码诊断、补全、跳转等能力。MCP 集成方面，VS Code 1.90+ 内建 MCP 支持，通过 `mcp.json` 配置 MCP 服务器。

### 示例：代码审查原创智能体在 Cline 中的 System Prompt + Skill 组合

```markdown
# .clinerules — 完整代码审查 Agent
## System Prompt 层
你是代码审查专家 Agent，运行在 Cline for VS Code 中。
## Skill 层
### Skill 1：差异分析（触发条件：git diff 有变更）
- 执行 `git diff --name-only` 获取变更文件列表
- 输出：变更文件清单 + 变更行列表
### Skill 2：逐文件审查（触发条件：差异分析完成）
- 读取变更文件的完整内容，按审查规则逐条检查
- 输出：审查报告（Markdown 格式）
### Skill 3：修复建议（触发条件：审查发现 MAJOR 以上问题）
- 对每个问题生成修复代码 diff
## 路由规则
- 轻度变更（< 10 行）→ 仅 Skill 1 + Skill 2
- 中度变更（10-100 行）→ 全流程，输出审查报告
- 重度变更（> 100 行）→ 全流程 + 按模块分组输出
```

---

## 2.2 QwenCoder（通义灵码）原创智能体设计

通义灵码（Tongyi Lingma）基于 Qwen 模型，其开发者模式允许通过自定义插件扩展 Agent 能力。插件入口为 `~/.lingma/plugins/` 目录，通过 `lingma.json` 注册自定义 Agent。

**System Prompt 编写要点（针对 Qwen 模型）：**

| 要点 | 说明 | 示例 |
|------|------|------|
| **中文优先** | Qwen 对中文指令的响应质量和稳定性最高 | 使用中文写 System Prompt，比英文效果更好 |
| **指令清晰度** | Qwen 偏好结构化、分点式的指令 | 用 1. 2. 3. 编号替代段落描述 |
| **角色锚定** | 明确指定角色，Qwen 对角色的跟随性很强 | "你是一个严格的代码规范检查员" |
| **示例驱动** | 给出 1-2 个 I/O 示例可大幅提升准确率 | 在 System Prompt 末尾附加 Few-shot 示例 |

**Skill 与路由：** 通义灵码内置了本地代码分析工具链（linter、type_check、complexity），可作为 Skill 的底层能力，通过 Pipeline 串行执行。

### 示例：企业级代码规范检查 Agent

```markdown
# lingma_agent.md — 企业规范检查 Agent
## 核心指令
1. 用户保存文件时自动触发代码规范检查
2. 检查范围：ESLint 规则 + 企业自定义规则 + 类型安全
3. 严重问题（类型错误、安全漏洞）必须阻止提交
## 企业自定义规则
- 禁止使用 `console.log`，必须使用 `@company/logger`
- 所有 API 请求必须经过统一拦截层
- 组件文件名必须使用 PascalCase，工具函数文件使用 camelCase
## 路由
- 语法错误 → 直接标注，无需 LLM 推理
- 规范问题 → LLM 分析后给出修复建议
- 架构问题 → 触发 Code Review Skill，生成完整报告
```

---

## 2.3 OpenCode 原创智能体设计

OpenCode 是一个开源的终端 AI 编程助手，基于 Agent 循环（Loop）模式运行。其 System Prompt 入口为 `.opencode.md` 文件，支持 Markdown 格式的自定义指令。

**Skill 路由：** OpenCode 实现了基于正则/关键词匹配的轻量路由机制，其思想与 King-Skill 的 L1 特征词典一致：

```yaml
routes:
  - pattern: "test|spec|unit|jest"
    skill: "test_generation"
    priority: 10
  - pattern: "deploy|release|ci/cd|docker"
    skill: "devops"
    priority: 8
  - pattern: ".*"
    skill: "general_dev"
    priority: 1
```

**MCP 集成：** OpenCode 支持通过 MCP Plugin 接口扩展工具能力，配置格式为 JSON 的 `plugins` 数组。

---

## 2.4 Claude Code 原创智能体设计

Claude Code 是 Anthropic 官方推出的终端 AI 编程工具。其核心设计是 **CLAUDE.md 文件**，作为 System Prompt 的主要入口——项目根目录下的 `CLAUDE.md` 自动加载，也支持全局配置 `~/.claude/CLAUDE.md`。

**Skill 设计：** Claude Code 提供 Hook 系统（pre/post hooks）和自定义 Tool 注册机制。

```json
{
  "hooks": {
    "preCommand": [
      {"command": "git pull --rebase", "on": "branch:main"},
      {"command": "npm run lint", "on": "file:*.ts"}
    ],
    "postCommand": [
      {"command": "npm test", "on": "exit_code:0"}
    ]
  }
}
```

**路由逻辑：** Claude Code 支持两种路由模式——**自主路由**（Agent 自行判断下一步，适用于简单任务）和**引导路由**（用户明确指定路径，适用于复杂任务）。

**MCP：** Claude Code 通过 `mcp.json` 配置 MCP 服务器，支持 JSON/YAML 两种格式，使用 `stdio` 或 `http` 传输协议。

### 示例：全栈项目脚手架 Agent 的完整 System Prompt

```markdown
# CLAUDE.md — 全栈项目脚手架 Agent
## 角色定义
你是全栈项目脚手架 Agent，专门负责从零搭建 Web 应用。
## 能力边界
- 擅长：React/Node.js 项目搭建、数据库配置、CI/CD 初始化
- 不擅长：已有项目的迁移、大型代码库重构
## 工作流
1. 确认项目名称和技术栈 → 2. 创建项目目录结构
3. 初始化前端（Vite + React + TypeScript）
4. 初始化后端（Express + Prisma）
5. 配置 Docker + CI/CD
6. 输出项目结构树，确认可运行
## 输出规范
- 每个步骤输出 ✓ 或 ✗ 标记
- 失败步骤输出原因 + 重试方案
```

---

## 2.5 Trae（King Agent IDE）原创智能体设计

Trae（King Agent IDE）是一个以 Agent 为核心的一体化开发环境。其 King Agent 架构已在 King-Skill 文件中详细阐述，核心要点：King Agent 负责全局任务编排，SkillHub 管理 Skill 注册，四层路由（L0→L3）实现智能路由，cc-switch 管理 AI Provider 切换与 MCP 工具。

**在 Trae 中设计垂直 Agent：** 通过 SkillHub 注册新的 Skill 组来创建垂直领域的专用 Agent。继承 King 的"四层结构"思想，精简为专用 Agent 的 System Prompt：

```markdown
# API Doc Generator Agent — System Prompt（四层结构）
## 第一层：身份定义（继承 King）
你是 Trae IDE 中的 API 文档生成 Agent，注册在 SkillHub 中。
## 第二层：能力边界
- 支持：Express/FastAPI/Spring Boot 路由解析
- 支持：JSDoc/Python Docstring/Swagger 注解提取
- 支持：输出 Markdown/OpenAPI 3.0/HTML 格式
## 第三层：输出规范
每个 API 端点输出：方法 + 路径 + 参数 + 返回值 + 示例
## 第四层：工作流
1. 扫描路由文件 → 2. 解析注解 → 3. 生成文档草稿 → 4. 用户确认后定稿
```

**Skill + MCP：** 在 Trae 的 cc-switch 基础上扩展 MCP 工具，通过 `mcpServers` 配置注册 MCP 服务器，Skill 的 `mcp_tool` 字段指向对应的 MCP Tool，形成显式映射。

### 示例：API 文档自动生成 Agent 的 Skill 注册 + MCP 配置

```json
{
  "skillHub": {
    "agents": [{
      "name": "api-doc-generator",
      "skills": [
        {"name": "parse_routes", "trigger": "command:/docs:parse", "mcp_tool": "parse_routes"},
        {"name": "generate_markdown", "trigger": "event:parse_complete", "logic": "llm_generate"}
      ]
    }]
  },
  "mcpServers": {
    "doc-generator": {
      "command": "node", "args": ["mcp-doc-generator.js"]
    }
  }
}
```

---

## 2.6 Hermes（本地 Agent 框架）原创智能体设计

Hermes（承接 AG-03 3.6）是一个轻量级本地 Agent 框架，核心是 Hook+Loop 架构——生命周期钩子（init, pre_tool, post_tool, error, shutdown）和 Agent 主循环（observe → think → act → observe）。

**System Prompt 设计：** Hermes 采用模板层次设计，支持多级继承，`base_prompt` 定义通用行为，`agent_templates` 定义专用 Agent 的增补指令。

**Skill 路由：** Hermes 使用事件驱动路由——每个 Skill 注册感兴趣的事件，路由引擎按事件类型分发到对应的 Skill 处理，无匹配时回退到 LLM 裁决。

**MCP 集成：** Hermes 提供适配器机制，支持 `stdio` 和 `http` 两种传输类型的 MCP 服务器注册，通过 `register_mcp()` 方法将 MCP Tool 注册到 Agent 的运行时中。

### 示例：Hermes 插件式 Agent 的完整生命周期

```python
from hermes import HermesAgent, Hook

class CodeReviewAgent(HermesAgent):
    def __init__(self):
        super().__init__()
        self.register_hook(Hook.INIT, self.load_review_rules)
        self.register_hook(Hook.PRE_TOOL, self.check_permission)
        self.register_hook(Hook.ERROR, self.retry_on_failure)
        self.register_skill("lint_check", LintCheckSkill())
        self.register_skill("security_scan", SecurityScanSkill())
        self.mcp.register("local_linter", {"type": "stdio", "command": "eslint"})
```

---

## 2.7 其他工具补充

- **Cursor**：通过 `.cursorrules` 文件设计定制 Agent，Composer 模式支持多文件编辑的场景化路由
- **Windsurf**：Cascade 模式的 Agent 自定义配置通过 `~/.windsurf/config.json` 的 `cascade.customInstructions` 字段
- **Aider**：通过 `.aider.conf.yml` 的 `custom-instructions-file` 指向自定义指令文件
- **JetBrains AI**：通过插件开发创建自定义 AI Assistant，在 `getSystemPrompt()` 中定义 System Prompt

**各工具总结对比：**

| 工具 | System Prompt 入口 | Skill 机制 | MCP 支持 | 路由方式 |
|------|-------------------|-----------|---------|---------|
| **Cline** | `.clinerules` 文件 | Task Provider API | VS Code MCP 扩展 | LLM 自主路由 |
| **Continue** | `config.json` | 自定义函数 | 实验性支持 | 规则匹配 |
| **QwenCoder** | `lingma.json` | 内置 Toolchain + Pipeline | 有限支持 | 管道串行 |
| **OpenCode** | `.opencode.md` | 正则/关键词匹配 | MCP Plugin 接口 | L1 特征词典 |
| **Claude Code** | `CLAUDE.md` 文件 | Hook 系统 + 自定义 Tool | 官方支持（mcp.json） | 自主 + 引导路由 |
| **Trae** | SkillHub 注册 | SkillHub + 事件触发 | cc-switch 扩展 | L0→L3 四层路由 |
| **Hermes** | YAML 模板层次 | Plugin + 事件驱动 | 适配器机制 | 事件驱动 + LLM 回退 |
| **Cursor** | `.cursorrules` | Composer 模式 | 支持 MCP | 场景化路由 |
| **Windsurf** | `config.json` | Cascade 内置 Skill | 有限支持 | Cascade 自主路由 |
| **Aider** | `.aider-instructions.md` | 自定义指令 | 无原生支持 | 统一指令路由 |
| **JetBrains AI** | 插件 `getSystemPrompt()` | Plugin API 注册 | 通过插件扩展 | 插件显式路由 |

---

## 3.1 跨工具 System Prompt 设计模式

**通用 System Prompt 设计公式：** `角色定义 + 能力边界 + 输出规范 + 工作流`

| 组件 | 作用 | 示例 |
|------|------|------|
| **角色定义** | 告诉模型"你是谁"，锚定行为风格 | "你是一个严格的代码审查专家" |
| **能力边界** | 明确"你会什么、不会什么"，防止幻觉 | "你擅长 React/TypeScript，不擅长 Python 后端" |
| **输出规范** | 规定输出的格式、风格、语言 | "输出使用 Markdown 表格，每项附带严重度标记" |
| **工作流** | 定义任务执行的步骤顺序 | "1. 读取文件 → 2. 分析 → 3. 输出报告" |

**五个经典模式：**

1. **角色扮演模式** — 适用于需要特定专业视角的场景。角色越具体，模型行为越稳定。示例："你是一个有 15 年经验的 SRE，关注资源泄漏、超时配置、日志规范。"

2. **思考链模式（CoT）** — 适用于需要逐步推理的复杂任务。强制模型按步骤思考以提升推理质量。示例："Step 1: 理解需求 → Step 2: 分析现有代码 → Step 3: 识别风险 → Step 4: 给出建议。"

3. **约束求解模式** — 适用于有明确硬性约束的场景。约束要可验证。示例："无外部依赖、单文件实现、兼容 Python 3.9+、内存占用不超过 50MB。"

4. **迭代优化模式** — 适用于需要多轮打磨的场景。需定义迭代终止条件。示例："第一轮功能完整、第二轮性能优化、第三轮代码质量，三轮完成终止。"

5. **多 Agent 协商模式** — 适用于需要多方视角的复杂决策。示例："你同时扮演架构师（关注扩展性）和工程师（关注可行性），在两者之间协商给出最终方案。"

**拓扑关联：** → AG-01 模型选型（不同模型对 System Prompt 的响应差异显著。Qwen 对中文结构化指令响应最佳，Claude 对长上下文角色扮演更稳定，GPT-4o 在思考链模式中推理更连贯。）

---

## 3.2 跨工具 Skill 设计模式

**Skill 定义三要素：** `触发条件 + 执行逻辑 + 输出格式`

| 要素 | 说明 | 示例 |
|------|------|------|
| **触发条件** | 什么情况下激活这个 Skill | 文件保存时 / 用户输入特定命令 |
| **执行逻辑** | 执行的具体步骤和算法 | 先读取文件 → 再调用 linter → 后生成报告 |
| **输出格式** | 结果的呈现方式 | Markdown 报告 / JSON 数据 / 诊断信息 |

**轻量 Skill vs 重型 Skill 的取舍：**

| 维度 | 轻量 Skill | 重型 Skill |
|------|-----------|-----------|
| 执行逻辑 | 规则驱动（if-then / 正则匹配） | LLM 推理驱动 |
| 性能 | 毫秒级 | 秒级到分钟级 |
| 适用场景 | 简单格式化、关键词提取、文件操作 | 代码审查、需求分析、架构设计 |
| 推荐策略 | 能轻则轻——能用规则解决的问题不要用 LLM | 仅在需要语义理解时才使用 LLM |

**路由设计：静态路由（硬编码）vs 动态路由（LLM 裁决）**

| 路由类型 | 机制 | 对应 King-Skill 层级 |
|---------|------|---------------------|
| **静态路由（硬编码）** | 预定义的 if-then 规则 | L0 |
| **关键词路由** | 基于正则/关键词匹配 | L1 |
| **Embedding 路由** | 基于语义相似度匹配 | L2 |
| **LLM 裁决路由** | LLM 自行判断使用哪个 Skill | L3 |

**路由设计原则：** 优先使用静态路由（零成本、零延迟），只有静态路由无法覆盖时才升级到动态路由。

---

## 3.3 跨工具 MCP 适配指南

**MCP 协议的本质：** MCP（Model Context Protocol）是一种标准化工具协议，让 AI Agent 以统一方式调用外部工具。Server 提供工具能力，Client 通过 Transport（stdio 本地进程通信 / HTTP 远程服务调用）调用 Server 提供的工具。

**各工具的 MCP 配置格式对比：**

| 工具 | 配置格式 | 配置文件位置 |
|------|---------|-------------|
| VS Code (Cline) | JSON | `.vscode/mcp.json` |
| Claude Code | JSON | `mcp.json` |
| Trae (King Agent) | JSON | 通过 cc-switch 配置 |
| OpenCode | JSON | `opencode-mcp.json` |
| Hermes | YAML | `hermes-mcp.yaml` |
| Cursor | JSON | `.cursor/mcp.json` |
| Continue | JSON | `config.json` 中嵌套 |

**Skill → MCP Tool 的映射模式：**

- **一对一**：一个 Skill 对应一个 MCP Tool（lint_check → eslint）
- **一对多**：一个 Skill 组合多个 MCP Tool（deploy → build + test + deploy）
- **多对一**：多个 Skill 共享同一个 MCP Tool（code_review + refactor → 都使用 git）

**最佳实践：** 在 MCP 配置中，每个 Tool 的 `name` 应与 Skill 触发条件中的 `mcp_tool` 字段保持一致，形成显式映射关系，使路由引擎可直接找到对应的 MCP Tool 调用。

---

## 4.1 拓扑关联总结表

| 工具 | 对应教材 Volume | 对应 King-Skill 组件 | 路由模式 |
|------|----------------|---------------------|---------|
| **Cline (VS Code)** | AG-07 2.1 | L0 硬编码 + L1 关键词 | LLM 自主路由 |
| **Continue (VS Code)** | AG-07 2.1 | L0 配置驱动 | 规则文件匹配 |
| **QwenCoder (通义灵码)** | AG-07 2.2 | L1 管道串行 | 预定义 Pipeline 串行路由 |
| **OpenCode** | AG-07 2.3 | L1 特征词典 | 正则/关键词匹配路由 |
| **Claude Code** | AG-07 2.4 | L0 + L3 混合 | 自主路由 + 引导路由 |
| **Trae (King Agent IDE)** | AG-07 2.5 | L0→L3 四层路由 | SkillHub 注册 + 多层路由 |
| **Hermes** | AG-03 3.6 + AG-07 2.6 | L2 事件驱动 | 事件驱动路由 + LLM 回退 |
| **Cursor** | AG-07 2.7 | L0 + L1 | Composer 场景化路由 |
| **Windsurf** | AG-07 2.7 | L0 配置驱动 | Cascade 自主路由 |
| **Aider** | AG-07 2.7 | L0 统一指令 | 统一指令路由 |
| **JetBrains AI** | AG-07 2.7 | L0 插件注册 | 插件显式路由 |

---

## 章节小结

| 模块 | 核心概念 | 关键收益 |
|------|---------|---------|
| 原创智能体定义 | 在工具本地利用扩展机制自研的 AI Agent | 可控性、定制深度、私有数据管道 |
| 通用架构骨架 | System Prompt + Skill + 路由 + MCP | 四层分离，跨工具可迁移 |
| 7 大工具生态 | Cline/Continue/QwenCoder/OpenCode/Claude Code/Trae/Hermes | 覆盖主流编程 AI 工具 |
| 跨工具 System Prompt 模式 | 角色扮演/思考链/约束求解/迭代优化/多 Agent 协商 | 5 种可复用模式 |
| 跨工具 Skill 设计 | 三要素 + 轻量 vs 重型 + 静态 vs 动态路由 | 统一的 Skill 设计方法论 |
| 跨工具 MCP 适配 | MCP 协议本质 + 各工具配置对比 + Skill→Tool 映射 | 标准化的工具集成方案 |

从 AG-01 到 AG-07，你不仅掌握了模型选型、场景设计、部署优化、记忆编排，更学会了在具体工具中设计和实现**原创智能体**。这是从"使用 AI 工具"到"创造 AI 工具"的关键跃迁。

---

**→ 相关降维概念：** [[手册：核心概念降维缓存/Concept明细/Concept_11：交通指挥中心|Concept_11：交通指挥中心 (路由架构)]] · [[手册：核心概念降维缓存/Concept明细/Concept_09：乐高零件箱|Concept_09：乐高零件箱 (Skill模块化)]]

→ [[教材：The Automated Mind/Phase 3：模型生态与Agent实践/Vol AG-01：AI模型全景与选型|返回 AG-01 AI模型全景与选型]]