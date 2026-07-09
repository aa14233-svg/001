# SkillHub 技能注册与分发中心

> SkillHub 是 King Agent 的"技能总览台"——所有技能在这里注册，所有请求在这里路由。

## 一、SkillHub 整体架构

```
用户请求
  │
  ▼
SkillHub 唯一入口
  │
  ├── 领域识别
  │    ├── 代码/开发
  │    ├── 文档/写作
  │    ├── 数据/分析
  │    ├── 设计/创意
  │    ├── 部署/运维
  │    ├── 安全/合规
  │    ├── 创意/生成
  │    └── 学习/研究
  │
  ├── 域激活
  │    └── 激活对应技能域下的所有技能
  │
  ├── 技能匹配
  │    ├── 精确匹配：关键词命中（Level 0/1）
  │    └── 模糊匹配：语义相似度 > 0.7（Level 3）
  │
  ├── 路由选择
  │    ├── 对应 Agent 选择（A/B/C）
  │    └── 工具注入 + 视野隔离
  │
  └── 执行监控
       ├── Hook 检查（4个节点）
       ├── Token 审计（token-stats）
       └── 引力盆更新（高频路径固化）
```

## 二、8 大技能组

### 组1：路由与调度（核心技能）

| 技能 | 功能 | 依赖 | 工具数 | 路由归属 |
|------|------|------|--------|---------|
| `master-engineer-hub` | 路由编排 + 引力盆 + 视野隔离 | 无 | 4 + 网关 | 入口网关 |
| `cc-switch` | Provider 切换 + MCP 管理 | master-engineer-hub | 10 | Agent B/C |
| `token-stats` | Token 审计 + 预算管理 | master-engineer-hub | 6 | Agent B/C |
| `路由skill` | 三体合一 + 域映射 | master-engineer-hub | — | 路由引擎 |

### 组2：基础设施（LSO-DAO）

| 技能 | 功能 | 依赖 |
|------|------|------|
| `lso-os` | 五境递进 + 20+ Agent 集群 | 路由组 |
| `omnipotent-engineer` | 七维全栈开发 | lso-os |

### 组3：文档与内容

| 技能 | 功能 | 工具数 | 典型场景 |
|------|------|--------|---------|
| `doc-writing-guide` | 文档写作指南 | — | PRD、技术文档、手册 |
| `docx` | Word 文档创建/编辑 | — | 报告、合同、标书 |
| `pdf` | PDF 处理 | — | PDF 提取、合并、转换 |

### 组4：演示与报告

| 技能 | 功能 | 工具数 | 典型场景 |
|------|------|--------|---------|
| `pptx` | PowerPoint 演示文稿 | — | 路演 PPT、汇报 |
| `html-deck` | HTML 幻灯片 | — | 技术分享、在线展示 |
| `html-report` | HTML 报告/仪表盘 | — | 数据分析报告、看板 |

### 组5：数据与表格

| 技能 | 功能 | 工具数 | 典型场景 |
|------|------|--------|---------|
| `xlsx` | 电子表格处理 | — | 数据清洗、报表生成 |

### 组6：编码优化

| 技能 | 功能 | 工具数 | 典型场景 |
|------|------|--------|---------|
| `ai-coding-optimizer` | 5 MCP 编码优化工具 | 5 | 代码审查、性能优化 |

### 组7：协作

| 技能 | 功能 | 工具数 | 典型场景 |
|------|------|--------|---------|
| `trae-auto-collaboration` | 双应用自动协作 | 5 | 跨应用工作流 |
| `trae-collaboration` | 双应用协作桥接 | 5 | 应用间数据同步 |

### 组8：学习与研究

| 技能 | 功能 | 典型场景 |
|------|------|---------|
| `research-guide` | 研究分析指南 | 竞品分析、技术调研 |
| `TRAE-product-knowledge` | Trae 产品知识 | 产品问答、功能咨询 |

## 三、技能注册过程

### 1. 注册数据结构

每个技能在 SkillHub 中注册时包含以下结构：

```json
{
  "skill_name": "cc-switch",
  "version": "1.0",
  "category": "routing_management",
  "description": "Provider 切换与 MCP 管理",
  "tools": [
    { "name": "ccs_provider_list", "owner": "agent_c", "function": "list_providers", "type": "read" },
    { "name": "ccs_provider_switch", "owner": "agent_b", "function": "switch_provider", "type": "write" },
    { "name": "ccs_mcp_sync", "owner": "agent_b", "function": "sync_mcp", "type": "write" }
  ],
  "dependencies": ["master-engineer-hub"],
  "activation_conditions": ["provider", "mcp", "sync", "env"],
  "activation_keywords": ["provider", "模型", "切换", "mcp", "同步"],
  "cost_model": { "base_tokens": 50, "per_tool_call": 10 }
}
```

```json
{
  "skill_name": "token-stats",
  "version": "1.0",
  "category": "routing_management",
  "description": "Token 审计与预算管理",
  "tools": [
    { "name": "token_stats_query", "owner": "agent_c", "function": "query_stats", "type": "read" },
    { "name": "token_stats_compare", "owner": "agent_c", "function": "compare_costs", "type": "read" },
    { "name": "token_stats_budget", "owner": "agent_b", "function": "manage_budget", "type": "write" }
  ],
  "dependencies": ["master-engineer-hub"],
  "activation_conditions": ["token", "budget", "cost", "audit"],
  "activation_keywords": ["token", "预算", "成本", "审计", "消耗"],
  "cost_model": { "base_tokens": 30, "per_tool_call": 5 }
}
```

### 2. 路由匹配过程

```
请求 q: "帮我写一份 AI 入门报告"
  │
  ├── 步骤1：提取关键词 / 语义向量
  │    ├── 关键词：["写", "报告", "AI", "入门"]
  │    └── 语义向量：[0.23, 0.87, 0.12, ...]
  │
  ├── 步骤2：匹配技能域
  │    ├── "报告" → 文档域（置信度 0.92）
  │    └── "AI" → 学习域（置信度 0.45，较低）
  │
  ├── 步骤3：域激活 → 文档组
  │    ├── doc-writing-guide（写作指南）
  │    ├── docx（Word 创建）
  │    └── pdf（PDF 输出）
  │
  ├── 步骤4：技能匹配
  │    ├── 精确匹配："报告" → doc-writing-guide
  │    └── 模糊匹配：docx（适合创建文档）
  │
  └── 步骤5：路由到对应 Agent
       ├── Agent C：搜索参考材料 → 调用 Read/Search
       └── Agent B：创建文档 → 调用 docx 工具
```

## 四、SkillHub 与 System Prompt 的联动

```
SkillHub 匹配技能
  │
  ├── 加载对应 SKILL.md
  │    ├── 工具定义
  │    │    ├── 工具名称
  │    │    ├── 输入参数
  │    │    ├── 输出格式
  │    │    └── 权限要求
  │    │
  │    ├── 路由规则
  │    │    ├── 归属 Agent
  │    │    ├── 调用条件
  │    │    └── 错误处理
  │    │
  │    └── 调用约束
  │         ├── Token 预算上限
  │         ├── 超时限制
  │         └── 重试策略
  │
  ├── 注入到当前 Agent 视野
  │    ├── Agent 获得该技能的工具集
  │    ├── 其他域的工具保持不可见
  │    └── 工具上限限制（B≤4, C≤5, A≤2）
  │
  └── 执行完成后
       ├── Token 审计 → 记录消耗
       ├── 引力盆频次更新
       └── 工具从视野中移除
```

## 五、技能调用流程示例

### 示例1：编写一份 Word 文档

```
用户："帮我写一份 AI 入门报告"
  │
  ▼
[1] SkillHub 领域识别 → 文档/写作
  │  关键词：["写", "报告", "AI"]
  │  置信度：0.92
  │
  ▼
[2] 域激活 → 文档组（doc-writing-guide, docx, pdf）
  │  激活条件：含 "写/生成/创建" + "文档/报告/文件"
  │
  ▼
[3] 技能匹配 → doc-writing-guide（写作指南）+ docx（Word 创建）
  │  doc-writing-guide：提供写作模板和结构
  │  docx：提供 Word 文档的创建 API
  │
  ▼
[4] 路由选择 → Agent C（搜索文件+读取）→ Agent B（创建文档）
  │  Agent C：搜索相关参考资料
  │  Agent B：生成 Word 文档
  │
  ▼
[5] 执行 → 生成 Word 文档
  │  ├── Agent C：读取参考模板
  │  ├── doc-writing-guide：生成文档结构
  │  └── docx：创建并格式化文档
  │
  ▼
[6] 完成
  │  ├── Hook 4：完整检查
  │  ├── token-stats 审计
  │  └── 引力盆更新
```

### 示例2：切换 AI 模型

```
用户："切换到大模型"
  │
  ▼
[1] SkillHub 领域识别 → 代码/开发（路由管理）
  │  关键词：["切换", "大模型"]
  │  置信度：0.95
  │
  ▼
[2] 引力盆命中 → Level 0 直通
  │  pattern: "切换.*provider" hit: 12
  │  Token 成本：0
  │
  ▼
[3] master-engineer-hub 路由 → cc-switch
  │  0 token，无需加载 SKILL.md
  │
  ▼
[4] ccs_provider_list + ccs_provider_switch
  │  ├── ccs_provider_list → 列出可用 Provider
  │  └── ccs_provider_switch → 切换到目标
  │
  ▼
[5] 完成
  │  ├── token_stats_audit → 记录消耗
  │  ├── 引力盆命中 + 1
  │  └── 返回切换结果
```

### 示例3：创建演讲 PPT

```
用户："基于上周的数据做一份销售汇报 PPT"
  │
  ▼
[1] SkillHub 领域识别 → 演示/报告 + 数据/分析
  │  多域识别：["PPT", "汇报"] → 演示域
  │            ["数据", "销售"] → 数据域
  │
  ▼
[2] 域激活 → 演示组（pptx）+ 数据组（xlsx）
  │
  ▼
[3] 技能匹配
  │  ├── pptx：创建 PPT 文件
  │  └── xlsx：读取和分析数据
  │
  ▼
[4] 路由 → Agent C（读取数据）→ Agent B（创建 PPT）
  │
  ▼
[5] 执行
  │  ├── Agent C：读取上周销售数据
  │  ├── xlsx：数据分析 → 生成图表数据
  │  ├── Agent B：pptx 创建 PPT
  │  └── Hook 3：D1/D2/D3 验收
  │
  ▼
[6] 完成 → 交付销售汇报 PPT
```

## 六、技能注册与卸载规则

| 操作 | 触发条件 | 流程 |
|------|---------|------|
| 注册 | 新 Skill 文件放入指定目录 | 读取 SKILL.md → 解析注册信息 → 添加到技能索引 |
| 激活 | 域识别命中 | 激活对应技能组 → 加载 SKILL.md → 注入 Agent 视野 |
| 卸载 | 30 天未使用 | 从技能索引移除 → 清理缓存 → 通知管理者 |
| 更新 | SKILL.md 变更 | 重新解析 → 更新注册信息 → 版本号 + 1 |

---

## 补充说明

### 1. 多域识别与冲突处理

当一个请求涉及多个技能域时，SkillHub 的冲突处理策略：

```
请求："把这份 Excel 数据做成 PPT"

1. 域识别结果：
   ├── 数据域（xlsx）：置信度 0.90
   └── 演示域（pptx）：置信度 0.85

2. 冲突处理：
   ├── 优先级：数据 > 演示（先读取数据再生成展示）
   └── 执行顺序：串行（Agent C 读数据 → Agent B 做 PPT）
```

**冲突解决规则**：
- 多域命中时，按依赖关系排序（数据依赖先执行）
- 同一请求需要多技能协作时，使用 Agent 间通信协议
- 优先级不明确时，默认由 master-engineer-hub 决定执行顺序

### 2. SkillHub 与 master-engineer-hub 的分工

| 职责 | SkillHub | master-engineer-hub |
|------|---------|-------------------|
| 技能注册 | 维护技能注册表 | 不参与 |
| 领域识别 | 8 大域识别 | 不参与 |
| 技能匹配 | 匹配具体技能 | 不参与 |
| 路由决策 | 不参与 | Level 0/1/2/3 路由 |
| 引力盆管理 | 不参与 | 动态固化高频路径 |
| Agent 视野 | 注入工具 | 隔离决策 |
| Token 审计 | 不参与 | 触发 token-stats |

**协作模式**：
```
SkillHub 负责"找对技能"
  └── 结果 → master-engineer-hub
       └── 负责"找对 Agent 执行"
            └── 结果 → 对应 Agent
```

### 3. 技能匹配的置信度阈值

| 匹配类型 | 置信度阈值 | 说明 |
|---------|-----------|------|
| Level 0 引力盆 | 1.0 | 完全精确匹配 |
| Level 1 特征词典 | ≥0.9 | 关键词/regex 匹配 |
| Level 2 工具签名 | ≥0.8 | 工具名称片段匹配 |
| Level 3 语义匹配 | ≥0.7 | LLM 语义解析 |
| 域识别 | ≥0.6 | 激活对应技能域 |
| 多域识别 | ≥0.4 | 辅助域识别 |

### 4. 技能调用统计与监控

SkillHub 内置调用统计，记录每个技能的：
- 调用次数
- 平均 Token 消耗
- 成功率/失败率
- 平均响应时间
- 最后调用时间

这些数据通过 token-stats 的 `token_stats_report` 工具生成定期报告，用于技能优化和预算管理。