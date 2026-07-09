# System Prompt 引用与依赖图

> 所有 System Prompt 文件的完整引用关系、依赖链路、版本映射。

## 一、System Prompt 文件清单

### 核心 Prompt 文件

| 文件 | 路径 | 类型 | 角色 | 状态 |
|------|------|------|------|------|
| `system-prompt.md` | 01-trae-intelligence/ | 基础定义 | 全局角色、安全约束 | 稳定 |
| `system-prompt-enhanced.md` | 01-trae-intelligence/ | 增强版 | 全能力叠加 | 当前主力 |
| `MASTER_AGENT_ARCHITECTURE.md` | 01-trae-intelligence/ | 架构概述 | 架构总览 | 稳定 |
| `TRAE_ENHANCED_SUMMARY.md` | 01-trae-intelligence/ | 增强摘要 | 能力摘要 | 稳定 |
| `CALL_SCENARIOS.md` | 01-trae-intelligence/ | 调用场景 | 场景映射 | 稳定 |
| `system-validation.md` | 01-trae-intelligence/ | 验证报告 | 131 测试验证 | 已验证 |

### 规则文件

| 文件 | 路径 | 类型 | 角色 | 引用来源 |
|------|------|------|------|---------|
| `agent-rules.md` | 01-trae-intelligence/rules/ | Agent 规则 | 行为约束 | system-prompt.md, system-prompt-enhanced.md |
| `core-rules.md` | 01-trae-intelligence/rules/ | 核心规则 | 安全边界 | system-prompt.md, system-prompt-enhanced.md |
| `decode-rules.md` | 01-trae-intelligence/rules/ | 解码规则 | 格式规范 | system-prompt.md |

### 路由文件

| 文件 | 路径 | 类型 | 角色 | 引用来源 |
|------|------|------|------|---------|
| `dispatch-hub.md` | 01-trae-intelligence/ | 分发枢纽 | 任务分发 | MASTER_AGENT_ARCHITECTURE.md |
| `hook-loop.md` | 01-trae-intelligence/ | Hook 循环 | 工程约束 | MASTER_AGENT_ARCHITECTURE.md |
| `respiratory-system.md` | 01-trae-intelligence/ | 呼吸管道 | 生命周期 | MASTER_AGENT_ARCHITECTURE.md |

### Agent 定义

| 文件 | 路径 | 类型 | 角色 | 引用来源 |
|------|------|------|------|---------|
| `agent-list.md` | 01-trae-intelligence/agents/ | Agent 列表 | 20+ 定义 | skill-hub |
| `core-agents/agent-analysis.md` | 01-trae-intelligence/agents/core/ | 核心分析 | Agent-A 分析 | agent-list.md |
| `core-agents/agent-architect.md` | 01-trae-intelligence/agents/core/ | 架构 Agent | Agent-B 架构 | agent-list.md |
| `core-agents/agent-code.md` | 01-trae-intelligence/agents/core/ | 代码 Agent | Agent-C 编码 | agent-list.md |
| `engineer-agents/agent-react.md` | 01-trae-intelligence/agents/engineer/ | React 开发 | 前端工程 | agent-list.md |
| `engineer-agents/agent-python.md` | 01-trae-intelligence/agents/engineer/ | Python 开发 | 后端工程 | agent-list.md |
| `engineer-agents/agent-data.md` | 01-trae-intelligence/agents/engineer/ | 数据处理 | 数据分析 | agent-list.md |
| `engineer-agents/agent-api.md` | 01-trae-intelligence/agents/engineer/ | API 开发 | 接口工程 | agent-list.md |

### Skill 定义

| 文件 | 路径 | 类型 | 角色 | 引用来源 |
|------|------|------|------|---------|
| `skill-list.md` | 01-trae-intelligence/skills/ | Skill 列表 | 所有 SKILL 文件索引 | skill-hub |
| `skills/cc-switch.md` | 01-trae-intelligence/skills/ | 路由切换 | Provider 管理 | skill-list.md |
| `skills/token-stats.md` | 01-trae-intelligence/skills/ | Token 统计 | 审计预算 | skill-list.md |
| `skills/master-engineer-hub.md` | 01-trae-intelligence/skills/ | 路由编排 | 系统路由 | skill-list.md |
| `skills/lso-os.md` | 01-trae-intelligence/skills/ | 五境递进 | 修炼体系 | skill-list.md |
| `skills/omnipotent-engineer.md` | 01-trae-intelligence/skills/ | 全能工程 | 全栈开发 | skill-list.md |

### SKILL 定义文件（8 核心技能）

| SKILL 文件 | 路径 | MCP 工具数 | 核心机制 |
|-----------|------|-----------|---------|
| `cc-switch` | skills/cc-switch/ | 10 | ccs_provider_switch, ccs_mcp_list, ccs_mcp_toggle, ccs_env_check 等 |
| `token-stats` | skills/token-stats/ | 6 | token_stats_query, token_stats_budget, token_stats_compare 等 |
| `master-engineer-hub` | skills/master-engineer-hub/ | 1（编排入口） | 假借修真路由、机械降神注入 |
| `lso-os` | skills/lso-os/ | 动态 | 五境递进、呼吸管道 |
| `omnipotent-engineer` | skills/omnipotent-engineer/ | 动态 | 一引擎六合一编排 |

## 二、完整依赖关系图

```
system-prompt.md
├── agent-rules.md (规则继承)
├── core-rules.md (安全继承)
├── decode-rules.md (格式继承)
└── system-prompt-enhanced.md (增强叠加)
    ├── agent-rules.md (规则覆盖)
    ├── core-rules.md (安全强化)
    ├── MASTER_AGENT_ARCHITECTURE.md (架构引入)
    │   ├── dispatch-hub.md (任务分发)
    │   ├── hook-loop.md (工程约束)
    │   └── respiratory-system.md (生命周期)
    ├── TRAE_ENHANCED_SUMMARY.md (能力汇总)
    ├── CALL_SCENARIOS.md (场景绑定)
    └── system-validation.md (验证背书)
        └── agent-list.md (Agent 列表)
            ├── core-agents/agent-analysis.md (A 类视野)
            ├── core-agents/agent-architect.md (B 类视野)
            ├── core-agents/agent-code.md (C 类视野)
            └── engineer-agents/* (工程 Agent)
                └── skill-list.md (Skill 列表)
                    ├── skills/cc-switch.md (Provider 路由)
                    ├── skills/token-stats.md (Token 审计)
                    ├── skills/master-engineer-hub.md (编排路由)
                    ├── skills/lso-os.md (五境递进)
                    └── skills/omnipotent-engineer.md (全能工程)
```

### 加载优先级（从根到叶）

加载顺序决定了 Token 的消耗层级。从根 `system-prompt.md` 开始，按需向下加载：

| 加载层 | 文件 | 触发条件 | Token 预估 |
|--------|------|---------|-----------|
| L0（全局常驻） | system-prompt.md, agent-rules.md, core-rules.md | 会话初始化 | ~500 token |
| L1（增强叠加） | system-prompt-enhanced.md, MASTER_AGENT_ARCHITECTURE.md | 复杂任务识别 | ~1200 token |
| L2（路由分发） | dispatch-hub.md, hook-loop.md, respiratory-system.md | 收到用户请求 | ~800 token |
| L3（Agent 定义） | agent-list.md + 对应 Agent | LSO-DAO 判断需要 | ~1500 token |
| L4（Skill 加载） | skill-list.md + 对应 Skill | 路由匹配成功 | ~2000 token |

### 关键依赖链路

以下是四条关键调用链路，展示 System Prompt 如何在请求处理中被动态加载：

**链路 1：简单问答（0 token 直通）**
```
system-prompt.md → agent-rules.md → （直接回复，无需加载更多）
```

**链路 2：开发任务（完整路径）**
```
system-prompt.md → system-prompt-enhanced.md → MASTER_AGENT_ARCHITECTURE.md
→ dispatch-hub.md → hook-loop.md → agent-list.md → core-agents/agent-code.md
→ skill-list.md → skills/omnipotent-engineer.md
```

**链路 3：Provider 切换**
```
system-prompt.md → system-prompt-enhanced.md → MASTER_AGENT_ARCHITECTURE.md
→ dispatch-hub.md → skill-list.md → skills/cc-switch.md
（ccs_provider_switch 调用，涉及 Agent 视野隔离检查）
```

**链路 4：Token 审计**
```
system-prompt.md → system-prompt-enhanced.md → dispatch-hub.md
→ skill-list.md → skills/token-stats.md
（token_stats_query 调用，Hook 2 环节触发）
```

## 三、system-prompt-enhanced.md 加载顺序栈

`system-prompt-enhanced.md` 是当前主力 System Prompt，其内部的加载顺序决定了 Agent 的行为优先级：

```
加载顺序栈（从栈底到栈顶）：
1. [基础角色] → "你是 TRAE，一款集成了 AI 能力的 IDE 助手"
2. [安全约束] → 核心规则、Agent 规则
3. [架构认知] → MASTER_AGENT_ARCHITECTURE.md 内容嵌入
4. [路由分发] → dispatch-hub 逻辑
5. [工程约束] → hook-loop 验收标准
6. [生命周期] → respiratory-system 呼吸管道
7. [能力汇总] → TRAE_ENHANCED_SUMMARY 所有能力
8. [场景映射] → CALL_SCENARIOS 调用场景
9. [增强叠加] → 各 Agent 和 Skill 的增强信息
```

### 版本映射关系

| 版本 | 核心文件 | Skill 版本 | 状态 |
|------|---------|-----------|------|
| v1.0 | system-prompt.md + rules | - | 已归档 |
| v2.0 | system-prompt-enhanced.md | cc-switch v1, token-stats v1 | 已归档 |
| v2.1 | system-prompt-enhanced.md | cc-switch v2, token-stats v2, master-engineer-hub v1 | 已归档 |
| v3.0 | system-prompt-enhanced.md + 全链路 | cc-switch v3, token-stats v3, master-engineer-hub v2, LSO-DAO v1 | **当前版本** |

## 四、引用统计

| 被引用文件 | 被引用次数 | 引用者 |
|-----------|-----------|--------|
| system-prompt.md | 5 | agent-rules, core-rules, decode-rules, system-prompt-enhanced, validation |
| system-prompt-enhanced.md | 3 | agent-rules, core-rules, MASTER_AGENT_ARCHITECTURE |
| MASTER_AGENT_ARCHITECTURE.md | 3 | dispatch-hub, hook-loop, respiratory-system |
| agent-list.md | 5 | 7 core/engineer agents 定义文件 |
| skill-list.md | 6 | 6 skill 定义文件 |
| dispatch-hub.md | 1 | system-prompt-enhanced |

### 引用热力图

高频引用节点（入度 >= 3）：
- **system-prompt.md**（入度 5）— 全局基础，任何请求都必须加载
- **agent-list.md**（入度 5）— Agent 定义枢纽，路由分发的目标索引
- **skill-list.md**（入度 6）— Skill 注册中心，技能调用的入口索引
- **MASTER_AGENT_ARCHITECTURE.md**（入度 3）— 架构理解的基础

这些高入度节点在 System Prompt 中扮演着"高速公路枢纽"的角色，它们的加载效率直接影响整体响应速度。

## 五、补充说明

### 5.1 与三大技能的引用关系

**cc-switch** 的 `ccs_provider_switch` 工具在调用时会触发以下依赖链加载：
```
system-prompt.md → system-prompt-enhanced.md → dispatch-hub.md
→ skill-list.md → skills/cc-switch.md
```
其中 `skills/cc-switch.md` 定义了 10 个 MCP 工具的路由规则和调用参数，需要额外的 ~800 token 加载。

**token-stats** 的 `token_stats_query` 工具主要在 Hook 2（执行中检查）和 Hook 4（完成验收）环节被引用：
```
system-prompt.md → system-prompt-enhanced.md → hook-loop.md
→ skill-list.md → skills/token-stats.md
```

**master-engineer-hub** 作为路由入口，引用路径最为复杂：
```
system-prompt.md → system-prompt-enhanced.md → MASTER_AGENT_ARCHITECTURE.md
→ dispatch-hub.md → agent-list.md → skill-list.md
→ skills/master-engineer-hub.md
```
master-engineer-hub.md 内部又引用了四层路由规则（L0/L1/L2/L3）和引力盆路径表。

### 5.2 延迟加载策略的 Token 优化

| 加载策略 | 触发时机 | 节省 Token | 适用场景 |
|---------|---------|-----------|---------|
| L0 常驻 | 会话开始 | - | 所有请求 |
| L1 按需 | 检测到复杂任务 | ~3000 | 开发任务 |
| L2 条件 | 需要路由分发 | ~5000 | 多步骤任务 |
| L3 惰性 | LSO 第一境判断后 | ~8000 | 单文件修改 |
| L4 精确 | Skill 匹配成功后 | ~12000 | 简单问答 |

### 5.3 Agent 视野隔离在引用中的体现

Agent 视野隔离规则直接影响 System Prompt 的加载范围：

- **Agent-A（分析/设计）**：最多加载 2 个工具定义文件，不可加载 cc-switch 或 token-stats 的工具详情
- **Agent-B（架构/工程）**：最多加载 4 个工具定义文件，可加载 cc-switch 工具但不可加载 token-stats 的预算管理工具
- **Agent-C（编码/文件）**：最多加载 5 个工具定义文件，可同时加载 cc-switch 和 token-stats

这意味着同一个 `agent-list.md` 在不同 Agent 视角下，加载的子节点是不同的：
```
agent-list.md
├── Agent-A 视角 → [agent-analysis.md] （仅加载分析类）
├── Agent-B 视角 → [agent-architect.md + 工程 Agent 部分] 
└── Agent-C 视角 → [agent-code.md + 工程 Agent 全部]
```

### 5.4 版本迁移注意事项

当 System Prompt 版本升级时，需要同步更新的文件依赖链：

1. `system-prompt-enhanced.md` 更新 → 需验证所有子节点引用仍有效
2. 新增 Agent 定义 → 需要在 `agent-list.md` 中注册，并设置对应的视野隔离等级
3. 新增 Skill → 需要在 `skill-list.md` 中注册，并更新 `dispatch-hub.md` 的路由规则
4. Skill 工具变动 → 需更新对应 `skills/*.md` 中的工具签名，并同步到 `cc-switch` 或 `token-stats` 的 MCP 配置

### 5.5 引用循环检测

经检测，当前依赖图不存在引用循环（无环有向图 DAG），所有依赖关系均为单向。这意味着：
- 可以安全地进行延迟加载，不会出现死锁
- 支持并行加载无依赖关系的分支（如 Agent 定义和 Skill 定义可以并行加载）
- 拓扑排序稳定，加载顺序可预测