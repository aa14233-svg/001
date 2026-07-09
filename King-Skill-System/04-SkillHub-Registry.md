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
  │    └── 在激活域中匹配最合适的技能
  │
  ├── 路由选择
  │    └── 选择对应 Agent 执行
  │
  └── 执行监控
       └── Hook 检查 + Token 审计
```

## 二、8 大技能组

### 组1：路由与调度（核心技能）

| 技能 | 功能 | 依赖 | 工具数 |
|------|------|------|--------|
| `master-engineer-hub` | 路由编排 + 引力盆 + 视野隔离 | 无 | 4 + 网关 |
| `cc-switch` | Provider 切换 + MCP 管理 | master-engineer-hub | 10 |
| `token-stats` | Token 审计 + 预算管理 | master-engineer-hub | 6 |
| `路由skill` | 三体合一 + 域映射 | master-engineer-hub | — |

### 组2：基础设施（LSO-DAO）

| 技能 | 功能 | 依赖 |
|------|------|------|
| `lso-os` | 五境递进 + 20+ Agent 集群 | 路由组 |
| `omnipotent-engineer` | 七维全栈开发 | lso-os |

### 组3：文档与内容

| 技能 | 功能 | 工具数 |
|------|------|--------|
| `doc-writing-guide` | 文档写作指南 | — |
| `docx` | Word 文档创建/编辑 | — |
| `pdf` | PDF 处理 | — |

### 组4：演示与报告

| 技能 | 功能 | 工具数 |
|------|------|--------|
| `pptx` | PowerPoint 演示文稿 | — |
| `html-deck` | HTML 幻灯片 | — |
| `html-report` | HTML 报告/仪表盘 | — |

### 组5：数据与表格

| 技能 | 功能 | 工具数 |
|------|------|--------|
| `xlsx` | 电子表格处理 | — |

### 组6：编码优化

| 技能 | 功能 | 工具数 |
|------|------|--------|
| `ai-coding-optimizer` | 5 MCP 编码优化工具 | 5 |

### 组7：协作

| 技能 | 功能 | 工具数 |
|------|------|--------|
| `trae-auto-collaboration` | 双应用自动协作 | 5 |
| `trae-collaboration` | 双应用协作桥接 | 5 |

### 组8：学习与研究

| 技能 | 功能 |
|------|------|
| `research-guide` | 研究分析指南 |
| `TRAE-product-knowledge` | Trae 产品知识 |

## 三、技能注册过程

### 1. 注册数据结构

```json
{
  "skill_name": "cc-switch",
  "version": "1.0",
  "category": "routing_management",
  "tools": [
    { "name": "ccs_provider_list", "owner": "agent_c", "function": "list_providers" },
    { "name": "ccs_provider_switch", "owner": "agent_b", "function": "switch_provider" }
  ],
  "dependencies": ["master-engineer-hub"],
  "activation_conditions": ["provider", "mcp", "sync", "env"]
}
```

### 2. 路由匹配过程

```
请求 q
  │
  ├── 提取关键词 / 语义向量
  │
  ├── 匹配技能域
  │    └── 代码 → 开发域
  │
  ├── 匹配具体技能
  │    └── 开发域 → omnipotent-engineer
  │
  └── 路由到对应 Agent
       └── Agent B（工程/编码）
```

## 四、SkillHub 与 System Prompt 的联动

```
SkillHub 匹配技能
  │
  ├── 加载对应 SKILL.md
  │    ├── 工具定义
  │    ├── 路由规则
  │    └── 调用约束
  │
  ├── 注入到当前 Agent 视野
  │    └── Agent 获得该技能的工具集
  │
  └── 执行完成后
       └── Token 审计 → 更新引力盆频次
```

## 五、技能调用流程示例

### 示例：编写一份 Word 文档

```
用户："帮我写一份 AI 入门报告"
  │
  ▼
[1] SkillHub 领域识别 → 文档/写作
  │
  ▼
[2] 域激活 → 文档组（doc-writing-guide, docx, pdf）
  │
  ▼
[3] 技能匹配 → doc-writing-guide（写作指南）+ docx（Word 创建）
  │
  ▼
[4] 路由选择 → Agent C（搜索文件+读取）→ Agent B（创建文档）
  │
  ▼
[5] 执行 → 生成 Word 文档
  │
  ▼
[6] 完成 → Token 审计 + 引力盆更新
```

### 示例：切换 AI 模型

```
用户："切换到大模型"
  │
  ▼
[1] SkillHub 领域识别 → 代码/开发（路由管理）
  │
  ▼
[2] 引力盆命中 → Level 0 直通
  │
  ▼
[3] master-engineer-hub 路由 → cc-switch
  │
  ▼
[4] ccs_provider_list + ccs_provider_switch
  │
  ▼
[5] 完成 → 引力盆命中 + 1
```