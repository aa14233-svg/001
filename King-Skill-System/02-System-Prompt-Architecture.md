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

**调用时机**：每次 Agent 初始化时加载，定义全局角色和边界。无论任务简单还是复杂，此层始终加载。

**关键内容**：
- TraeOrchestrator 的全局角色声明
- 安全边界定义：哪些路径禁止访问、哪些操作需要审核
- 5 条工作原则：任务分解、渐进式交付、自我验证、记录决策、优先复用

### 第2层：增强版 System Prompt（system-prompt-enhanced.md）

**定位**：在基础版之上叠加 TraeOrchestrator 主调度 + 三合一拓扑 + Hook+Loop

```
基础 System Prompt
  └── 增强叠加
       ├── TraeOrchestrator 主调度定义
       │    ├── 任务路由分发
       │    ├── Hook+Loop 约束（4个 Hook 节点）
       │    ├── 流程化验收（D1/D2/D3）
       │    └── 全程监工（5项指标：进度、质量、成本、安全、时间）
       ├── 20+ Agent 集群（核心7 + 工程8 + 三合一5）
       │    ├── Agent A 视野（语义/设计）
       │    ├── Agent B 视野（工程/编码）
       │    └── Agent C 视野（文件/搜索）
       ├── 三合一拓扑能力
       │    ├── Program+设计（ProgramDesignerAgent）
       │    ├── 美工（ArtisticDesignerAgent）
       │    └── RTK+Headroom+CodeGraph 协同
       ├── 五境递进 + 呼吸管道
       └── 开销约束（月上限 $15）
```

**调用时机**：当任务需要多 Agent 协作 / 复杂路由 / 质量验收时激活。简单问答类任务不会触发此层。

### 第3层：SkillHub 注册中心（skill-hub.md + skill-registry.md）

**定位**：技能注册 + 领域识别 + 路由分发

```
用户请求
  └── SkillHub 接收
       ├── 领域识别（代码/文档/数据/设计/部署/安全/创意/学习）
       │    ├── 基于关键词匹配（Level 0/1）
       │    ├── 基于语义分析（Level 3）
       │    └── 多域识别（一个请求可能涉及多个域）
       ├── 域激活
       │    └── 激活对应技能域下的所有技能
       ├── 技能匹配
       │    └── 在激活域中匹配最合适的技能
       │         ├── 精确匹配：关键词命中
       │         └── 模糊匹配：语义相似度 > 0.7
       ├── 路由选择
       │    └── 选择对应 Agent 执行（A/B/C 三维度）
       └── 对应 Agent 执行
```

**调用时机**：每个任务必经 SkillHub 单一入口路由，是系统的"交通警察"。

### 第4层：Skill 定义文件（SKILL.md）

**定位**：每个技能的具体定义、工具列表、路由规则

| Skill 文件 | 核心功能 | 工具数 | 路由归属 | 关键工具 |
|-----------|---------|--------|---------|---------|
| `cc-switch-SKILL.md` | Provider 切换 + MCP 管理 | 10 | Agent B/C | ccs_provider_switch, ccs_mcp_sync |
| `token-stats-SKILL.md` | Token 审计 + 预算管理 | 6 | Agent B/C | token_stats_query, token_stats_budget |
| `master-engineer-hub-SKILL.md` | 路由编排 + 假借修真 | 6 | 网关 | 引力盆, 机械降神 |
| `lso-os-SKILL.md` | 五境递进 + Agent 集群 | — | 基础设施 | 筑基→金丹→元婴→化神→渡劫 |
| `omnipotent-engineer-SKILL.md` | 七维全栈开发 | — | 应用层 | 全栈开发 7 维度 |
| `路由skill-SKILL.md` | 三体合一路由 | — | 路由引擎 | 域映射, 五行门控 |
| `ai-coding-optimizer-SKILL.md` | 编码优化工具集 | 5 MCP | 工具层 | 代码分析, 优化建议 |
| `trae-auto-collaboration-SKILL.md` | 双应用协作 | 5 | 桥接 | 跨应用通信 |

**调用时机**：当 SkillHub 匹配到具体技能域时，加载对应 SKILL.md 获取工具定义和路由规则。加载完成后，工具注入到当前 Agent 视野，Agent 获得该技能的全部工具集。

## 二、System Prompt 调用链路

### 完整链路图

```
用户输入 q
  │
  ▼
[1] TraeOrchestrator 接收
  │  ← 加载基础 System Prompt（角色定义 + 安全约束 + 工作原则）
  │
  ▼
[2] Hook 1：任务准备
  │  ← 安全检查 + 资源确认 + 依赖检查 + 权限验证
  │  ├── 路径是否在禁区？
  │  ├── 文件/资源是否存在？
  │  ├── 依赖是否满足？
  │  └── 是否有权限执行？
  │
  ▼
[3] SkillHub 路由
  │  ← 加载增强版 System Prompt（全能力）
  │  ├── master-engineer-hub 路由决策
  │  │    ├── Level 0: 引力盆命中 → 直通（0 token）
  │  │    │    └── 如 "切换 Provider" 命中固化路由
  │  │    ├── Level 1: 特征词典命中 → 路由
  │  │    │    └── 如 `查.*日志` 匹配到 Grep
  │  │    ├── Level 2: 工具签名匹配
  │  │    │    └── 如 `ccs_` 前缀匹配 cc-switch 工具
  │  │    └── Level 3: LLM 兜底 → 机械降神
  │  │         └── 语义解析 → 能力缺口检测 → 按需注入
  │  │
  │  └── 路由结果 → 对应 Agent 归属
  │       └── { track: "A|B|C", confidence: 0-1 }
  │
  ▼
[4] Agent 执行
  │  ← 加载对应 SKILL.md（技能定义）
  │  ├── Agent B（工程）→ 编码/部署/测试
  │  │    ├── 可用工具：ccs_provider_switch, RunCommand 等
  │  │    └── 工具上限：≤4
  │  ├── Agent C（搜索）→ 文件/搜索/工具
  │  │    ├── 可用工具：Read, Grep, Glob, ccs_provider_list 等
  │  │    └── 工具上限：≤5
  │  └── Agent A（设计）→ 语义/设计/构思
  │       ├── 可用工具：brainstorm, writing-plans
  │       └── 工具上限：≤2
  │
  ▼
[5] 五境递进（lso-os 基础设施）
  │  ← 加载 LSO-DAO 五境架构
  │  ├── 筑基（类型推导）→ 确定任务类型
  │  ├── 金丹（安全扫描）→ 安全边界检查
  │  ├── 元婴（五行门控+域映射）→ 门控决策
  │  ├── 化神（技能路由+负载均衡）→ 执行路由
  │  └── 渡劫（内省+演化+验收）→ 自我验证
  │
  ▼
[6] Hook 检查 + D1/D2/D3 验收
  │  ← 执行中 Hook → 验收 Hook → 完成 Hook
  │  ├── Hook 2：执行中 → 进度/质量/风险检查
  │  ├── Hook 3：验收 → D1/D2/D3 验收
  │  └── Hook 4：完成 → 完整检查/文档/清理
  │
  ▼
[7] 交付结果
  │  ← Token 审计（token-stats 自动记录）
  │  ← 引力盆更新（高频路径固化）
  │  ← 返回结果给用户
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
  │              │              │              ├── cc-switch-SKILL.md (10工具)
  │              │              │              ├── token-stats-SKILL.md (6工具)
  │              │              │              ├── master-engineer-hub-SKILL.md (6工具)
  │              │              │              ├── lso-os-SKILL.md (基础设施)
  │              │              │              ├── omnipotent-engineer-SKILL.md (应用层)
  │              │              │              └── 路由skill-SKILL.md (路由引擎)
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
  │              └──依赖──► system-validation.md (验证报告 - 131测试通过)
  │
  ├──依赖──► agent-rules.md (Agent 规则 - 行为约束)
  │
  ├──依赖──► core-rules.md (核心规则 - 安全边界)
  │
  └──依赖──► SYSTEM_ARCHITECTURE.md (系统架构)
```

完整依赖共 **22 个文件**，其中 16 个在初始化时按序加载，6 个 SKILL.md 在运行时按需加载。

## 四、调用逻辑的核心原则

### 1. 延迟加载

System Prompt 不是一次性全部加载，而是按需分层加载：

```
第1层：永远加载          → 基础角色定义（~2KB）
第2层：任务复杂时加载    → 增强能力（~8KB）
第3层：路由决策时加载    → SkillHub（~3KB）
第4层：技能匹配后加载    → 对应 SKILL.md（~5KB/个）
```

**优势**：
- 减少不必要的 Token 消耗
- 降低每次请求的初始延迟
- Agent 视野保持精简，避免信息过载

### 2. 视野隔离

不同 Agent 只看到属于自己的 System Prompt 部分：

| Agent | 可见 Prompt 范围 | 可见工具 | 工具上限 |
|-------|----------------|---------|---------|
| Agent A（语义/设计） | 构思、设计、写作相关 | brainstorm, writing-plans | ≤2 |
| Agent B（工程/编码） | 编码、部署、测试相关 | ccs_provider_switch, ccs_mcp_sync, ccs_skills_sync, ccs_provider_test, token_stats_budget, token_stats_alert, RunCommand | ≤4 |
| Agent C（文件/搜索） | 搜索、文件、工具相关 | Read, Grep, Glob, Search, ccs_provider_list, ccs_provider_current, ccs_mcp_list, ccs_skills_list, ccs_config_show, ccs_env_check, token_stats_query, token_stats_compare, token_stats_audit, token_stats_report | ≤5 |

**Agent A 不可见任何 cc-switch 和 token-stats 工具**。这种隔离机制防止了 Agent 越权调用，是系统安全的核心保障。

### 3. 引力盆固化

高频调用路径自动固化到 Level 0/1，**0 token 直通**，无需重复加载 System Prompt 链。

```
引力盆示例数据：
{ "pattern": "切换.*provider", "action": "ccs_provider_switch", "hit": 12 }
{ "pattern": "查.*日志",       "action": "Grep",                "hit": 8  }
{ "pattern": "=> tool_list",   "action": "ccs_mcp_list",        "hit": 15 }
{ "pattern": "换.*模型",       "action": "ccs_provider_switch", "hit": 7  }
{ "pattern": "预算.*够",       "action": "token_stats_budget",  "hit": 5  }
```

引力盆数据存储在 `master-engineer-hub` 的技能上下文中，每次任务完成后由 `token-stats` 自动触发审计并更新频次。

## 五、System Prompt 版本管理

| 版本 | 文件 | 状态 | 关键变更 |
|------|------|------|---------|
| v1.0 | system-prompt.md | 基础版，稳定 | 初始版本，角色定义 + 安全约束 |
| v2.0 | system-prompt-enhanced.md | 增强版，当前主力 | 全能力叠加，TraeOrchestrator 主调度 |
| v2.1 | 新增三合一拓扑 | 已验证，131 测试通过 | Program+设计、美工、RTK 协同 |
| v2.2 | 新增 Hook+Loop | 集成中 | 4 个 Hook 节点 + D1/D2/D3 验收 |
| v3.0 | 待定 | 规划中 | 计划引入自适应路由权重、多模态支持 |

---

## 补充说明

### 1. cc-switch 与 System Prompt 的联动

当 master-engineer-hub 识别到 "Provider" 相关请求时：
1. 加载 `cc-switch-SKILL.md`
2. 将 cc-switch 的 10 个工具注入到当前 Agent 视野
3. Agent B 获得 4 个写操作工具，Agent C 获得 6 个读操作工具
4. 执行完毕，工具从 Agent 视野中移除（按需注入原则）

### 2. token-stats 与 System Prompt 的联动

每次任务完成时自动触发：
1. 调用 `token_stats_audit` 审计本次任务 Token 消耗
2. 对比月预算 $15 剩余
3. 如果消耗接近阈值（80%），触发告警
4. 记录到引力盆，更新高频路径频次

### 3. System Prompt 加载的性能考量

| 加载场景 | 加载文件数 | 平均加载时间 | Token 消耗 |
|---------|-----------|------------|-----------|
| Level 0 引力盆命中 | 0 (无加载) | 0ms | **0 token** |
| 简单问答 | 1 (基础 SP) | ~10ms | ~2KB |
| 编码任务 | 3 (基础+增强+SKILL) | ~30ms | ~15KB |
| 复杂多 Agent 任务 | 6 (全部) | ~60ms | ~30KB |
| 机械降神 | 6 + 动态注入 | ~100ms+ | ~50KB+ |

### 4. Agent 视野隔离的实现机制

```
master-engineer-hub 路由决策
  └── 路由结果 → { track: "A", confidence: 0.95 }
       └── 注入 Agent A 视野
            ├── 可见工具：brainstorm, writing-plans
            ├── 不可见：ccs_provider_switch, Read, Grep ...
            └── 视野切换：Agent A 看不到 B/C 的工具

视野切换过程：
  1. 当前 Agent 完成自己的任务
  2. 输出结果传递给下一个 Agent
  3. 下一个 Agent 加载自己视野内的工具集
  4. 上一个 Agent 的工具集从视野中移除
```