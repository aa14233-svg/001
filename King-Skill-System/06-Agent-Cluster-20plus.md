# 20+ Agent 集群体系

> King Agent 的核心不是单打独斗，而是指挥 20+ 专业 Agent 组成的集群军团。

## 一、Agent 集群总览

```
TraeOrchestrator（主调度器）
  │
  ├── 核心 Agent（7个）
  │    ├── ChatAgent            → 对话交互
  │    ├── EditorAgent          → 代码编辑
  │    ├── SearchAgent          → 搜索检索
  │    ├── KnowledgeAgent       → 知识管理
  │    ├── MemoryAgent          → 记忆管理
  │    ├── TaskAgent            → 任务分解
  │    └── PlanAgent            → 计划制定
  │
  ├── 工程 Agent（8个）
  │    ├── FullStackAgent       → 全栈开发
  │    ├── DataAgent            → 数据处理
  │    ├── DocAgent             → 文档生成
  │    ├── TestAgent            → 测试验证
  │    ├── DebugAgent           → 调试排错
  │    ├── DeployAgent          → 部署运维
  │    ├── CodeReviewAgent      → 代码审查
  │    └── SecurityAgent        → 安全检查
  │
  ├── 三合一 Agent（5个）
  │    ├── ProgramDesignerAgent → 程序设计
  │    ├── ArtisticDesignerAgent→ 美工设计
  │    ├── RtkAgent             → RTK 实时内核
  │    ├── HeadroomAgent        → 预留空间管理
  │    └── CodeGraphAgent       → 代码图谱
  │
  └── 路由/调度 Agent（3个）
       ├── SkillHubAgent        → 技能路由
       ├── HookAgent            → Hook 检查
       └── AuditAgent           → 审计验收
```

## 二、Agent 分工与视野

### 核心 Agent（7个）

| Agent | 功能 | 归属 | 依赖 |
|-------|------|------|------|
| ChatAgent | 用户对话、意图理解 | 全局 | 无 |
| EditorAgent | 代码编辑、文件修改 | B | ChatAgent |
| SearchAgent | 搜索代码、文件、网络 | C | ChatAgent |
| KnowledgeAgent | 知识库管理、检索 | C | SearchAgent |
| MemoryAgent | 短期/长期记忆管理 | 全局 | 无 |
| TaskAgent | 任务分解为子任务 | A | PlanAgent |
| PlanAgent | 制定执行计划 | A | TaskAgent |

### 工程 Agent（8个）

| Agent | 功能 | 归属 | 依赖 |
|-------|------|------|------|
| FullStackAgent | 全栈开发、前后端 | B | EditorAgent |
| DataAgent | 数据分析、处理 | B | SearchAgent |
| DocAgent | 文档生成、格式转换 | B | EditorAgent |
| TestAgent | 测试编写、运行 | B | FullStackAgent |
| DebugAgent | 错误排查、修复 | B | EditorAgent |
| DeployAgent | 部署配置、发布 | B | FullStackAgent |
| CodeReviewAgent | 代码审查、质量 | B | FullStackAgent |
| SecurityAgent | 安全检查、漏洞 | B | CodeReviewAgent |

### 三合一 Agent（5个）

| Agent | 功能 | 归属 | 核心能力 |
|-------|------|------|---------|
| ProgramDesignerAgent | 程序架构设计 | A | 系统设计、架构规划 |
| ArtisticDesignerAgent | 美工视觉设计 | A | UI/UX、图标、配色 |
| RtkAgent | 实时内核监控 | C | 运行时状态、性能 |
| HeadroomAgent | 预留空间管理 | B | 资源预留、缓冲区 |
| CodeGraphAgent | 代码图分析 | C | 依赖图、调用链 |

## 三、Agent 路由规则

### 路由依据

```
请求特征 → Agent 匹配
  │
  ├── 代码相关 → 工程 Agent
  │    ├── 前端 → FullStackAgent
  │    ├── 后端 → FullStackAgent
  │    ├── 测试 → TestAgent
  │    └── 调试 → DebugAgent
  │
  ├── 文档相关 → DocAgent / 文档技能
  │
  ├── 设计相关 → 三合一 Agent
  │    ├── 架构设计 → ProgramDesignerAgent
  │    └── 视觉设计 → ArtisticDesignerAgent
  │
  ├── 搜索相关 → SearchAgent / Agent C
  │
  └── 管理相关 → 路由/调度 Agent
       ├── 技能路由 → SkillHubAgent
       └── 审计验收 → AuditAgent
```

### Agent 加载策略

```
Agent 不是全部常驻内存，而是按需加载：

1. 基础 Agent（ChatAgent, EditorAgent, SearchAgent）→ 常驻
2. 领域 Agent（FullStackAgent, DocAgent 等）→ 按需加载
3. 三合一 Agent（ProgramDesignerAgent 等）→ 检测到设计需求时加载
4. 路由/调度 Agent（SkillHubAgent 等）→ 路由决策时加载
```

## 四、Agent 间通信

### 通信方式

```
串行通信：Agent1 → Agent2 → Agent3
  适用：流水线任务（搜索 → 分析 → 输出）

并行通信：Agent1 + Agent2 + Agent3
  适用：独立子任务（前端 + 后端 + 测试）

主从通信：Master → Agent1 → Agent2
  适用：Master 调度子 Agent
```

### 通信协议

```
Agent 间通信使用标准化的"任务卡"格式：

{
  "from": "ChatAgent",
  "to": "FullStackAgent",
  "task_id": "t-20260709-001",
  "type": "code_generation",
  "payload": {
    "requirement": "创建 React 登录页面",
    "context": { "project": "my-app", "framework": "React" }
  },
  "hooks": ["hook_prep", "hook_exec", "hook_accept"],
  "deadline": "D1"
}
```

## 五、Agent 与 Skill 的对应关系

| 技能 | 主要 Agent | 辅助 Agent |
|------|-----------|-----------|
| cc-switch | SkillHubAgent | AuditAgent |
| token-stats | AuditAgent | KnowledgeAgent |
| master-engineer-hub | PlanAgent | SkillHubAgent |
| lso-os | 全体 Agent（五境分配） | — |
| omnipotent-engineer | FullStackAgent | 工程 Agent 7个 |
| docx | DocAgent | ArtisticDesignerAgent |
| pptx | DocAgent | ArtisticDesignerAgent |
| html-report | FullStackAgent | DocAgent |