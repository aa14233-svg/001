# System Prompt 架构与调用逻辑

> King Agent 的 System Prompt 不是一段文本，而是一套**分层调用体系**。

## 一、System Prompt 四层结构

### 第1层：基础 System Prompt（system-prompt.md）

**定位**：角色定义 + 核心架构 + 安全约束 + 工作原则

```
角色定义
  └── Trae 一体化超强智能体
       ├── LSO-Dao 五境递进架构
       ├── Trae Agent 集群（15个核心 Agent）
       ├── 呼吸管道工作流
       ├── SkillHub 技能系统（8大技能组）
       ├── 安全约束（路径禁区 + 高危拦截 + 配置保护）
       └── 工作原则（5条）
```

**调用时机**：每次 Agent 初始化时加载，定义全局角色和边界。

### 第2层：增强版 System Prompt（system-prompt-enhanced.md）

**定位**：在基础版之上叠加 TraeOrchestrator 主调度 + 三合一拓扑 + Hook+Loop

```
基础 System Prompt
  └── 增强叠加
       ├── TraeOrchestrator 主调度定义
       │    ├── 任务路由分发
       │    ├── Hook+Loop 约束（4个 Hook 节点）
       │    ├── 流程化验收（D1/D2/D3）
       │    └── 全程监工（5项指标）
       ├── 20+ Agent 集群（核心7 + 工程8 + 三合一5）
       ├── 三合一拓扑能力
       │    ├── Program+设计（ProgramDesignerAgent）
       │    ├── 美工（ArtisticDesignerAgent）
       │    └── RTK+Headroom+CodeGraph 协同
       ├── 五境递进 + 呼吸管道
       └── 开销约束（月上限 $15）
```

**调用时机**：当任务需要多 Agent 协作 / 复杂路由 / 质量验收时激活。

### 第3层：SkillHub 注册中心（skill-hub.md + skill-registry.md）

**定位**：技能注册 + 领域识别 + 路由分发

```
用户请求
  └── SkillHub 接收
       ├── 领域识别（代码/文档/数据/设计/部署/安全/创意/学习）
       ├── 域激活
       ├── 技能匹配
       ├── 路由选择
       └── 对应 Agent 执行
```

**调用时机**：每个任务必经 SkillHub 单一入口路由。

### 第4层：Skill 定义文件（SKILL.md）

**定位**：每个技能的具体定义、工具列表、路由规则

| Skill 文件 | 核心功能 | 工具数 | 路由归属 |
|-----------|---------|--------|---------|
| `cc-switch-SKILL.md` | Provider 切换 + MCP 管理 | 10 | Agent B/C |
| `token-stats-SKILL.md` | Token 审计 + 预算管理 | 6 | Agent B/C |
| `master-engineer-hub-SKILL.md` | 路由编排 + 假借修真 | 6 | 网关 |
| `lso-os-SKILL.md` | 五境递进 + Agent 集群 | — | 基础设施 |
| `omnipotent-engineer-SKILL.md` | 七维全栈开发 | — | 应用层 |
| `路由skill-SKILL.md` | 三体合一路由 | — | 路由引擎 |
| `ai-coding-optimizer-SKILL.md` | 编码优化工具集 | 5 MCP | 工具层 |
| `trae-auto-collaboration-SKILL.md` | 双应用协作 | 5 | 桥接 |

**调用时机**：当 SkillHub 匹配到具体技能域时，加载对应 SKILL.md 获取工具定义和路由规则。

## 二、System Prompt 调用链路

### 完整链路图

```
用户输入 q
  │
  ▼
[1] TraeOrchestrator 接收
  │  ← 加载基础 System Prompt（角色定义）
  │
  ▼
[2] Hook 1：任务准备
  │  ← 安全检查 + 资源确认 + 依赖检查
  │
  ▼
[3] SkillHub 路由
  │  ← 加载增强版 System Prompt（全能力）
  │  ├── master-engineer-hub 路由决策
  │  │    ├── Level 0: 引力盆命中 → 直通（0 token）
  │  │    ├── Level 1: 特征词典 → 路由
  │  │    ├── Level 2: 工具签名匹配
  │  │    └── Level 3: LLM 兜底 → 机械降神
  │  │
  │  └── 路由结果 → 对应 Agent 归属
  │
  ▼
[4] Agent 执行
  │  ← 加载对应 SKILL.md（技能定义）
  │  ├── Agent B（工程）→ 编码/部署/测试
  │  ├── Agent C（搜索）→ 文件/搜索/工具
  │  └── Agent A（设计）→ 语义/设计/构思
  │
  ▼
[5] 五境递进（lso-os 基础设施）
  │  ← 加载 LSO-DAO 五境架构
  │  ├── 筑基（类型推导）
  │  ├── 金丹（安全扫描）
  │  ├── 元婴（五行门控+域映射）
  │  ├── 化神（技能路由+负载均衡）
  │  └── 渡劫（内省+演化+验收）
  │
  ▼
[6] Hook 检查 + D1/D2/D3 验收
  │  ← 执行中 Hook → 验收 Hook → 完成 Hook
  │
  ▼
[7] 交付结果
```

## 三、System Prompt 依赖关系图

```
system-prompt.md (基础)
  │
  ├──依赖──► system-prompt-enhanced.md (增强)
  │              │
  │              ├──依赖──► skill-hub.md (技能路由)
  │              │              │
  │              │              ├──依赖──► skill-registry.md (注册中心)
  │              │              │              │
  │              │              │              ├── cc-switch-SKILL.md
  │              │              │              ├── token-stats-SKILL.md
  │              │              │              ├── master-engineer-hub-SKILL.md
  │              │              │              ├── lso-os-SKILL.md
  │              │              │              ├── omnipotent-engineer-SKILL.md
  │              │              │              └── 路由skill-SKILL.md
  │              │              │
  │              │              └── skills-index.md (技能索引)
  │              │
  │              ├──依赖──► MASTER_AGENT_ARCHITECTURE.md
  │              │              ├── TraeOrchestrator 调度
  │              │              ├── Hook+Loop 约束
  │              │              └── D1/D2/D3 验收
  │              │
  │              ├──依赖──► CALL_SCENARIOS.md (调用场景)
  │              │
  │              └──依赖──► system-validation.md (验证报告)
  │
  ├──依赖──► agent-rules.md (Agent 规则)
  │
  ├──依赖──► core-rules.md (核心规则)
  │
  └──依赖──► SYSTEM_ARCHITECTURE.md (系统架构)
```

## 四、调用逻辑的核心原则

### 1. 延迟加载

System Prompt 不是一次性全部加载，而是按需分层加载：

```
第1层：永远加载          → 基础角色定义
第2层：任务复杂时加载    → 增强能力
第3层：路由决策时加载    → SkillHub
第4层：技能匹配后加载    → 对应 SKILL.md
```

### 2. 视野隔离

不同 Agent 只看到属于自己的 System Prompt 部分：

| Agent | 可见 Prompt 范围 |
|-------|----------------|
| Agent A（语义/设计） | 构思、设计、写作相关 |
| Agent B（工程/编码） | 编码、部署、测试相关 |
| Agent C（文件/搜索） | 搜索、文件、工具相关 |

### 3. 引力盆固化

高频调用路径自动固化到 Level 0/1，**0 token 直通**，无需重复加载 System Prompt 链。

## 五、System Prompt 版本管理

| 版本 | 文件 | 状态 |
|------|------|------|
| v1.0 | system-prompt.md | 基础版，稳定 |
| v2.0 | system-prompt-enhanced.md | 增强版，当前主力 |
| v2.1 | 新增三合一拓扑 | 已验证，131 测试通过 |
| v2.2 | 新增 Hook+Loop | 集成中 |
| v3.0 | 待定 | 规划中 |