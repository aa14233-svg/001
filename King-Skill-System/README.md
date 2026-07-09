# King-Skill 技能群系系统

> 以 King Agent 为中心拓扑展开的大智能体技能路由群系系统。
>
> **核心哲学**：不持一物，而万物皆备。不预一器，而万器待召。
>
> 📖 本系统的理论原型与底层原理详见根 README **第一部分：教材体系详解（一体分二式）** ，建议先读教材 Phase 3 AG-06（Agent 持久记忆与智能编排）再回看本系统以建立完整认知。

## 一、系统概述

King-Skill 是一套以 **King Agent（一体超强智能体）** 为中心拓扑，整合 **cc-switch、token-stats、master-engineer-hub** 三大核心技能，辅以 8 大技能组、20+ Agent 集群、LSO-DAO 五境递进基础设施的完整 **AI 智能体技能路由生态**。

### 系统架构全景

```
                    ┌─────────────────────────────────────┐
                    │         用户请求（自然语言）           │
                    └──────────────┬──────────────────────┘
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       master-engineer-hub            │
                    │   （四层路由引擎：L0/L1/L2/L3）       │
                    └──────┬──────────────┬───────────────┘
                           │              │
               ┌───────────▼────┐   ┌────▼──────────────┐
               │   cc-switch    │   │    token-stats     │
               │ (Provider 管理) │   │  (Token 审计预算)  │
               │  10 MCP 工具   │   │   6 MCP 工具       │
               └─────────┬──────┘   └────┬──────────────┘
                         │               │
               ┌─────────▼───────────────▼────────────────┐
               │          SkillHub（8 大技能组）             │
               │    域识别 → 技能匹配 → 路由分发              │
               └─────────────────┬────────────────────────┘
                                 │
               ┌─────────────────▼────────────────────────┐
               │        20+ Agent 集群（7+8+5+3）          │
               │  A类(分析/设计) │ B类(架构/工程) │ C类(编码)│
               └─────────────────┬────────────────────────┘
                                 │
               ┌─────────────────▼────────────────────────┐
               │    LSO-DAO 五境递进 + 呼吸管道             │
               │  筑基→金丹→元婴→化神→渡劫                  │
               └─────────────────┬────────────────────────┘
                                 │
               ┌─────────────────▼────────────────────────┐
               │    Hook+Loop 质量体系（4 Hook + D1/D2/D3） │
               │   引力盆 0 token 直通 90% 请求             │
               └──────────────────────────────────────────┘
```

### 核心设计原则

| 原则 | 说明 | 涉及组件 |
|------|------|---------|
| **假借修真** | 借用 LLM 能力实现智能体路由，90% 请求 0 token 完成 | 引力盆、四层路由 |
| **机械降神** | 按需注入能力，用完即回收，一次部署一次性生命周期 | 技能注入、Agent 编排 |
| **Token 即货币** | 所有消耗都以 Token 计量，路由优先级 = Token 成本排序 | token-stats、路由规则 |
| **视野隔离** | Agent 所见即所得，限制工具暴露范围，避免能力溢出 | A/B/C 三类 Agent 隔离 |
| **延迟加载** | System Prompt 分 5 层按需加载，从 L0 到 L4 精确匹配 | 依赖链、引用图 |

### 技术栈

| 组件 | 技术 | Token 成本 |
|------|------|-----------|
| 路由引擎 | master-engineer-hub（假借修真 + 机械降神） | L0: 0 token, L1: 0 token, L2: ≤50 token, L3: 全量 |
| Provider 管理 | cc-switch（10 MCP 工具） | ~200 token/次切换 |
| Token 审计 | token-stats（6 MCP 工具） | ~50 token/次查询 |
| 基础设施 | LSO-DAO 五境递进 + 呼吸管道 | ~300 token/完整周期 |
| 执行层 | 20+ Agent 集群（7 核心 + 8 工程 + 5 三合一 + 3 路由） | 按 Agent 类型 50-500 token 不等 |
| 技能层 | SkillHub 8 大技能组（20+ SKILL 文件） | ~500 token/技能加载 |
| 质量体系 | Hook+Loop + D1/D2/D3 三级验收 | ~100 token/次验收 |
| 经济体系 | 引力盆 0 token 直通 90% 请求 | 0 token |

### 与传统架构的对比

| 维度 | 传统 LLM 调用 | King-Skill 系统 |
|------|-------------|----------------|
| 路由策略 | 单一路由或硬编码 | 四层递进路由 + 引力盆自动路径固化 |
| 技能管理 | 手动选择或固定 Pipeline | SkillHub 自动域识别 + 技能匹配 |
| Token 控制 | 无全局预算管理 | token-stats 全链路审计 + 预算告警 |
| Agent 分工 | 单一 Agent 处理所有任务 | 20+ Agent 分工协作 + 视野隔离 |
| 质量保障 | 人工 Review | Hook+Loop 自动化 + D1/D2/D3 三级验收 |
| 能力扩展 | 需修改代码部署 | 机械降神动态注入 + 引力盆路径学习 |

## 二、文件索引

| 文件 | 内容 | 核心概念 | 推荐读者 |
|------|------|---------|---------|
| `01-King-Agent-Design.md` | King Agent 设计理念 | 假借修真、机械降神、Hook+Loop、D1/D2/D3 | 所有读者 |
| `02-System-Prompt-Architecture.md` | System Prompt 架构与调用逻辑 | 四层结构、延迟加载、视野隔离、依赖链 | 架构师、研究者 |
| `03-Routing-Topology.md` | 三技能路由拓扑 | cc-switch、token-stats、master-engineer-hub 联动 | 所有读者 |
| `04-SkillHub-Registry.md` | SkillHub 技能注册与分发 | 8 大技能组、注册流程、匹配机制 | 开发者 |
| `05-LSO-DAO-Five-Realms.md` | 五境递进架构 | 筑基→金丹→元婴→化神→渡劫、呼吸管道 | 架构师 |
| `06-Agent-Cluster-20plus.md` | 20+ Agent 集群体系 | 核心 7 + 工程 8 + 三合一 5 + 路由 3 | 开发者、架构师 |
| `07-Hook-Loop-Engineering.md` | Hook+Loop 工程约束 | 4 Hook、D1/D2/D3、3 次循环 | 开发者、架构师 |
| `08-Complete-Topology-Map.md` | 完整拓扑图 | 7 层架构、数据流路径、依赖关系 | 所有读者 |
| `09-System-Prompt-References.md` | System Prompt 引用与依赖图 | 22 文件依赖链、引用统计 | 架构师、研究者 |
| `10-Migration-Deployment-Guide.md` | **跨工具迁移部署指南** | 全系路由系统迁移到 Cursor/VS Code/Windsurf 等平台 + 嵌入式模型自适应机制 | 开发者、架构师 |
| `11-Self-Cognitive-OS-Manual.md` | **自我认知操作系统脉络手册** | AI 智能体自解构使用说明书 / CodeGraph 类认知图谱 / 人机交互逻辑 | 所有读者、AI 研究者 |

## 三、快速路径

### 新手入门

```
├── 01-King-Agent-Design.md → 理解 King Agent 是什么（核心概念）
├── 03-Routing-Topology.md  → 理解三大技能如何协作（路由机制）
└── 08-Complete-Topology-Map.md → 看全景图（系统概览）
```

### 进阶学习

```
├── 02-System-Prompt-Architecture.md → 深入 System Prompt 体系
│   （理解 4 层 System Prompt 如何加载和调度）
├── 04-SkillHub-Registry.md         → 技能注册与路由机制
│   （掌握 8 大技能组的注册和匹配流程）
├── 05-LSO-DAO-Five-Realms.md       → 五境递进修炼体系
│   （理解筑基到渡劫的完整修炼路径）
└── 06-Agent-Cluster-20plus.md      → 了解 20+ Agent 分工
    （熟悉 A/B/C 三类 Agent 的视野隔离和职责划分）
```

### 系统设计

```
├── 07-Hook-Loop-Engineering.md     → 质量保障体系
│   （深入理解 4 Hook 节点和 D1/D2/D3 验收标准）
├── 09-System-Prompt-References.md  → 文件依赖关系
│   （查看 22 文件的完整依赖图和引用热力图）
└── 08-Complete-Topology-Map.md     → 完整拓扑设计
    （从 7 层架构看系统的全貌和设计意图）
```

## 四、与三大技能的关系

| 技能 | 在 King-Skill 中的角色 | 关键文件 | MCP 工具数 | 核心 API |
|------|----------------------|---------|-----------|---------|
| **cc-switch** | Provider 路由 + MCP 管理 | 03-Routing-Topology.md | 10 | ccs_provider_switch, ccs_mcp_list, ccs_mcp_toggle, ccs_env_check |
| **token-stats** | Token 审计 + 预算管理 | 03-Routing-Topology.md | 6 | token_stats_query, token_stats_budget, token_stats_compare, token_stats_alert |
| **master-engineer-hub** | 路由编排引擎 | 01 + 03 + 08 | 1（编排入口） | 四层路由 L0/L1/L2/L3 |
| **lso-os** | 五境递进基础设施 | 05 | 动态 | 五行门控、呼吸管道 |
| **omnipotent-engineer** | 全栈开发执行 | 06 | 动态 | 一引擎六合一编排 |

### 三大技能的数据流关系

```
用户请求
    │
    ▼
master-engineer-hub（路由入口）
    │
    ├── Level 0: 引力盆直通（70% 请求，0 token）
    │   └── 从历史成功路径中直接匹配
    │
    ├── Level 1: 特征词典匹配（20% 请求，0 token）
    │   └── 关键词 + 域识别 → 匹配 Skill
    │
    ├── Level 2: 工具签名匹配（5% 请求，≤50 token）
    │   └── 精确参数匹配 → cc-switch/token-stats 调用
    │
    └── Level 3: LLM 裁决（5% 请求，全量 token）
        └── LLM 分析 → 路由分发 → 结果反馈 → 引力盆路径固化
```

## 五、补充说明

### 5.1 系统定位与使用场景

King-Skill 系统适用于以下场景：

| 场景 | 说明 | 优势 |
|------|------|------|
| **IDE 智能助手** | 在 Trae IDE 中提供 AI 辅助编程 | 20+ Agent 分工，覆盖全开发流程 |
| **多 Provider 管理** | 切换不同 LLM Provider 的复杂场景 | cc-switch 提供统一的 Provider 管理接口 |
| **Token 成本控制** | 需要精确控制 Token 消耗的企业场景 | token-stats 全链路审计 + 预算告警 |
| **复杂多步骤任务** | 需要多个 Agent 协作的复杂任务 | LSO-DAO 五境递进确保任务质量 |
| **系统集成** | 将 AI 能力集成到现有工作流 | Hook+Loop 提供标准化的质量保障接口 |

### 5.2 与 cc-switch 技能的深度关系

cc-switch 在 King-Skill 中提供 Provider 路由能力，其 10 个 MCP 工具分布在以下 Agent 视野中：

- **Agent-A 视野**：仅可见 `ccs_env_check`（环境检测），用于分析阶段判断可用 Provider
- **Agent-B 视野**：可见 `ccs_provider_switch`、`ccs_mcp_list`、`ccs_mcp_toggle`、`ccs_env_check`（4 个），用于架构阶段的 Provider 选择和切换
- **Agent-C 视野**：可见全部 10 个工具，包括 `ccs_config_export`、`ccs_config_import` 等高级管理工具

### 5.3 与 token-stats 技能的深度关系

token-stats 在 King-Skill 中提供全链路 Token 审计，其 6 个 MCP 工具被嵌入到 Hook+Loop 体系中：

- **Hook 1（准备阶段）**：调用 `token_stats_budget_check` 检查预算
- **Hook 2（执行阶段）**：调用 `token_stats_query` 监控 Token 消耗
- **Hook 4（完成阶段）**：调用 `token_stats_compare` 对比预估和实际消耗

### 5.4 与 master-engineer-hub 技能的深度关系

master-engineer-hub 是整个系统的路由入口，其四层路由机制与 King-Skill 的每个组件都有交互：

1. **L0 引力盆**：存储的是 "域 → Agent → Skill" 的成功路径对，路径会随使用频率自动固化
2. **L1 特征词典**：包含 King-Skill 所有 8 大技能组的域关键词和触发条件
3. **L2 工具签名**：精确匹配 cc-switch 和 token-stats 的工具参数签名
4. **L3 LLM 裁决**：作为兜底，将未知请求交由 LLM 分析并生成新的路由路径

### 5.5 扩展与定制

要扩展 King-Skill 系统，可以：

1. **新增 Agent**：在 `06-Agent-Cluster-20plus.md` 中定义，设置视野隔离等级（A/B/C），注册到 `agent-list.md`
2. **新增 Skill**：在 `04-SkillHub-Registry.md` 中注册，定义域关键词，关联到对应的 Agent
3. **新增 Provider**：通过 cc-switch 的 `ccs_provider_switch` 工具注册新的 LLM Provider
4. **自定义 Hook**：在 `07-Hook-Loop-Engineering.md` 的 4 个 Hook 节点中插入自定义检查逻辑

### 5.6 性能指标参考

| 指标 | 期望值 | 测量方法 | 优化建议 |
|------|-------|---------|---------|
| L0 引力盆命中率 | ≥70% | token-stats 统计 | 增加使用频率，路径固化 |
| L0+L1 总命中率 | ≥90% | token-stats 统计 | 完善特征词典 |
| Agent 调用成功率 | ≥95% | Hook 2 检查 | 检查 Agent 视野隔离配置 |
| Hook 验收通过率 | ≥90% | D1/D2/D3 统计 | 调整验收标准阈值 |
| 端到端响应时间 | ≤5s | 呼吸管道计时 | 优化延迟加载策略 |