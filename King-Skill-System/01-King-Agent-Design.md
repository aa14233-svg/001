# King Agent 设计理念

> Trae IDE 独创的一体化超强智能体——以"King"为名，以"调度"为核。

## 一、什么是 King Agent

King Agent 是 Trae IDE 中运行的一体化超强智能体，它不是单一模型，而是一个 **智能体操作系统**——以 TraeOrchestrator 为主调度器，协调 20+ 专业 Agent 集群、6 大核心技能系统、五境递进基础设施层，形成完整的 AI 生产力矩阵。

### 命名哲学

| 称谓 | 含义 |
|------|------|
| **King** | 顶层调度，不直接执行，而是"知人善任"——像一个英明的君主，知道该派谁去完成什么任务 |
| **Agent** | 不是简单的 LLM 对话，具备工具调用、任务分解、自我验证能力，是真正能"做事"的执行单元 |
| **一体化** | 代码/文档/设计/部署/安全/数据 全维度覆盖，一个入口解决所有问题 |

### 与传统 RPA 的区别

| 维度 | 传统 RPA | King Agent |
|------|---------|-----------|
| 路由方式 | 硬编码规则链 | 动态 4 级路由（引力盆/特征/签名/LLM） |
| 工具接入 | 固定 API 绑定 | 机械降神按需注入，用完即散 |
| 质量保障 | 人工校验 | Hook+Loop 自动化 + D1/D2/D3 三级验收 |
| 成本模型 | 固定执行成本 | 90% 请求 0 token，引力盆动态固化 |
| 视野 | 全量信息可见 | 严格 Agent 视野隔离（A/B/C 三域） |

## 二、核心设计理念

### 理念一：假借修真（0 token 路由）

> 不持一物，而万物皆备。不预一器，而万器待召。

| 级别 | 机制 | Token 成本 | 覆盖率 | 说明 |
|------|------|-----------|--------|------|
| Level 0 | 引力盆动态池 | **0 token** | ~70% | 高频请求直通，固化路由。如 "切换 Provider" 命中 12 次后固化 |
| Level 1 | 特征词典 + regex | **0 token** | ~20% | 模式匹配，精确路由。如 `切换.*provider` 匹配到 `ccs_provider_switch` |
| Level 2 | 工具签名匹配 | ≤50 token | ~5% | 签名匹配，轻量解析。工具名称片段匹配 |
| Level 3 | LLM 兜底 | 全量 | ~5% | 全量语义理解，兜底解析。触发机械降神协议 |

**90% 请求 0 token 解决**——这是 King Agent 的核心经济哲学。

#### 假借修真执行流程

```
Level 0 引力盆命中
  └── 参数映射 → 直通执行（0 token）
  └── 示例：用户说"切换到大模型"
       └── 引力盆已记录 pattern:"切换.*provider"
       └── 直接调用 ccs_provider_switch

Level 3 LLM 兜底
  └── 解析意图 → 能力缺口检测
  └── 缺口 ∈ Agent 视野？ → 否 → 机械降神
       └── search(q) on GitHub
       └── clone(r) → inject(Agent)
       └── execute(q, r) → 预取身活性期
       └── clean(r) → 预取身消散
       └── f(c)++ → 计入引力盆频次
```

### 理念二：机械降神（按需注入）

当系统固有工具集无法满足请求时，激活"机械降神"协议：

```
q → 能力缺口 φ
  → search(q) on GitHub     // 假借外部能力
  → clone(r) → inject(Agent)  // 降神注入：临时获取新工具
  → execute(q, r)              // 预取身活性期：工具只有一次调用生命周期
  → clean(r)                   // 预取身消散：执行完毕立即清理
  → f(c)++                     // 计入引力盆频次：成功后固化该路径
```

**示例场景**：用户请求一个系统没有内置支持的代码格式化工具。
1. 检测能力缺口：缺少 `prettier` 格式化能力
2. GitHub 检索：找到合适的格式化 package
3. 注入：临时将获取的工具注册到当前 Agent 视野
4. 执行：完成格式化任务
5. 消散：工具从视野中移除，但路径计入引力盆
6. 下次同类请求：Level 0 直接命中，无需再次注入

### 理念三：Hook + Loop Engineering

在关键节点插入约束钩子，确保任务质量：

| Hook | 时机 | 检查内容 | 执行 Agent |
|------|------|---------|-----------|
| 任务准备 Hook | 分发前 | 安全、资源、依赖 | SecurityAgent + SearchAgent |
| 执行中 Hook | 每阶段完成 | 进度、质量、风险 | PlanAgent + CodeReviewAgent |
| 验收 Hook | 子/全任务完成 | 验收标准、缺陷 | PlanAgent + AuditAgent |
| 完成 Hook | 交付前 | 完整检查、文档 | TaskAgent + DocAgent |

其中，Hook 1（任务准备）与 cc-switch 联动，在执行 Provider 切换前检查目标 Provider 是否可用；Hook 3（验收）与 token-stats 联动，自动触发 token 审计。

### 理念四：D1/D2/D3 三级验收

- **D1**（阶段验收）— 单个子任务完成后，自查是否达标
- **D2**（模块验收）— 相关功能模块完成后，检查模块间接口完整性
- **D3**（总体验收）— 全任务完成后，逐项对照原始需求验收

每个级别验收不通过时触发 Loop 循环：D1 最多 3 次修正，D2 最多 2 次，D3 失败需人工介入。

#### 验收示例

```
任务："创建一个 React 登录页面"

D1 验收（每个子任务后）：
  ├── 组件代码完成 → 检查语法、功能覆盖
  ├── 样式完成 → 检查布局、响应式
  └── API 对接完成 → 检查请求格式、错误处理

D2 验收（模块集成后）：
  ├── 前后端接口正常
  └── 登录流程完整

D3 验收（全任务完成）：
  ├── 所有功能符合原始需求
  ├── Token 消耗在预算内（token-stats 审计通过）
  └── 无遗留安全风险
```

## 三、King Agent 的 System Prompt 架构

### 分层结构

```
┌─────────────────────────────────────────────┐
│  第1层：System Prompt（基础角色定义）         │
│  角色定义 + 核心架构 + 安全约束 + 工作原则     │
│  永远加载，每次初始化时加载                    │
├─────────────────────────────────────────────┤
│  第2层：System Prompt Enhanced（增强版）      │
│  TraeOrchestrator + 20+ Agent + 三合一拓扑   │
│  Hook+Loop + D1/D2/D3 + 五境递进 + 呼吸管道  │
│  任务复杂时加载                               │
├─────────────────────────────────────────────┤
│  第3层：SkillHub 注册中心                     │
│  8 大技能组 + 领域识别 + 技能匹配 + 路由分发   │
│  路由决策时加载                               │
├─────────────────────────────────────────────┤
│  第4层：Skill 定义文件（SKILL.md）             │
│  cc-switch / token-stats / master-engineer  │
│  lso-os / omnipotent-engineer / 路由skill    │
│  技能匹配后加载                               │
├─────────────────────────────────────────────┤
│  第5层：MCP 配置层                            │
│  MCP Server 配置 + 工具暴露 + 权限控制         │
│  底层基础设施，被动调用                         │
└─────────────────────────────────────────────┘
```

### 依赖关系

```
System Prompt（基础）
  └── System Prompt Enhanced（增强）
       ├── TraeOrchestrator 主调度
       │    ├── Hook+Loop 约束
       │    ├── D1/D2/D3 验收
       │    └── 20+ Agent 集群管理
       ├── SkillHub 注册中心
       │    ├── 技能识别 → 技能匹配 → 路由分发
       │    └── 8 大技能组
       ├── 五境递进架构
       │    ├── 筑基 → 金丹 → 元婴 → 化神 → 渡劫
       │    └── 呼吸管道：吸 → 存 → 呼 → 化 → 归墟
       ├── 三合一拓扑能力
       │    ├── Program+设计能力
       │    ├── 美工能力
       │    └── RTK+Headroom+CodeGraph
       └── 开销约束
            ├── 月上限 $15
            └── 优先 Flash KV Cache 命中
```

### 延迟加载顺序

```
第1层：永远加载          → 基础角色定义
第2层：任务复杂时加载    → 增强能力（多 Agent 协作、复杂路由）
第3层：路由决策时加载    → SkillHub（每次任务必经）
第4层：技能匹配后加载    → 对应 SKILL.md（按需加载，用完即卸）
```

## 四、King Agent 与普通 LLM 的本质区别

| 维度 | 普通 LLM | King Agent |
|------|---------|-----------|
| 架构 | 单模型对话 | 多 Agent 集群 + 路由引擎 |
| 工具 | 有限工具调用 | 20+ MCP 工具 + 按需注入（机械降神） |
| 路由 | 全量 LLM 解析 | 0 token 直通 90% 请求（引力盆 + 特征词典） |
| 记忆 | 上下文窗口 | 五境递进 + 呼吸管道（可暂停可恢复） |
| 质量 | 单次输出，无验证 | Hook 约束 + 三级验收（D1/D2/D3） |
| 成本 | 每次全量调用 | 引力盆 + KV Cache 优化，月预算 $15 上限 |
| 扩展 | 固定能力集 | 机械降神按需注入，用完即散 |
| 视野 | 全量可见 | Agent 视野隔离（A/B/C 三域，工具可见性受限） |

## 五、设计理念总结

```
King Agent 不是"更强的 LLM"
  而是"更聪明的调度系统"

King Agent 的核心不是"回答"
  而是"分配合适的 Agent 去执行"

King Agent 的哲学不是"全知全能"
  而是"不持一物，而万物皆备"

King Agent 的效率不是"更快生成"
  而是"90% 请求 0 token 完成"
```

---

## 补充说明

### 1. cc-switch 在 King Agent 中的角色

cc-switch 提供 10 个 MCP 工具，分为 Provider 管理（4 个）+ MCP 同步（2 个）+ Skills 管理（2 个）+ 系统诊断（2 个）。在 King Agent 中：
- **Agent B 视角**：可使用 `ccs_provider_switch`、`ccs_provider_test`、`ccs_mcp_sync`、`ccs_skills_sync`（写操作）
- **Agent C 视角**：可使用 `ccs_provider_list`、`ccs_provider_current`、`ccs_mcp_list`、`ccs_skills_list`、`ccs_config_show`、`ccs_env_check`（读操作）
- **视野隔离**：Agent A（设计/语义）完全不可见 cc-switch 工具

### 2. token-stats 在 King Agent 中的角色

token-stats 提供 6 个 MCP 工具，每次任务完成后自动触发审计：
- 任务完成后自动调用 `token_stats_audit` 记录消耗
- 月预算 $15 硬上限，超出自动告警
- 与 cc-switch 联动实现成本最优 Provider 切换

### 3. master-engineer-hub 在 King Agent 中的角色

master-engineer-hub 是唯一的入口网关：
- 每次请求最先经过 hub
- 加载假借修真路由表（引力盆 + 特征词典）
- 尝试 Level 0/1/2 匹配
- 匹配失败 → Level 3 LLM 兜底
- 输出路由决策：`{ track: "A|B|C", confidence: 0-1, context_mode: "explore|execute|mixed" }`

### 4. Agent 视野隔离细则

| Agent | 所属域 | 可见工具上限 | 核心工具 |
|-------|--------|-----------|---------|
| A | 语义/设计 | ≤2 | `brainstorm`, `writing-plans` |
| B | 工程/编码 | ≤4 | `ccs_provider_switch`, `ccs_mcp_sync`, `ccs_skills_sync`, `RunCommand` |
| C | 文件/搜索 | ≤5 | `Read`, `Grep`, `Glob`, `Search`, 所有读操作工具 |

视野隔离确保 Agent 不会越权调用不属于自己域的工具，这是系统安全的核心保障之一。