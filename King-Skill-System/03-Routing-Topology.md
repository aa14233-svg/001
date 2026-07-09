# 三技能路由拓扑：cc-switch · token-stats · master-engineer-hub

> 这三个技能构成了 King Agent 的**路由调度、Provider 管理、Token 审计**三位一体核心。

## 一、三大技能定位

```
                   ┌──────────────────────────┐
                   │   master-engineer-hub     │
                   │     (路由编排引擎)         │
                   │  假借修真 + 机械降神       │
                   │  引力盆 + Agent 视野隔离   │
                   └───────────┬──────────────┘
                               │
               ┌───────────────┼───────────────┐
               │               │               │
               ▼               ▼               ▼
     ┌─────────────────┐ ┌──────────┐ ┌──────────────┐
     │   cc-switch     │ │ token-   │ │ omnipotent-  │
     │  Provider 切换   │ │ stats    │ │ engineer     │
     │  MCP 管理       │ │ Token    │ │ 全栈工程     │
     │  Skills 同步     │ │ 审计预算  │ │ 七维设计     │
     └─────────────────┘ └──────────┘ └──────────────┘
                               │
                               ▼
                     ┌──────────────────┐
                     │     lso-os       │
                     │  五境递进基础设施  │
                     │  20+ Agent 集群   │
                     └──────────────────┘
```

## 二、cc-switch：Provider 路由与 MCP 管理

### 设计理念

cc-switch 是 AI Provider 的"交换机"——负责在不同模型提供商之间切换路由，同时管理所有 MCP Server 的同步。

### 工具列表（10个）

| 工具 | 归属 | 功能 | 调用场景 |
|------|------|------|---------|
| `ccs_provider_list` | Agent C | 列出所有 Provider | 查看可用模型 |
| `ccs_provider_switch` | Agent B | 切换到指定 Provider | 换模型干活 |
| `ccs_provider_current` | Agent C | 查看当前激活的 Provider | 确认当前模型 |
| `ccs_provider_test` | Agent B | 测试 Provider API 连通性 | 故障排查 |
| `ccs_mcp_sync` | Agent B | 同步 MCP 配置到目标应用 | 新增/更新 MCP |
| `ccs_mcp_list` | Agent C | 列出管理的所有 MCP Server | 查看 MCP 状态 |
| `ccs_skills_list` | Agent C | 列出已安装的 Skills | 技能盘点 |
| `ccs_skills_sync` | Agent B | 同步 Skills 到目标应用目录 | 技能更新 |
| `ccs_config_show` | Agent C | 查看全局配置 | 系统诊断 |
| `ccs_env_check` | Agent C | 检查本地 CLI 工具链状态 | 环境诊断 |

### System Prompt 调用逻辑

```
master-engineer-hub 路由决策
  └── 识别到 "Provider" 相关请求
       └── 加载 cc-switch-SKILL.md
            ├── Agent B 可用：ccs_provider_switch / ccs_mcp_sync / ccs_skills_sync / ccs_provider_test
            ├── Agent C 可用：ccs_provider_list / ccs_provider_current / ccs_mcp_list / ccs_skills_list / ccs_config_show / ccs_env_check
            └── 执行对应工具 → 返回结果
```

## 三、token-stats：Token 审计与预算管理

### 设计理念

token-stats 是 King Agent 的"财务总监"——监控每次任务调用的 Token 消耗、Provider 成本对比、预算上限管理。

### 工具列表（6个）

| 工具 | 归属 | 功能 | 调用场景 |
|------|------|------|---------|
| `token_stats_query` | Agent C | 查询 Token 用量统计 | 统计总消耗 |
| `token_stats_compare` | Agent C | 对比各 Provider 成本 | 选最便宜的 |
| `token_stats_budget` | Agent B | 查看/设置预算上限 | 月预算 $15 管理 |
| `token_stats_audit` | Agent C | 审计 Token 消费记录 | 审查异常消耗 |
| `token_stats_report` | Agent C | 生成 Token 使用报告 | 周报/月报 |
| `token_stats_alert` | Agent B | 配置用量告警阈值 | 超额预警 |

### System Prompt 调用逻辑

```
每次任务完成后
  └── 自动触发 token_stats_audit
       ├── 记录本次任务 Token 消耗
       ├── 对比预算剩余
       ├── 如果接近阈值 → 触发告警
       └── 更新使用报告
```

### 与 cc-switch 的联动

```
用户请求 "换到便宜的 Provider"
  └── master-engineer-hub 路由
       ├── 调用 token_stats_compare → 对比各 Provider 成本
       ├── 选择最优 Provider
       └── 调用 ccs_provider_switch → 切换到该 Provider
```

## 四、master-engineer-hub：路由编排引擎

### 设计理念

master-engineer-hub 是 King Agent 的"大脑"——不直接执行任务，而是决定"谁来执行"。

> 不持一物，而万物皆备。不预一器，而万器待召。

### 路由本体

```
输入 q
 │
 ├── 假借修真(q) ?  →  引力盆命中  →  参数映射  →  execute
 │                       Level 0/1  0 token 直通
 │
 └── ¬∃ 假借修真(q)  →  机械降神协议
        ↓
     特征词典 L1 ?  →  yes → 路由 A/B/C
     工具签名 L2 ?  →  yes → 路由 A/B/C
     LLM 兜底 L3    →  解析意图
                        ↓
                    能力缺口检测
                        ↓
                 缺口 ∈ Agent 视野？  →  no → GitHub 检索 → 坍缩注入
                                             │
                                          预取身诞生
                                             │
                                          execute → 消散
```

### Agent 视野隔离

| Agent | 归属工具 | 上限 |
|-------|---------|------|
| **A** 语义/设计 | `brainstorm`, `writing-plans` | ≤2 |
| **B** 工程/编码 | `ccs_provider_switch`, `ccs_mcp_sync`, `ccs_skills_sync`, `ccs_provider_test`, `token_stats_budget`, `token_stats_alert`, `RunCommand` | ≤4 |
| **C** 文件/搜索 | `Read`, `Grep`, `Glob`, `Search`, `ccs_provider_list`, `ccs_provider_current`, `ccs_mcp_list`, `ccs_skills_list`, `ccs_config_show`, `ccs_env_check`, `token_stats_query`, `token_stats_compare`, `token_stats_audit`, `token_stats_report` | ≤5 |

### 引力盆（动态固化）

```
高频模式自动固化到 Level 0/1，0 token 直通：

{ "pattern": "切换.*provider", "action": "ccs_provider_switch", "hit": 12 }
{ "pattern": "查.*日志",       "action": "Grep",                "hit": 8  }
{ "pattern": "=> tool_list",   "action": "ccs_mcp_list",        "hit": 15 }
```

### System Prompt 调用逻辑

```
master-engineer-hub 是唯一的"入口网关"
  ├── 每次请求最先经过 hub
  ├── 加载假借修真路由表（引力盆 + 特征词典）
  ├── 尝试 Level 0/1/2 匹配
  ├── 匹配失败 → Level 3 LLM 兜底
  └── 输出路由决策 → { track: "A|B|C", confidence: 0-1, context_mode: "explore|execute|mixed" }
```

## 五、三技能联动总图

```
用户请求 q
  │
  ▼
master-engineer-hub（路由决策）
  │
  ├── Level 0/1 命中 → 直通执行
  │
  └── Level 3 兜底 → 识别意图
       │
       ├── { "type": "provider_switch" }
       │    └── cc-switch 接管
       │         ├── ccs_provider_list → 列出可用 Provider
       │         ├── token_stats_compare → 对比成本
       │         └── ccs_provider_switch → 切换
       │
       ├── { "type": "token_audit" }
       │    └── token-stats 接管
       │         ├── token_stats_query → 查询用量
       │         ├── token_stats_audit → 审计
       │         └── token_stats_report → 生成报告
       │
       ├── { "type": "development" }
       │    └── omnipotent-engineer 接管
       │         └── lso-os 基础设施执行
       │
       └── { "type": "unknown" }
            └── 机械降神 → GitHub 检索 → 注入 → 执行
```