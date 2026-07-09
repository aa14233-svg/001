# 20+ Agent 集群体系

> King Agent 的核心不是单打独斗，而是指挥 20+ 专业 Agent 组成的集群军团。

## 一、Agent 集群总览

```
TraeOrchestrator（主调度器）
  │
  ├── 核心 Agent（7个）
  │    ├── ChatAgent            → 对话交互、意图理解
  │    ├── EditorAgent          → 代码编辑、文件修改
  │    ├── SearchAgent          → 搜索检索、文件查找
  │    ├── KnowledgeAgent       → 知识管理、库检索
  │    ├── MemoryAgent          → 短期/长期记忆管理
  │    ├── TaskAgent            → 任务分解、子任务管理
  │    └── PlanAgent            → 计划制定、进度跟踪
  │
  ├── 工程 Agent（8个）
  │    ├── FullStackAgent       → 全栈开发（前端+后端）
  │    ├── DataAgent            → 数据处理、分析
  │    ├── DocAgent             → 文档生成、格式转换
  │    ├── TestAgent            → 测试编写、运行
  │    ├── DebugAgent           → 错误排查、修复
  │    ├── DeployAgent          → 部署配置、发布
  │    ├── CodeReviewAgent      → 代码审查、质量
  │    └── SecurityAgent        → 安全检查、漏洞
  │
  ├── 三合一 Agent（5个）
  │    ├── ProgramDesignerAgent → 程序设计、架构规划
  │    ├── ArtisticDesignerAgent→ 美工设计、UI/UX
  │    ├── RtkAgent             → RTK 实时内核监控
  │    ├── HeadroomAgent        → 预留空间管理
  │    └── CodeGraphAgent       → 代码图谱、依赖分析
  │
  └── 路由/调度 Agent（3个）
       ├── SkillHubAgent        → 技能路由、域识别
       ├── HookAgent            → Hook 检查、质量门控
       └── AuditAgent           → 审计验收、Token 审计
```

## 二、Agent 分工与视野

### 核心 Agent（7个）

| Agent | 功能 | 归属 | 依赖 | 可见工具 | 加载策略 |
|-------|------|------|------|---------|---------|
| ChatAgent | 用户对话、意图理解 | 全局 | 无 | 对话相关 | **常驻** |
| EditorAgent | 代码编辑、文件修改 | B | ChatAgent | 编辑工具 | **常驻** |
| SearchAgent | 搜索代码、文件、网络 | C | ChatAgent | 搜索工具 | **常驻** |
| KnowledgeAgent | 知识库管理、检索 | C | SearchAgent | 知识工具 | 按需 |
| MemoryAgent | 短期/长期记忆管理 | 全局 | 无 | 记忆工具 | **常驻** |
| TaskAgent | 任务分解为子任务 | A | PlanAgent | 任务工具 | 按需 |
| PlanAgent | 制定执行计划 | A | TaskAgent | 计划工具 | 按需 |

### 工程 Agent（8个）

| Agent | 功能 | 归属 | 依赖 | 可见工具 | 加载策略 |
|-------|------|------|------|---------|---------|
| FullStackAgent | 全栈开发、前后端 | B | EditorAgent | 编码工具 | 按需 |
| DataAgent | 数据分析、处理 | B | SearchAgent | 数据处理工具 | 按需 |
| DocAgent | 文档生成、格式转换 | B | EditorAgent | 文档工具 | 按需 |
| TestAgent | 测试编写、运行 | B | FullStackAgent | 测试工具 | 按需 |
| DebugAgent | 错误排查、修复 | B | EditorAgent | 调试工具 | 按需 |
| DeployAgent | 部署配置、发布 | B | FullStackAgent | 部署工具 | 按需 |
| CodeReviewAgent | 代码审查、质量 | B | FullStackAgent | 审查工具 | 按需 |
| SecurityAgent | 安全检查、漏洞 | B | CodeReviewAgent | 安全工具 | 按需 |

### 三合一 Agent（5个）

| Agent | 功能 | 归属 | 核心能力 | 加载条件 |
|-------|------|------|---------|---------|
| ProgramDesignerAgent | 程序架构设计 | A | 系统设计、架构规划 | 检测到设计需求 |
| ArtisticDesignerAgent | 美工视觉设计 | A | UI/UX、图标、配色 | 检测到视觉需求 |
| RtkAgent | 实时内核监控 | C | 运行时状态、性能 | 检测到性能需求 |
| HeadroomAgent | 预留空间管理 | B | 资源预留、缓冲区 | 检测到资源需求 |
| CodeGraphAgent | 代码图分析 | C | 依赖图、调用链 | 检测到分析需求 |

### 路由/调度 Agent（3个）

| Agent | 功能 | 归属 | 核心能力 | 加载条件 |
|-------|------|------|---------|---------|
| SkillHubAgent | 技能路由 | 全局 | 域识别、技能匹配 | 每次任务路由时 |
| HookAgent | Hook 检查 | 全局 | 质量门控 | 每个 Hook 节点 |
| AuditAgent | 审计验收 | 全局 | Token 审计、D3 验收 | 任务完成时 |

## 三、Agent 路由规则

### 路由依据

```
请求特征 → Agent 匹配
  │
  ├── 代码相关 → 工程 Agent
  │    ├── 前端 → FullStackAgent
  │    ├── 后端 → FullStackAgent + DataAgent
  │    ├── 测试 → TestAgent
  │    ├── 调试 → DebugAgent
  │    └── 审查 → CodeReviewAgent + SecurityAgent
  │
  ├── 文档相关 → DocAgent / 文档技能
  │    ├── 技术文档 → DocAgent
  │    └── 商业文档 → DocAgent + doc-writing-guide
  │
  ├── 设计相关 → 三合一 Agent
  │    ├── 架构设计 → ProgramDesignerAgent
  │    └── 视觉设计 → ArtisticDesignerAgent
  │
  ├── 搜索相关 → SearchAgent / Agent C
  │    ├── 文件搜索 → SearchAgent
  │    ├── 知识检索 → KnowledgeAgent
  │    └── 代码分析 → CodeGraphAgent
  │
  └── 管理相关 → 路由/调度 Agent
       ├── 技能路由 → SkillHubAgent
       ├── Hook 检查 → HookAgent
       └── 审计验收 → AuditAgent
```

### Agent 加载策略

```
Agent 不是全部常驻内存，而是按需加载：

1. 基础 Agent（ChatAgent, EditorAgent, SearchAgent, MemoryAgent）
   → 常驻内存，随时可用
   → 优先级：最高

2. 工程 Agent（FullStackAgent, DocAgent, DataAgent 等）
   → 按需加载，用完即卸
   → 优先级：任务需要时加载

3. 三合一 Agent（ProgramDesignerAgent, ArtisticDesignerAgent 等）
   → 检测到设计需求时加载
   → 优先级：低于工程 Agent

4. 路由/调度 Agent（SkillHubAgent, HookAgent, AuditAgent）
   → 路由决策时加载
   → 优先级：任务开始时加载
```

### Agent 加载时间预估

| Agent 类型 | 加载时间 | 内存占用 | 常驻/按需 |
|-----------|---------|---------|----------|
| ChatAgent | <1ms | ~10KB | 常驻 |
| EditorAgent | <1ms | ~15KB | 常驻 |
| SearchAgent | <1ms | ~10KB | 常驻 |
| FullStackAgent | ~3ms | ~50KB | 按需 |
| DocAgent | ~2ms | ~30KB | 按需 |
| ProgramDesignerAgent | ~2ms | ~40KB | 按需 |
| SkillHubAgent | ~1ms | ~20KB | 按需 |

## 四、Agent 间通信

### 通信方式

```
串行通信：Agent1 → Agent2 → Agent3
  适用：流水线任务
  示例：搜索 → 分析 → 输出
  ├── SearchAgent → DataAgent → DocAgent
  └── 数据流：搜索结果 → 分析结果 → 最终文档

并行通信：Agent1 + Agent2 + Agent3
  适用：独立子任务
  示例：前端 + 后端 + 测试
  ├── FullStackAgent (前端) + FullStackAgent (后端) + TestAgent
  └── 结果合并：各子任务完成后统一集成

主从通信：Master → Agent1 → Agent2
  适用：Master 调度子 Agent
  示例：PlanAgent 调度多个 Agent
  ├── PlanAgent → FullStackAgent → TestAgent
  └── 控制流：Master 分配任务，子 Agent 执行并汇报
```

### 通信协议

Agent 间通信使用标准化的"任务卡"格式：

```json
{
  "from": "ChatAgent",
  "to": "FullStackAgent",
  "task_id": "t-20260709-001",
  "type": "code_generation",
  "priority": "high",
  "payload": {
    "requirement": "创建 React 登录页面",
    "context": { 
      "project": "my-app", 
      "framework": "React",
      "existing_code": "src/App.jsx"
    }
  },
  "hooks": ["hook_prep", "hook_exec", "hook_accept"],
  "deadline": "D1",
  "token_budget": 500,
  "timeout_ms": 30000
}
```

**任务卡字段说明**：

| 字段 | 说明 | 必填 |
|------|------|------|
| from | 发送方 Agent 名称 | 是 |
| to | 接收方 Agent 名称 | 是 |
| task_id | 唯一任务标识 | 是 |
| type | 任务类型 | 是 |
| priority | 优先级（high/medium/low） | 否 |
| payload | 任务具体内容 | 是 |
| hooks | 需要触发的 Hook 节点 | 否 |
| deadline | 验收级别（D1/D2/D3） | 否 |
| token_budget | Token 预算上限 | 否 |
| timeout_ms | 超时时间 | 否 |

### Agent 间通信示例

```
场景：用户要求"创建一个带登录功能的 React 应用"

1. ChatAgent 接收请求
   └── 发送任务卡给 PlanAgent

2. PlanAgent 制定计划
   ├── 任务1：FullStackAgent - 创建页面结构和路由
   ├── 任务2：DataAgent - 设计数据模型和 API 对接
   └── 任务3：TestAgent - 编写测试用例

3. FullStackAgent 完成任务1
   └── 回复任务卡给 PlanAgent
   └── PlanAgent 检查 D1 通过 → 分配任务2

4. 所有任务完成后
   └── AuditAgent 执行 D3 验收
   └── 交付结果给用户
```

## 五、Agent 与 Skill 的对应关系

| 技能 | 主要 Agent | 辅助 Agent | 协作模式 |
|------|-----------|-----------|---------|
| cc-switch | SkillHubAgent | AuditAgent | SkillHubAgent 路由，AuditAgent 审计 |
| token-stats | AuditAgent | KnowledgeAgent | AuditAgent 执行，KnowledgeAgent 存储 |
| master-engineer-hub | PlanAgent | SkillHubAgent | PlanAgent 决策，SkillHubAgent 执行 |
| lso-os | 全体 Agent（五境分配） | — | 五境逐层，各 Agent 各司其职 |
| omnipotent-engineer | FullStackAgent | 工程 Agent 7个 | FullStackAgent 主导，工程 Agent 协作 |
| docx | DocAgent | ArtisticDesignerAgent | DocAgent 创建，ArtisticDesignerAgent 美化 |
| pptx | DocAgent | ArtisticDesignerAgent | DocAgent 创建，ArtisticDesignerAgent 设计 |
| html-report | FullStackAgent | DocAgent | FullStackAgent 开发，DocAgent 文档 |

## 六、Agent 集群的异常处理

| 异常场景 | 处理策略 | 恢复机制 |
|---------|---------|---------|
| Agent 加载失败 | 记录错误，切换到替代 Agent | 重试 2 次后降级 |
| Agent 执行超时 | 终止当前 Agent，回收资源 | 回退到上一个稳定状态 |
| Agent 间通信失败 | 重试 3 次 | 切换通信方式（串行→并行） |
| Agent 产出异常 | 触发 Hook 3 验收不通过 | Loop 循环最多 3 次 |
| 内存不足 | 卸载不活跃 Agent | 保留常驻 Agent，释放按需 Agent |

---

## 补充说明

### 1. Agent 归属与视野隔离的实现

Agent 归属（A/B/C）决定了 Agent 的工具可见性：

```
Agent 创建时：
  ├── 分配归属域（A/B/C）
  ├── 加载该域的工具集
  └── 该域的工具上限限制

Agent 执行时：
  ├── 只能使用自己域内的工具
  ├── 不可见其他域的工具
  └── 不可跨域调用工具

Agent 完成后：
  ├── 工具从视野移除
  ├── 产出传递给下一个 Agent
  └── 下一个 Agent 加载自己的工具集
```

### 2. 三合一 Agent 的协同工作

三合一 Agent（ProgramDesignerAgent, ArtisticDesignerAgent, RtkAgent, HeadroomAgent, CodeGraphAgent）通常不会单独工作，而是与其他 Agent 协同：

```
示例：设计一个电商网站首页

1. ProgramDesignerAgent（架构设计）
   ├── 设计页面组件结构
   ├── 定义数据流
   └── 输出 → FullStackAgent

2. ArtisticDesignerAgent（视觉设计）
   ├── 选择配色方案
   ├── 设计 UI 组件
   └── 输出 → FullStackAgent

3. FullStackAgent（编码实现）
   ├── 基于架构设计编码
   ├── 应用视觉设计
   └── 输出 → TestAgent

4. CodeGraphAgent（代码分析）
   ├── 分析依赖关系
   ├── 检查代码质量
   └── 输出 → CodeReviewAgent
```

### 3. Agent 集群与三大技能的协作

| Agent | cc-switch | token-stats | master-engineer-hub |
|-------|-----------|-------------|-------------------|
| SkillHubAgent | 加载 cc-switch SKILL | 触发 token 审计 | 接收路由决策 |
| HookAgent | 检查 Provider 切换安全 | 检查预算合规 | 检查路由一致性 |
| AuditAgent | 记录切换日志 | **执行 Token 审计** | 记录路由统计 |
| PlanAgent | 决定何时切换 Provider | 决定预算分配 | **执行路由决策** |

### 4. Agent 生命周期管理

```
Agent 生命周期：
  创建 → 初始化 → 执行 → 完成 → 销毁

创建：
  ├── 由 TraeOrchestrator 按需创建
  └── 分配归属域和工具集

初始化：
  ├── 加载对应 System Prompt 部分
  └── 加载对应 SKILL.md 工具集

执行：
  ├── 接收任务卡
  ├── 执行任务
  └── 产出结果

完成：
  ├── 触发 Hook 检查
  ├── 触发 token 审计
  └── 结果传递给下一个 Agent

销毁：
  ├── 工具从视野移除
  ├── 临时资源清理
  └── 经验记录到引力盆
```