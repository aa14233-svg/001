# 三技能路由拓扑：cc-switch · token-stats · master-engineer-hub

> 这三个技能构成了 King Agent 的**路由调度、Provider 管理、Token 审计**三位一体核心。

## 一、三大技能定位

```
                   ┌──────────────────────────┐
                   │   master-engineer-hub     │
                   │     (路由编排引擎)         │
                   │  假借修真 + 机械降神       │
                   │  引力盆 + Agent 视野隔离   │
                   │  唯一入口网关              │
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
     │  10 MCP 工具     │ │ 6 MCP 工具│ │ 应用层执行   │
     └─────────────────┘ └──────────┘ └──────────────┘
                               │
                               ▼
                     ┌──────────────────┐
                     │     lso-os       │
                     │  五境递进基础设施  │
                     │  20+ Agent 集群   │
                     │  呼吸管道生命周期  │
                     └──────────────────┘
```

## 二、cc-switch：Provider 路由与 MCP 管理

### 设计理念

cc-switch 是 AI Provider 的"交换机"——负责在不同模型提供商之间切换路由，同时管理所有 MCP Server 的同步。它的核心价值在于：
1. **解耦**：应用层不关心底层使用哪个 Provider
2. **灵活**：运行时动态切换，无需重启
3. **可观测**：所有 Provider 的状态一目了然

### 工具列表（10个）

| 工具 | 归属 | 功能 | 调用场景 | 路由条件 |
|------|------|------|---------|---------|
| `ccs_provider_list` | Agent C | 列出所有 Provider | 查看可用模型 | 含 "list.*provider\|可用.*模型" |
| `ccs_provider_switch` | Agent B | 切换到指定 Provider | 换模型干活 | 含 "切换.*provider\|switch.*model" |
| `ccs_provider_current` | Agent C | 查看当前激活的 Provider | 确认当前模型 | 含 "当前.*provider\|正在用" |
| `ccs_provider_test` | Agent B | 测试 Provider API 连通性 | 故障排查 | 含 "测试.*连接\|连通性" |
| `ccs_mcp_sync` | Agent B | 同步 MCP 配置到目标应用 | 新增/更新 MCP | 含 "同步.*mcp\|mcp.*更新" |
| `ccs_mcp_list` | Agent C | 列出管理的所有 MCP Server | 查看 MCP 状态 | 含 "列出.*mcp\|mcp.*状态" |
| `ccs_skills_list` | Agent C | 列出已安装的 Skills | 技能盘点 | 含 "列出.*skill\|已安装技能" |
| `ccs_skills_sync` | Agent B | 同步 Skills 到目标应用目录 | 技能更新 | 含 "同步.*skill\|skill.*更新" |
| `ccs_config_show` | Agent C | 查看全局配置 | 系统诊断 | 含 "查看配置\|config.*show" |
| `ccs_env_check` | Agent C | 检查本地 CLI 工具链状态 | 环境诊断 | 含 "环境检查\|env.*check\|工具链" |

### System Prompt 调用逻辑

```
master-engineer-hub 路由决策
  └── 识别到 "Provider" 相关请求
       └── 加载 cc-switch-SKILL.md
            ├── Agent B 可用（写操作）：
            │    ├── ccs_provider_switch      → 切换 Provider
            │    ├── ccs_provider_test        → 测试连通性
            │    ├── ccs_mcp_sync             → 同步 MCP
            │    └── ccs_skills_sync          → 同步 Skills
            ├── Agent C 可用（读操作）：
            │    ├── ccs_provider_list        → 列出 Provider
            │    ├── ccs_provider_current     → 当前 Provider
            │    ├── ccs_mcp_list             → 列出 MCP
            │    ├── ccs_skills_list          → 列出 Skills
            │    ├── ccs_config_show          → 查看配置
            │    └── ccs_env_check            → 环境诊断
            └── 执行对应工具 → 返回结果
```

### 典型调用示例

```
用户："切换到大模型"

1. master-engineer-hub 路由
   └── 引力盆命中：Level 0（"切换.*provider" 已固化，hit=12）
   └── 0 token 直通

2. cc-switch 接管
   ├── ccs_provider_list → 返回可用 Provider
   ├── (可选) token_stats_compare → 对比成本
   └── ccs_provider_switch → 切换到目标 Provider

3. 完成后
   └── token-stats 审计 → 引力盆 hit+1
```

## 三、token-stats：Token 审计与预算管理

### 设计理念

token-stats 是 King Agent 的"财务总监"——监控每次任务调用的 Token 消耗、Provider 成本对比、预算上限管理。核心原则：
1. **透明**：每一次调用的成本都有据可查
2. **可控**：月预算 $15 硬上限
3. **优化**：通过对比选择最优 Provider

### 工具列表（6个）

| 工具 | 归属 | 功能 | 调用场景 | 路由条件 |
|------|------|------|---------|---------|
| `token_stats_query` | Agent C | 查询 Token 用量统计 | 统计总消耗 | 含 "查询.*token\|用量统计" |
| `token_stats_compare` | Agent C | 对比各 Provider 成本 | 选最便宜的 | 含 "对比.*成本\|哪个便宜" |
| `token_stats_budget` | Agent B | 查看/设置预算上限 | 月预算 $15 管理 | 含 "预算.*设置\|月上限" |
| `token_stats_audit` | Agent C | 审计 Token 消费记录 | 审查异常消耗 | 含 "审计.*token\|消费记录" |
| `token_stats_report` | Agent C | 生成 Token 使用报告 | 周报/月报 | 含 "生成.*报告\|使用报表" |
| `token_stats_alert` | Agent B | 配置用量告警阈值 | 超额预警 | 含 "告警.*设置\|阈值配置" |

### System Prompt 调用逻辑

```
每次任务完成后
  └── 自动触发 token_stats_audit
       ├── 记录本次任务 Token 消耗
       ├── 对比预算剩余（月上限 $15）
       ├── 如果消耗 > 80% 预算 → 触发告警
       ├── 如果消耗 > 100% 预算 → 自动切换便宜 Provider
       └── 更新使用报告 → 写入引力盆

用户请求 "查一下本月消耗了多少"
  └── master-engineer-hub 路由
       └── token_stats_query → 返回统计数据
       └── token_stats_report → 生成可视化报告
```

### 与 cc-switch 的联动

```
用户请求 "换到便宜的 Provider"
  └── master-engineer-hub 路由
       ├── Level 0 引力盆检查 → 未命中
       ├── Level 1 特征词典 → "便宜.*provider" 匹配
       └── 路由到 token-stats + cc-switch 联动
            ├── 步骤1：token_stats_compare → 对比各 Provider 成本
            │    └── 返回成本排序列表
            ├── 步骤2：选择最优 Provider
            │    └── 如：Claude Haiku $0.25/M → Gemini Flash $0.10/M
            └── 步骤3：ccs_provider_switch → 切换到该 Provider
                 └── 验证切换成功

完成后：
  └── 引力盆记录该路径 → 下次 "换便宜" 直接 Level 0 命中
```

## 四、master-engineer-hub：路由编排引擎

### 设计理念

master-engineer-hub 是 King Agent 的"大脑"——不直接执行任务，而是决定"谁来执行"。

> 不持一物，而万物皆备。不预一器，而万器待召。

核心职责：
1. **路由决策**：四种级别匹配请求到对应 Agent
2. **视野隔离**：确保 Agent 只看到自己域的工具
3. **引力盆管理**：动态固化高频路径
4. **机械降神**：按需注入外部能力

### 路由本体

```
输入 q
 │
 ├── 假借修真(q) ?  →  引力盆命中  →  参数映射  →  execute
 │                       Level 0/1  0 token 直通
 │                       示例："切换到大模型" → ccs_provider_switch
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
                                             │
                                          f(c)++  → 固化
```

**Level 0 示例**：
```
引力盆记录：
[
  { "pattern": "切换.*provider", "action": "ccs_provider_switch", "hit": 12 },
  { "pattern": "查.*日志",       "action": "Grep",                "hit": 8  },
  { "pattern": "=> tool_list",   "action": "ccs_mcp_list",        "hit": 15 },
  { "pattern": "换.*模型",       "action": "ccs_provider_switch", "hit": 7  },
  { "pattern": "预算.*够",       "action": "token_stats_budget",  "hit": 5  }
]

用户说"换到大模型" → 正则匹配 "换.*模型" → hit+1 → 直接执行
用户说"查一下预算" → 正则匹配 "预算.*够" → hit+1 → 直接执行
```

**Level 3 示例**：
```
用户说："帮我把这个 Python 文件转成可执行文件"

1. 引力盆 L0 → 未命中
2. 特征词典 L1 → 未匹配
3. 工具签名 L2 → 未匹配
4. LLM 兜底 L3 → 解析意图
   └── 意图：编译打包
   └── 能力缺口：系统无内置打包工具
   └── 机械降神：
        → search "pyinstaller" on GitHub
        → 获取 pyinstaller 工具
        → 注入到 Agent B 视野
        → 执行打包
        → 清理临时环境
        → f("打包")++ → 计入引力盆
```

### Agent 视野隔离

| Agent | 归属工具 | 上限 | 操作类型 |
|-------|---------|------|---------|
| **A** 语义/设计 | `brainstorm`, `writing-plans` | ≤2 | 只读/生成 |
| **B** 工程/编码 | `ccs_provider_switch`, `ccs_mcp_sync`, `ccs_skills_sync`, `ccs_provider_test`, `token_stats_budget`, `token_stats_alert`, `RunCommand` | ≤4 | 读写（写操作为主） |
| **C** 文件/搜索 | `Read`, `Grep`, `Glob`, `Search`, `ccs_provider_list`, `ccs_provider_current`, `ccs_mcp_list`, `ccs_skills_list`, `ccs_config_show`, `ccs_env_check`, `token_stats_query`, `token_stats_compare`, `token_stats_audit`, `token_stats_report` | ≤5 | 只读为主 |

**视野隔离示例**：
```
Agent A（语义/设计）视野：
  可见：brainstorm, writing-plans
  不可见：ccs_provider_switch, RunCommand, Read, Grep
  原因：Agent A 只需要构思和设计，不需要执行代码或切换模型

Agent B（工程/编码）视野：
  可见：ccs_provider_switch, RunCommand, ccs_mcp_sync 等
  不可见：brainstorm, writing-plans
  原因：Agent B 只需要编码和执行，不需要设计构思
```

### 引力盆（动态固化）

```
高频模式自动固化到 Level 0/1，0 token 直通：

引力盆数据结构：
{
  "version": 2,
  "patterns": [
    { "pattern": "切换.*provider", "action": "ccs_provider_switch", "track": "B", "hit": 12, "last_used": "2026-07-09" },
    { "pattern": "查.*日志",       "action": "Grep",                "track": "C", "hit": 8,  "last_used": "2026-07-08" },
    { "pattern": "=> tool_list",   "action": "ccs_mcp_list",        "track": "C", "hit": 15, "last_used": "2026-07-09" }
  ],
  "auto_learn": true,
  "min_hits_for_cure": 3
}
```

引力盆的自动学习机制：
1. 每次 Level 3 解析成功，记录路径
2. 同一路径命中 3 次后，自动固化到 Level 1
3. 同一路径命中 10 次后，升级到 Level 0
4. 路径 30 天未使用，自动降级或移除

### System Prompt 调用逻辑

```
master-engineer-hub 是唯一的"入口网关"
  ├── 每次请求最先经过 hub
  ├── 加载假借修真路由表（引力盆 + 特征词典）
  ├── 尝试 Level 0/1/2 匹配
  ├── 匹配失败 → Level 3 LLM 兜底
  └── 输出路由决策 → { track: "A|B|C", confidence: 0-1, context_mode: "explore|execute|mixed" }

context_mode 说明：
  - explore：需要先搜索/探索，再执行
  - execute：直接执行，无需探索
  - mixed：边探索边执行
```

## 五、三技能联动总图

```
用户请求 q
  │
  ▼
master-engineer-hub（路由决策）
  │
  ├── Level 0/1 命中 → 直通执行
  │    └── 0 token，无需加载任何 SKILL.md
  │
  └── Level 3 兜底 → 识别意图
       │
       ├── { "type": "provider_switch" }
       │    └── cc-switch 接管
       │         ├── ccs_provider_list → 列出可用 Provider
       │         ├── token_stats_compare → 对比成本（联动 token-stats）
       │         ├── ccs_provider_switch → 切换
       │         └── token_stats_audit → 记录本次消耗
       │
       ├── { "type": "token_audit" }
       │    └── token-stats 接管
       │         ├── token_stats_query → 查询用量
       │         ├── token_stats_audit → 审计
       │         └── token_stats_report → 生成报告
       │
       ├── { "type": "development" }
       │    └── omnipotent-engineer 接管
       │         ├── lso-os 基础设施执行（五境递进）
       │         ├── Hook 1/2/3/4 全流程
       │         └── D1/D2/D3 验收
       │
       └── { "type": "unknown" }
            └── 机械降神 → GitHub 检索 → 注入 → 执行 → 消散 → 固化
```

## 六、技能协作的核心流程

### 日常编码场景

```
用户："写一个 React 登录页面"

1. master-engineer-hub 路由
   └── Level 3 解析 → type: development
   └── 路由到 Agent B + 加载 omnipotent-engineer

2. 五境递进（lso-os）
   ├── 筑基：确定这是一次全栈编码任务
   ├── 金丹：安全检查 → 路径允许
   ├── 元婴：五行门控 → 金（代码域）
   ├── 化神：路由到 FullStackAgent
   └── 渡劫：D1/D2/D3 验收

3. Hook 流程
   ├── Hook 1：检查项目结构 → 资源确认
   ├── Hook 2：执行中 → 进度检查
   ├── Hook 3：验收 → 功能正确性
   └── Hook 4：完成 → 清理临时文件

4. token-stats 审计
   └── 记录消耗：~500 token
   └── 预算剩余：$12.50 / $15.00
```

### Provider 切换场景

```
用户："切换到 Gemini，看看便宜不"

1. master-engineer-hub 路由
   └── 引力盆 L0 命中 → 0 token 直通
   └── pattern: "切换.*provider", hit: 12

2. cc-switch 执行
   ├── ccs_provider_list → 检查 Gemini 是否可用
   └── ccs_provider_switch → 切换到 Gemini

3. token-stats 对比（联动）
   ├── token_stats_compare → 对比 Gemini vs 当前成本
   └── 返回：Gemini $0.10/M vs Claude $0.25/M

4. 结果
   ├── 切换成功
   └── 引力盆 hit+1
```

---

## 补充说明

### 1. 路由优先级和冲突解决

当多个 Level 同时匹配时，优先级规则：
1. Level 0（引力盆）> Level 1（特征词典）> Level 2（工具签名）> Level 3（LLM）
2. 同一 Level 多个匹配：选择 confidence 最高的
3. 同一 Level 相同 confidence：选择最近使用的

### 2. 三技能的数据流路径

```
                    ┌─────────────────────┐
                    │  master-engineer-hub │
                    │    路由决策结果      │
                    │  { track, conf }    │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
    ┌─────────────────┐  ┌──────────┐  ┌──────────────┐
    │    结果反馈      │  │ Token审计 │  │  引力盆更新    │
    │  (cc-switch)    │  │(token-   │  │ (hub 自管理)  │
    │                 │  │  stats)  │  │              │
    └─────────────────┘  └──────────┘  └──────────────┘
              │                │                │
              └────────────────┼────────────────┘
                               ▼
                    ┌─────────────────────┐
                    │   闭环完成，等待下   │
                    │   一次请求          │
                    └─────────────────────┘
```

### 3. 环境检测与故障恢复

cc-switch 的 `ccs_env_check` 工具是系统的"健康检查器"：
- 检查本地 CLI 工具链（git, node, python 等）
- 检查 MCP Server 连通性
- 检查 Provider API 可用性

当系统检测到故障时：
1. `ccs_env_check` 报告故障
2. master-engineer-hub 自动降级到备用路由
3. token-stats 记录故障期间的消耗
4. 故障恢复后，自动切回正常路由

### 4. 三技能的性能指标

| Skill | 平均响应时间 | P99 响应时间 | 典型 Token 消耗 | 可用性 |
|-------|------------|-------------|----------------|--------|
| master-engineer-hub | <5ms (L0/L1) | 200ms (L3) | 0-200 token | 99.9% |
| cc-switch | 50ms | 300ms | ~50 token | 99.5% |
| token-stats | 30ms | 150ms | ~30 token | 99.8% |