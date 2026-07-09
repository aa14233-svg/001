# King-Skill 技能群系系统 + AI 教材体系

> 从零基础入门到深度架构研究的完整 AI 知识库
>
> **一体超强智能体（King Agent）** 的设计、实现与技术哲学

---

## 一、仓库概览

本仓库包含两大部分内容，分别面向不同的学习目标：

### 第一部分：AI 教材体系

系统化的 AI 学习教材，从基础概念到高级应用，分为三个阶段：

- **Phase 1：基础入门** -- AI 基本概念、Prompt 工程入门、工具链安装配置
- **Phase 2：进阶应用** -- Agent 设计模式、RAG 实现、工作流编排
- **Phase 3：高级架构** -- 多 Agent 协作、系统集成、性能优化

教材体系适合从零开始的 AI 学习者，以及希望系统化 AI 知识的开发者。

### 第二部分：King-Skill 技能群系系统

以 **King Agent** 为中心拓扑展开的大智能体技能路由群系系统，包含：

- **三大核心技能**：cc-switch（Provider 管理）、token-stats（Token 审计）、master-engineer-hub（路由编排）
- **8 大技能组**：覆盖文档、表格、演示、研究、代码等全领域
- **20+ Agent 集群**：A/B/C 三类 Agent 分工协作，视野隔离
- **LSO-DAO 五境递进**：筑基→金丹→元婴→化神→渡劫的修炼式质量保障
- **Hook+Loop 工程体系**：4 Hook 节点 + D1/D2/D3 三级验收 + 3 次循环重试

---

## 二、教材介绍

### 教材体系结构

```
📚 AI 教材体系
├── Phase 1：基础入门
│   ├── 什么是 AI 与 LLM
│   ├── Prompt 工程基础
│   ├── 工具链安装与配置
│   └── 第一个 AI 应用
├── Phase 2：进阶应用
│   ├── Agent 设计模式
│   ├── RAG 检索增强生成
│   ├── 工作流编排
│   └── API 集成与部署
└── Phase 3：高级架构
    ├── 多 Agent 协作系统
    ├── 企业级 AI 架构
    ├── 性能优化与监控
    └── 安全与合规
```

### 教材特点

- **循序渐进**：从零开始，每阶段建立在前一阶段基础上
- **理论与实践并重**：每个概念都有对应的动手练习
- **案例驱动**：通过真实项目案例讲解技术要点
- **持续更新**：紧跟 AI 技术发展，定期更新内容

### 学习目标

完成本教材后，你将能够：

1. 理解 AI/LLM 的核心概念和工作原理
2. 独立设计和实现 AI Agent 应用
3. 掌握多 Agent 协作系统的架构设计
4. 具备 AI 系统性能优化和问题排查能力

---

## 三、King-Skill 系统介绍

### 系统概述

King-Skill 是一套以 **King Agent（一体超强智能体）** 为中心拓扑，整合 **cc-switch、token-stats、master-engineer-hub** 三大核心技能，辅以 8 大技能组、20+ Agent 集群、LSO-DAO 五境递进基础设施的完整 AI 智能体技能路由生态。

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

### 核心哲学

> **不持一物，而万物皆备。不预一器，而万器待召。**

King-Skill 的核心理念是"假借修真"--借用 LLM 的能力来实现智能体路由，而不是自己开发 AI 模型。通过精心设计的四层路由体系，90% 的请求可以在 0 token 消耗下完成，只有 5% 的复杂请求需要调用 LLM。

### 三大核心技能

| 技能 | 角色 | MCP 工具数 | 核心功能 |
|------|------|-----------|---------|
| **cc-switch** | Provider 路由 + MCP 管理 | 10 | ccs_provider_switch、ccs_mcp_list、ccs_env_check 等 |
| **token-stats** | Token 审计 + 预算管理 | 6 | token_stats_query、token_stats_budget、token_stats_compare 等 |
| **master-engineer-hub** | 路由编排引擎 | 1（编排入口） | 四层路由 L0/L1/L2/L3、引力盆路径固化 |

---

## 四、三档用户指南

### 第一档：AI 入门者指南

> 适合对象：零基础、刚接触 AI、没有编程经验

#### 什么是 AI 智能体？

想象你有一个非常聪明的秘书，他的特点如下：

| 秘书的特点 | AI 智能体的对应 |
|-----------|----------------|
| 能听懂你的指令 | 理解自然语言（你说中文、英文都可以） |
| 知道什么时候该用什么工具 | 自动路由到最合适的 "技能" |
| 做事有章法，不会乱来 | 遵循 LSO-DAO 五境递进流程 |
| 做完了会给你检查 | 通过 Hook+Loop 进行质量验收 |
| 做完一次后，下次类似的活更快 | 引力盆机制自动学习路径 |

简单来说，**AI 智能体就是一个能理解你的需求、自动调用各种工具、并且保证质量的数字助手**。King Agent 就是这个智能体的"大脑"，它知道什么情况下该用什么技能。

#### 用比喻帮助理解

把 King-Skill 系统想象成一个**高级餐厅的后厨**：

- **你（用户）**：来吃饭的客人，你说"我想吃辣的"
- **King Agent（主厨）**：理解你的需求，决定做什么菜
- **master-engineer-hub（排菜员）**：判断这个菜该由哪个师傅做
- **cc-switch（食材供应商）**：提供不同的食材（不同的 AI 模型）
- **token-stats（记账员）**：记录用了多少食材（花了多少 Token）
- **20+ Agent（各位师傅）**：炒菜的、切菜的、摆盘的，各有专长
- **LSO-DAO（质量检查）**：出锅前检查味道、摆盘、温度
- **引力盆（经验积累）**：做过的菜下次做更快

#### 推荐阅读路径

如果你是零基础，建议按以下顺序阅读：

```
第一步：理解什么是 King Agent
  → King-Skill-System/01-King-Agent-Design.md
  （重点看"什么是 King Agent"和"假借修真"部分，技术细节可以先跳过）

第二步：看系统全景图
  → King-Skill-System/08-Complete-Topology-Map.md
  （先看拓扑图的全貌，理解各个部分的关系）

第三步：了解三大技能
  → King-Skill-System/03-Routing-Topology.md
  （理解 cc-switch、token-stats、master-engineer-hub 各自做什么）
```

#### 不需要理解的技术细节（可以跳过）

- Agent 视野隔离的具体实现（A/B/C 类的工具数量限制）
- Hook+Loop 的 Token 成本计算公式
- LSO-DAO 五行门控的具体 SQL 或代码实现
- 引力盆的数据结构和路径固化算法
- MCP 协议的底层实现细节

#### 入门者常见问题 FAQ

**Q1：我需要会编程才能用这个系统吗？**
A：不需要。King-Skill 系统的设计目标就是让非技术人员也能使用 AI 能力。你只需要用自然语言描述需求，系统会自动处理技术细节。

**Q2：Token 是什么？为什么要关心它？**
A：Token 是 AI 模型处理信息的最小单位，可以理解为"字数"。每次请求都会消耗 Token，而 Token 需要花钱买。King-Skill 系统的特殊之处在于，90% 的请求可以在不消耗 Token（不花钱）的情况下完成。

**Q3：King Agent 和 ChatGPT 有什么区别？**
A：ChatGPT 是一个单一的对话 AI。King Agent 是一个"智能体操作系统"——它能调用 20 多个不同的专业 AI "员工"（Agent），每个员工有自己擅长的领域，还会自动进行质量检查。

**Q4：什么是"假借修真"？听起来像是修仙小说。**
A：哈哈，确实用了修仙的比喻。"假借"意思是借用外部力量，"修真"意思是实现真正的智能。就是说 King Agent 不自己开发 AI 能力，而是巧妙地借用各种现有的 AI 模型和工具，组合出更强大的能力。

**Q5：我的数据安全吗？**
A：cc-switch 技能提供了 Provider 管理功能，你可以选择使用本地模型或私有部署的模型。此外，Agent 视野隔离机制确保每个 Agent 只能看到它需要的信息，不会泄露不相关的数据。

**Q6：如果我遇到问题怎么办？**
A：系统内置了 Hook+Loop 质量保障机制，会自动检测和重试失败的任务。如果问题持续存在，系统会生成详细的错误报告。你也可以查阅本仓库的教材部分获取更多帮助。

**Q7：这个系统能做什么实际的事情？**
A：可以写代码、写文档、做表格、生成 PPT、搜索信息、分析数据、管理项目等等。它就像一个全能助手，你说"帮我写一个 Python 爬虫"或"分析这份 Excel 数据"，它就能自动完成。

**Q8：我该怎么开始？**
A：最简单的方式就是按照上面的"推荐阅读路径"开始阅读，从 01 文件看起。不需要安装任何软件，不需要配置任何环境，先理解概念再动手。

---

### 第二档：有一定计算机能力基础者指南

> 适合对象：有编程/开发经验、了解 API 调用、熟悉传统软件开发流程

#### King Agent 与传统编程的区别

| 维度 | 传统编程 | King Agent 系统 |
|------|---------|----------------|
| **问题解决方式** | 写代码实现固定逻辑 | 用自然语言描述目标，系统自动路由 |
| **逻辑控制** | if-else / switch-case 硬编码 | 四层路由（引力盆→特征→签名→LLM） |
| **能力扩展** | 安装库、引入 SDK | 机械降神动态注入 Skill |
| **调试方式** | 断点、日志、单元测试 | Hook 节点检查 + D1/D2/D3 验收 |
| **状态管理** | 数据库、缓存、全局变量 | 呼吸管道（吸入→储存→呼出→化合→归墟） |
| **部署方式** | 编译打包、容器化 | 一次性生命周期，用完即回收 |
| **质量保障** | CI/CD、代码审查、测试覆盖率 | LSO-DAO 五境递进 + 自动重试 |

#### 核心概念速览

**1. 路由（Routing）**
```
传统：if (request.contains("python")) { callPythonAgent(); }
King：四层递进匹配 →
  L0: 查引力盆缓存（命中率 70%，0 token）
  L1: 特征词典匹配（命中率 20%，0 token）
  L2: 工具签名匹配（命中率 5%，≤50 token）
  L3: LLM 裁决（命中率 5%，全量 token）
```

**2. Agent**
- 不是单个模型，而是一个"角色 + 工具集 + 行为约束"的封装
- A 类（分析/设计）：最多看 2 个工具，侧重理解需求
- B 类（架构/工程）：最多看 4 个工具，侧重方案设计
- C 类（编码/文件）：最多看 5 个工具，侧重代码实现
- 这就是 **Agent 视野隔离** -- 每个 Agent 只看到它需要的信息

**3. Skill**
- Skill = MCP 工具集 + System Prompt 定义 + 路由规则
- 8 大技能组：docx、pptx、xlsx、pdf、html-report、html-deck、research-guide、doc-writing-guide
- 每个 Skill 有自己的域识别关键词和匹配优先级

**4. Hook**
- 在请求处理的 4 个关键节点插入检查逻辑
- Hook 1（准备）→ Hook 2（执行）→ Hook 3（验收）→ Hook 4（完成）
- D1（格式校验）/ D2（功能验证）/ D3（性能评估）
- 任一环节失败触发 Loop 重试（最多 3 次）

#### 推荐阅读路径

```
第一步：先看拓扑图，建立整体认知
  → King-Skill-System/08-Complete-Topology-Map.md
  （理解 7 层架构和两条数据流路径）

第二步：深入路由机制
  → King-Skill-System/03-Routing-Topology.md
  （重点：四层路由的触发条件和 Token 成本）

第三步：理解 Agent 架构
  → King-Skill-System/06-Agent-Cluster-20plus.md
  （重点：A/B/C 三类 Agent 的视野隔离）

第四步：学习质量体系
  → King-Skill-System/07-Hook-Loop-Engineering.md
  （重点：4 Hook 节点和 D1/D2/D3 验收标准）

第五步：补充阅读
  → King-Skill-System/04-SkillHub-Registry.md（技能注册流程）
  → King-Skill-System/05-LSO-DAO-Five-Realms.md（五境修炼体系）
  → King-Skill-System/02-System-Prompt-Architecture.md（System Prompt 架构）
  → King-Skill-System/09-System-Prompt-References.md（依赖关系）
```

#### 与现有开发工作流的对比

| 你的日常工作 | King-Skill 中的对应 |
|------------|-------------------|
| Git 分支管理 | 引力盆路径版本化 |
| CI/CD Pipeline | LSO-DAO 五境递进 |
| 单元测试 | D1 格式校验 |
| 集成测试 | D2 功能验证 |
| 性能测试 | D3 性能评估 |
| 异常处理 | Loop 重试机制 |
| 配置管理 | cc-switch Provider 管理 |
| 日志监控 | token-stats 全链路审计 |
| API 网关 | master-engineer-hub 路由引擎 |
| 微服务治理 | Agent 视野隔离 + 分工协作 |

#### 从传统开发到 AI 智能体开发的思维转变要点

1. **从"写逻辑"到"写约束"**：不再需要编写每一步的执行逻辑，而是定义目标和约束条件（规则、边界、验收标准），让系统自动规划执行路径。

2. **从"确定性"到"概率性"**：传统代码是确定性的（同样的输入永远得到同样的输出），AI 系统是概率性的。需要通过 Hook 节点和验收机制来保证输出质量。

3. **从"长流程"到"短循环"**：传统开发中一个完整流程可能需要编写大量代码（数百行），AI 开发中通过 Hook+Loop 机制实现"短周期执行→快速检查→迭代修正"。

4. **从"资源管理"到"Token 管理"**：传统开发关心 CPU/内存/网络，AI 开发中 Token 成为最核心的资源。需要像管理预算一样管理 Token 消耗。

5. **从"代码复用"到"路径复用"**：传统开发通过函数、类、模块来复用代码；AI 开发通过引力盆路径固化来复用成功的调用路径。

6. **从"异常捕获"到"预期验收"**：不再用 try-catch 捕获异常，而是通过 D1/D2/D3 验收标准来确保每个环节的输出符合预期。

7. **从"垂直扩展"到"水平扩展"**：传统开发通过增加代码量来扩展功能；AI 开发通过增加 Agent 和 Skill（水平扩展）来覆盖更多场景。

---

### 第三档：深度使用老手指南

> 适合对象：系统架构师、AI 研究者、希望深度定制和扩展系统的开发者

#### 系统架构深度分析

King-Skill 本质上是一个 **元路由系统**（Meta-Routing System），其架构设计遵循以下原则：

**1. 分层抽象原则**
```
┌─────────────────────────────────────────────┐
│  交互层：用户请求（自然语言）                  │
├─────────────────────────────────────────────┤
│  路由层：master-engineer-hub（四层匹配机制）    │
├─────────────────────────────────────────────┤
│  调度层：SkillHub（域识别 → 技能匹配 → 分发）  │
├─────────────────────────────────────────────┤
│  执行层：20+ Agent（A/B/C 三类视野隔离）       │
├─────────────────────────────────────────────┤
│  质量层：LSO-DAO（五境递进）                  │
├─────────────────────────────────────────────┤
│  保障层：Hook+Loop（4 Hook + D1/D2/D3）       │
├─────────────────────────────────────────────┤
│  经济层：引力盆 + token-stats（Token 优化）    │
└─────────────────────────────────────────────┘
```

**2. 控制反转（IoC）**
- 不是 Agent 主动获取工具，而是由路由引擎判断后注入
- 四层路由机制本质上是 IoC 容器 + AOP（面向切面编程）
- Hook 节点相当于 AOP 的切点（Pointcut）

**3. 事件驱动架构**
```
用户请求 → 路由事件 → 匹配事件 → 执行事件 → 验收事件 → 完成事件
              │           │          │          │          │
              ▼           ▼          ▼          ▼          ▼
          L0/L1/L2/L3  SkillHub   Agent    Hook+Loop   引力盆学习
```

#### 假借修真的实现原理

"假借修真"的核心是 **动态路径选择与固化**：

```
请求到达 →
  ├─ 查引力盆（Hash 表：域特征 → [AgentID, SkillID, 成功率]）
  │   └─ 命中 → 直接调用（0 token）
  │
  ├─ L1 特征词典（Trie 树：关键词 → 域）
  │   └─ 匹配 → 域识别 → Skill匹配 → Agent调用（0 token）
  │
  ├─ L2 工具签名（参数 Schema 匹配）
  │   └─ 匹配 → 精确调用 cc-switch/token-stats（≤50 token）
  │
  └─ L3 LLM 裁决（上下文注入 + LLM 分析）
      └─ 生成路径 → 执行 → 成功 → 写入引力盆（学习）
```

引力盆的数据结构本质上是一个 **加权有向图**：

```json
{
  "gravity_basin": {
    "domain_feature": "python_web_scraping",
    "entries": [
      {
        "pattern_hash": "a1b2c3d4",
        "agent_id": "agent-python",
        "skill_id": "omnipotent-engineer",
        "success_count": 47,
        "avg_token_cost": 320,
        "last_used": "2026-07-09T10:30:00Z",
        "is_solidified": true
      }
    ]
  }
}
```

当某条路径的 `success_count` 超过阈值（默认 10 次）且 `avg_token_cost` 低于预期时，自动标记为 `is_solidified: true`，从此直接走 L0 直通。

#### 机械降神的实现原理

"机械降神"（Deus ex Machina）是能力动态注入机制：

```
触发条件：路由引擎检测到当前所有 Agent 都无法处理请求
执行流程：
  1. 搜索可用 Skill（本地注册表 + GitHub 远程仓库）
  2. 找到匹配 Skill 后，克隆/下载其定义
  3. 动态注入到 Agent 的 System Prompt 中（追加工具签名）
  4. 执行请求（一次性生命周期）
  5. 执行完毕后回收注入的能力（从 System Prompt 中移除）
  6. 如果执行成功，将路径写入引力盆供后续复用
  7. 更新 skill-list.md 注册新 Skill
```

**关键设计**：一次性生命周期确保每次注入都是独立的，不会污染后续请求。

#### 性能优化要点

**1. Token 优化策略**

| 优化点 | 策略 | 预期效果 |
|-------|------|---------|
| System Prompt 加载 | 五层延迟加载（L0-L4） | 简单请求节省 90% Token |
| 引力盆命中率 | 高频路径自动固化 | ≥70% 请求 0 token |
| 特征词典 | Trie 树优化 + 前缀匹配 | 匹配时间 < 1ms |
| Skill 加载 | 按需加载 + 缓存 | 重复使用免加载 |
| Token 审计 | 异步日志 + 批量上报 | 不阻塞主流程 |

**2. 响应时间优化**

| 优化点 | 当前值 | 目标值 | 方法 |
|-------|-------|-------|------|
| L0 匹配 | ~5ms | < 1ms | 引力盆 Hash 索引 |
| L1 匹配 | ~20ms | < 5ms | Trie 树 + 缓存 |
| L2 匹配 | ~50ms | < 10ms | Schema 预编译 |
| L3 裁决 | ~2000ms | < 1000ms | 流式输出 + 预加载 |
| Agent 切换 | ~100ms | < 50ms | Agent 预热池 |

**3. 并发处理**

- Agent 实例池化：每个 Agent 保持 3-5 个预热实例
- 请求队列：优先级队列（L0 > L1 > L2 > L3）
- 超时控制：每层路由有独立超时时间（L0: 100ms, L1: 500ms, L2: 1s, L3: 30s）
- 熔断机制：当某 Agent 连续失败 5 次时自动熔断 30 秒

#### 扩展与定制指南

**1. 新增一个自定义 Agent**

```python
# 1. 在 agent-list.md 中注册
agent_id: "agent-custom-vision"
type: "B"  # A/B/C 视野等级
tools: ["image_analyze", "ocr_extract"]
max_tools: 4
dependencies: ["agent-analysis.md"]

# 2. 编写 Agent 定义文件
# King-Skill-System/06-Agent-Cluster-20plus.md 中添加

# 3. 在技能路由中注册匹配规则
# King-Skill-System/04-SkillHub-Registry.md 中添加域关键词

# 4. 验证视野隔离
# Agent-B 类应只能看到 4 个工具
```

**2. 新增一个自定义 Skill**

```json
{
  "skill_id": "custom-data-viz",
  "name": "数据可视化技能",
  "domain_keywords": ["图表", "可视化", "chart", "plot"],
  "mcp_tools": [
    {"name": "create_chart", "params": {"type": "string", "data": "object"}},
    {"name": "export_viz", "params": {"format": "string"}}
  ],
  "confidence_threshold": 0.7,
  "fallback_skill": "omnipotent-engineer"
}
```

**3. 自定义 Hook 逻辑**

```javascript
// Hook 2（执行中检查）的自定义逻辑
function customHook2(context) {
  // 检查 Agent 的 Token 消耗是否超过预算
  if (context.token_used > context.budget_limit) {
    return { status: "FAIL", reason: "Token budget exceeded" };
  }
  // 检查执行时间是否超过限制
  if (context.elapsed_ms > 30000) {
    return { status: "WARN", reason: "Execution time warning" };
  }
  return { status: "PASS" };
}
```

**4. 扩展技能组的匹配规则**

```yaml
# 在 SkillHub 注册表中添加新的匹配规则
- domain: "custom-domain"
  priority: 5  # 1-10，越高越优先
  match_rules:
    - type: "keyword"
      keywords: ["custom", "special"]
    - type: "semantic"
      threshold: 0.8
  target_skill: "custom-data-viz"
  target_agent: "agent-custom-vision"
```

#### 与主流 AI 框架的对比

| 维度 | LangChain | AutoGPT | King-Skill |
|------|-----------|---------|-----------|
| **路由策略** | Chain 串联 | 循环自省 | 四层递进（L0/L1/L2/L3） |
| **Token 优化** | 无内置优化 | 无 | 引力盆 0 token 直通 70% |
| **Agent 管理** | 手动编排 | 单 Agent | 20+ Agent + 视野隔离 |
| **质量保障** | 无内置 | 简单重试 | LSO-DAO + Hook+Loop + D1/D2/D3 |
| **扩展性** | 插件机制 | 插件 | 机械降神动态注入 |
| **学习机制** | 无 | 无 | 引力盆路径固化 |
| **Provider 管理** | 手动配置 | 固定 | cc-switch 统一管理 |
| **监控审计** | 基本日志 | 基本日志 | token-stats 全链路审计 |
| **适用场景** | 原型开发 | 实验项目 | 生产级多 Agent 协作 |

#### 系统架构决策树

当需要设计新的 AI 系统或扩展现有系统时，可以参考以下决策树：

```
开始：需要处理什么类型的请求？
│
├── 简单问答/信息查询
│   └── 走 L0 引力盆直通 → 不需要额外设计
│
├── 单一领域任务（写文档、做表格、生成代码等）
│   ├── SkillHub 中是否有匹配技能？
│   │   ├── 是 → L1 特征词典匹配 → 直接调用
│   │   └── 否 → 新增 Skill → 注册到 SkillHub
│
├── 需要精确参数匹配的任务（切换模型、查询 Token 等）
│   └── L2 工具签名匹配 → 确认 cc-switch/token-stats 是否有对应工具
│       ├── 是 → 直接调用
│       └── 否 → 扩展对应技能的 MCP 工具集
│
├── 复杂多步骤任务（开发一个完整功能）
│   ├── 需要几个 Agent 协作？
│   │   ├── 1个 → L3 LLM 裁决 → 单 Agent 完成
│   │   ├── 2-3个 → L3 + 呼吸管道编排 → 多 Agent 串行
│   │   └── 4个以上 → L3 + LSO-DAO 五境 → 多 Agent 并行+递进
│   └── 是否需要新增 Agent？
│       ├── 是 → 定义 Agent → 注册到 agent-list → 设置视野隔离
│       └── 否 → 路由到现有 Agent
│
├── 需要质量保障的任务
│   ├── 基础质量 → Hook 1 + D1 格式校验
│   ├── 功能质量 → Hook 2 + D2 功能验证
│   └── 全面质量 → Hook 3 + D3 性能评估 + Hook 4 完成确认
│
└── 需要持续优化的任务
    └── 启用引力盆学习 → 路径自动固化
```

---

## 五、文件索引

### 教材文件索引

| 目录 | 内容 | 适用读者 |
|------|------|---------|
| `textbooks/Phase-1/` | 基础入门教材 | 零基础初学者 |
| `textbooks/Phase-2/` | 进阶应用教材 | 有编程经验者 |
| `textbooks/Phase-3/` | 高级架构教材 | 开发者/架构师 |

### King-Skill 文件索引

| 文件 | 内容 | 核心概念 | 推荐读者 |
|------|------|---------|---------|
| `King-Skill-System/README.md` | King-Skill 系统概述 | 系统总览、快速路径 | 所有读者 |
| `King-Skill-System/01-King-Agent-Design.md` | King Agent 设计理念 | 假借修真、机械降神、Hook+Loop、D1/D2/D3 | 所有读者 |
| `King-Skill-System/02-System-Prompt-Architecture.md` | System Prompt 架构 | 四层结构、延迟加载、视野隔离、依赖链 | 架构师、研究者 |
| `King-Skill-System/03-Routing-Topology.md` | 三技能路由拓扑 | cc-switch、token-stats、master-engineer-hub 联动 | 所有读者 |
| `King-Skill-System/04-SkillHub-Registry.md` | SkillHub 注册与分发 | 8 大技能组、注册流程、匹配机制 | 开发者 |
| `King-Skill-System/05-LSO-DAO-Five-Realms.md` | 五境递进架构 | 筑基→金丹→元婴→化神→渡劫、呼吸管道 | 架构师 |
| `King-Skill-System/06-Agent-Cluster-20plus.md` | 20+ Agent 集群 | 核心 7 + 工程 8 + 三合一 5 + 路由 3 | 开发者、架构师 |
| `King-Skill-System/07-Hook-Loop-Engineering.md` | Hook+Loop 工程约束 | 4 Hook、D1/D2/D3、3 次循环 | 开发者、架构师 |
| `King-Skill-System/08-Complete-Topology-Map.md` | 完整拓扑图 | 7 层架构、数据流路径、依赖关系 | 所有读者 |
| `King-Skill-System/09-System-Prompt-References.md` | System Prompt 引用图 | 22 文件依赖链、引用统计 | 架构师、研究者 |

---

## 六、推荐阅读路径

### 路径一：从零开始的 AI 学习者

```
第一步：阅读 AI 教材 Phase 1（基础概念）
第二步：阅读 King-Skill-System/01-King-Agent-Design.md（理解 AI 智能体）
第三步：阅读 King-Skill-System/08-Complete-Topology-Map.md（看系统全景）
第四步：继续 AI 教材 Phase 2（进阶应用）
第五步：阅读 King-Skill-System/03-Routing-Topology.md（理解路由机制）
```

### 路径二：有经验的开发者

```
第一步：阅读 King-Skill-System/08-Complete-Topology-Map.md（全景图）
第二步：阅读 King-Skill-System/01-King-Agent-Design.md（核心概念）
第三步：阅读 King-Skill-System/03-Routing-Topology.md（路由机制）
第四步：阅读 King-Skill-System/06-Agent-Cluster-20plus.md（Agent 体系）
第五步：阅读 King-Skill-System/07-Hook-Loop-Engineering.md（质量体系）
第六步：根据兴趣深入其他文件
```

### 路径三：架构师/研究者

```
第一步：快速浏览所有文件（建立全局认知）
第二步：深入 King-Skill-System/02-System-Prompt-Architecture.md（System Prompt）
第三步：深入 King-Skill-System/05-LSO-DAO-Five-Realms.md（五境体系）
第四步：深入 King-Skill-System/09-System-Prompt-References.md（依赖关系）
第五步：对照 King-Skill-System/08-Complete-Topology-Map.md（验证理解）
第六步：阅读三档用户指南中的"深度使用老手指南"（架构决策树）
```

---

> **King-Skill 系统** -- 不持一物，而万物皆备。不预一器，而万器待召。