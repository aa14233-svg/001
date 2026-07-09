# 迁移部署指南：King-Skill 全系路由系统跨工具迁移方案

> 如何将 King Agent 为中心拓扑展开的智能体技能路由群系系统，迁移部署到其他代码工具。
>
> **核心哲学**：不持一物，而万物皆备。不预一器，而万器待召。

---

## 一、迁移概述

### 1.1 为什么要迁移

King-Skill 系统最初在 Trae IDE 中孕育，但其设计理念——**假借修真、四层路由、引力盆、机械降神、视野隔离、Hook+Loop**——是**工具无关的元架构**。这套系统本质上是 AI 智能体领域的"操作系统内核"，可以部署到任何支持 AI Agent 或 MCP 协议的代码工具中。

| 迁移目标 | 适用场景 | 迁移难度 | 收益 |
|---------|---------|---------|------|
| **Cursor** | AI-first IDE，原生支持 Rules | ★☆☆ | 可直接复用 80% 规则 |
| **VS Code** | 最广泛使用，扩展生态丰富 | ★★☆ | 通过 Cline/Continue 插件承载 |
| **Windsurf** | AI 流式 IDE，Cascade 引擎 | ★★☆ | 适配 Cascade 模式的 Rule 系统 |
| **GitHub Copilot** | 代码补全+Chat | ★★★ | 通过 .github/copilot-instructions.md |
| **JetBrains** | 企业级 IDE | ★★★ | 通过 AI Assistant 插件 |
| **Claude Code** | CLI 原生 AI 编程 | ★★☆ | 通过 CLAUDE.md 规则文件 |
| **自定义工具** | 自研 AI 平台 | ★★★ | 全量适配，但自由度最高 |

### 1.2 迁移的核心组件

King-Skill 系统的迁移不是简单复制文件，而是将其**四层路由引擎、SkillHub 技能注册体系、Agent 视野隔离、LSO-DAO 五境递进、Hook+Loop 质量保障**这五大核心组件适配到目标平台。

```
┌─────────────────────────────────────────────────────────────┐
│               King-Skill 迁移核心组件包                       │
├─────────────────────────────────────────────────────────────┤
│  [1] 路由引擎层 (master-engineer-hub)                        │
│  │  ├─ 四层路由：L0 引力盆 / L1 特征词典 / L2 签名 / L3 LLM  │
│  │  ├─ 机械降神协议：能力缺口检测 → 动态注入 → 消散回收       │
│  │  └─ Agent 视野隔离：A(≤2) / B(≤4) / C(≤5) 工具限制       │
│  ├─────────────────────────────────────────────────────────┤
│  [2] 技能注册层 (SkillHub)                                    │
│  │  ├─ 8 大技能组的域识别 + 匹配 + 路由分发                    │
│  │  └─ 20+ 技能文件的路由注册表                               │
│  ├─────────────────────────────────────────────────────────┤
│  [3] Provider 管理层 (cc-switch)                              │
│  │  ├─ Provider 列表 / 切换 / 测试 / 连通性检测                │
│  │  └─ MCP Server 同步 / Skills 同步                          │
│  ├─────────────────────────────────────────────────────────┤
│  [4] Token 审计层 (token-stats)                               │
│  │  ├─ Token 用量查询 / 成本对比 / 预算管理 / 审计            │
│  │  └─ 全链路 Token 监控 + 告警配置                           │
│  ├─────────────────────────────────────────────────────────┤
│  [5] 质量保障体系 (LSO-DAO + Hook+Loop)                       │
│  │  ├─ 五境递进：筑基→金丹→元婴→化神→渡劫                     │
│  │  ├─ 4 Hook 节点 + D1/D2/D3 三级验收 + 3 次循环            │
│  │  └─ 呼吸管道：吸→存→呼→化→归                               │
│  └─────────────────────────────────────────────────────────┘
```

---

## 二、迁移前的准备工作

### 2.1 环境评估清单

在开始迁移之前，先评估目标平台的能力边界：

| 评估项 | 说明 | 影响组件 |
|-------|------|---------|
| **MCP 协议支持** | 是否支持 Model Context Protocol | cc-switch、token-stats |
| **自定义规则文件** | 是否支持 `.cursorrules` / `CLAUDE.md` 等 | 路由引擎规则 |
| **Agent 自定义** | 是否支持自定义 Agent/工具/插件 | 所有 Agent 层 |
| **System Prompt 注入** | 是否能注入自定义 System Prompt | 路由逻辑、视野隔离 |
| **工具调用上限** | 单个 Agent 可调用工具数量的上限 | Agent 视野隔离 |
| **循环/重试机制** | 是否支持自动重试或循环 | Hook+Loop 体系 |
| **扩展生态** | 插件/扩展市场的丰富程度 | SkillHub 技能注册 |

### 2.2 迁移策略选择

根据目标平台的能力，选择最适合的迁移策略：

```
策略 A：规则文件迁移（推荐首选）
  适用：Cursor、Windsurf、Claude Code
  方法：将路由规则、技能定义写入平台支持的规则文件
  优势：零插件、零配置、即开即用
  局限：动态能力受限（机械降神、引力盆自学习）

策略 B：MCP 扩展迁移
  适用：VS Code (Cline/Continue)、JetBrains、自定义工具
  方法：将 cc-switch、token-stats 等 MCP Server 迁移到目标平台
  优势：保留完整动态能力
  局限：需要安装扩展/插件

策略 C：全量架构迁移
  适用：自研 AI 平台、企业级定制
  方法：完整移植 King-Skill 全部架构
  优势：100% 保留所有能力
  局限：工作量最大
```

---

## 三、目标平台迁移指南

### 3.1 迁移到 Cursor

Cursor 原生支持 `.cursorrules` 规则文件，这是迁移 King-Skill 系统的最佳平台之一。

#### 步骤一：创建核心规则文件

在项目根目录或 `~/.cursor/` 下创建 `.cursorrules` 文件，注入 King Agent 的核心路由逻辑：

```markdown
# King Agent 路由规则 (Cursor 适配版)

## 路由引擎
- 所有请求先过四层路由：L0 引力盆 → L1 特征词典 → L2 工具签名 → L3 LLM
- 引力盆优先级最高，L0 命中后直接返回，不消耗额外 Token

## Agent 视野隔离
- A类（分析/设计）：最多暴露 2 个工具
- B类（架构/工程）：最多暴露 4 个工具  
- C类（编码/文件）：最多暴露 5 个工具

## 技能注册表
- 文档技能：docx、pdf → 域关键词 [文档, 报告, 论文, 合同]
- 演示技能：pptx、html-deck → 域关键词 [演示, 幻灯片, 演讲, 汇报]
- 数据技能：xlsx → 域关键词 [表格, 数据, 统计, 分析]
- 编码技能：omnipotent-engineer → 域关键词 [代码, 开发, 编程, 调试]
- 搜索技能：research-guide → 域关键词 [搜索, 研究, 调研, 分析]
- 写作技能：doc-writing-guide → 域关键词 [写作, PRD, 需求, 文档]

## 质量保障
- 每次输出前执行 D1（格式校验）、D2（功能验证）、D3（性能评估）
- 不通过时自动修正，最多 3 次循环
```

#### 步骤二：配置 MCP Server（可选加强）

如果 Cursor 支持 MCP，可配置 cc-switch 和 token-stats 的 MCP Server：

```json
{
  "mcpServers": {
    "cc-switch": {
      "command": "pwsh",
      "args": ["-File", "E:\\cc-switch\\mcp-server.ps1"]
    },
    "token-stats": {
      "command": "node",
      "args": ["E:\\cc-switch\\mcp\\token-stats-server.js"]
    }
  }
}
```

#### 步骤三：创建多文件组织

将 King-Skill 的 9 个核心文档放入项目中的 `King-Skill-System/` 目录，并在 `.cursorrules` 中引用：

```
# 知识库引用
- 详细设计理念参考：King-Skill-System/01-King-Agent-Design.md
- 路由拓扑参考：King-Skill-System/03-Routing-Topology.md
- 完整拓扑图参考：King-Skill-System/08-Complete-Topology-Map.md
```

---

### 3.2 迁移到 VS Code（通过 Cline/Continue 插件）

VS Code 本身不直接支持 AI 路由规则，但通过 **Cline** 或 **Continue** 插件可以实现。

#### 方案 A：Cline 插件

Cline 支持 `.clinerules` 规则文件，用法类似 Cursor：

```markdown
# .clinerules — King-Skill 路由规则 (Cline 适配版)

## 四层路由规则
1. 先检查引力盆（缓存的高频成功路径）
2. 再匹配特征词典（关键词 + 域识别）
3. 然后匹配工具签名（精确参数匹配）
4. 最后 LLM 兜底裁决

## SkillHub 技能注册表
[docx] → 域：文档/报告/论文/合同
[pptx] → 域：演示/幻灯片/演讲
[xlsx] → 域：表格/数据/统计
[pdf]  → 域：PDF/表单/扫描件
[html-report] → 域：报告/网页/仪表盘
[html-deck] → 域：演示/幻灯片/网页
[research-guide] → 域：搜索/研究/调研
[doc-writing-guide] → 域：写作/PRD/需求

## 质量保障
- 遵循 D1 → D2 → D3 三级验收流程
- 不通过时最多 3 次重试
```

#### 方案 B：Continue 插件

Continue 支持 `.continuerc.json` 配置文件：

```json
{
  "models": [
    {
      "title": "King-Skill 路由模式",
      "provider": "openai",
      "model": "gpt-4o",
      "systemMessage": "你是一个遵循 King-Skill 路由规则的系统。请参考仓库中的 King-Skill-System/ 目录学习路由规则。"
    }
  ],
  "rules": [
    {
      "file": ".clinerules",
      "description": "King-Skill 路由规则"
    }
  ]
}
```

#### 方案 C：MCP Server 全量迁移

将 cc-switch 和 token-stats 的 MCP Server 配置到 VS Code 的 `.vscode/mcp.json`：

```json
{
  "mcpServers": {
    "cc-switch": {
      "command": "pwsh",
      "args": ["-File", "E:\\cc-switch\\mcp-server.ps1"]
    },
    "token-stats": {
      "command": "node",
      "args": ["E:\\cc-switch\\mcp\\token-stats-server.js"]
    },
    "master-engineer-hub": {
      "command": "pwsh",
      "args": ["-File", "E:\\cc-switch\\mcp\\hub-server.ps1"]
    }
  }
}
```

---

### 3.3 迁移到 Windsurf

Windsurf 使用 **Cascade** 引擎，支持 `.windsurfrules` 规则文件。

#### 创建规则文件

```markdown
# .windsurfrules — King-Skill 路由规则 (Windsurf 适配版)

## 核心路由原则
- 假借修真：90% 请求通过缓存/规则直通，0 Token 消耗
- 机械降神：当能力不足时，动态搜索和注入新能力
- 引力盆：高频路径自动固化，越用越快

## 请求路由流程
1. 解析用户意图 → 提取域关键词
2. 匹配 SkillHub 技能注册表 → 确定使用哪个技能
3. 根据技能确定 Agent 归属（A/B/C）
4. 执行 + 质量验收（D1/D2/D3）
5. 成功路径写入引力盆缓存

## 域关键词表
- 文档创作：docx, pdf, 报告, 论文, 合同
- 数据表格：xlsx, 表格, 数据, 统计
- 演示制作：pptx, 演示, 幻灯片
- 报告生成：html-report, 报告, 仪表盘
- 研究分析：research-guide, 搜索, 调研
- 写作指导：doc-writing-guide, 写作, PRD
```

---

### 3.4 迁移到 Claude Code

Claude Code 使用 `CLAUDE.md` 规则文件，适合命令行 AI 编程。

```markdown
# CLAUDE.md — King-Skill 路由规则 (Claude Code 适配版)

## 路由系统
你运行在 King-Skill 路由架构下，请遵循以下规则：

### 四层路由
- L0 引力盆：用户请求 → 检查缓存路径 → 命中则直接执行
- L1 特征词典：提取关键词 → 匹配域 → 确定技能
- L2 工具签名：精确匹配工具参数 → 调用对应工具
- L3 LLM 裁决：LLM 分析意图 → 生成路由 → 执行 → 写入引力盆

### 可用技能
- 当你需要创建文档时，使用 docx 技能
- 当你需要制作演示时，使用 pptx 或 html-deck 技能
- 当你需要处理数据时，使用 xlsx 技能
- 当你需要搜索研究时，使用 research-guide 技能

### 质量要求
- 每次输出前检查格式、功能完整性、性能
- 不通过时修正，最多 3 次
- 记录每次成功路径供后续复用
```

---

### 3.5 迁移到 GitHub Copilot

GitHub Copilot 通过 `.github/copilot-instructions.md` 支持自定义指令。

```markdown
# Copilot 指令 — King-Skill 路由规则 (Copilot 适配版)

## 路由规则
- 用户请求 → 提取域关键词 → 匹配技能 → 执行
- 优先使用缓存路径（如果之前的类似请求有成功方案）
- 当需要新能力时，先搜索可用的库或工具，再注入使用

## 技能匹配指南
- 代码/开发类请求 → 专注于编码实现
- 文档/注释类请求 → 确保文档完整清晰
- 调试/修复类请求 → 先定位问题，再提出修复方案

## 输出质量
- 确保代码完整可运行
- 提供清晰的注释和说明
- 检查边界情况和错误处理
```

---

### 3.6 迁移到 JetBrains (IntelliJ IDEA)

JetBrains 通过 **AI Assistant** 插件支持自定义提示词。

```markdown
# JetBrains AI Assistant 自定义提示词 — King-Skill 路由规则

## 路由模式
你是一个遵循四层路由的 AI 编程助手：

1. 领域识别：根据用户请求中的关键词，判断属于哪个领域
2. 路由匹配：根据领域匹配到对应的 Skill 和 Agent
3. 执行策略：按照匹配到的策略执行任务
4. 质量验收：检查输出是否符合预期

## 技能领域表
- 后端开发：Java, Kotlin, Spring, 数据库, API
- 前端开发：React, Vue, TypeScript, CSS, HTML
- 数据工程：SQL, ETL, 数据处理, 分析
- 基础设施：Docker, Kubernetes, CI/CD, 部署
```

---

## 四、嵌入式模型自适应机制

> 这是 King-Skill 迁移方案的核心创新——让模型根据自身性能和多模态专长，**自动调整路由行为**。

### 4.1 自适应机制的设计原理

不同的 AI 模型有不同的能力特点。例如：
- **GPT-4o**：强多模态（文本+图像+音频），适合 A 类（分析/设计）和 C 类（编码）
- **Claude 3.5 Sonnet**：强文本理解，擅长代码和文档，适合 B 类（架构）和 C 类（编码）
- **DeepSeek V4**：强代码生成，推理速度快，适合 B 类（工程）和 C 类（编码）
- **Gemini 2.0**：强多模态，长上下文，适合 A 类（分析）和搜索类任务
- **本地小模型 (Qwen2.5-7B)**：资源受限，应尽可能走 L0/L1 直通，避开 L3

自适应机制的核心是**模型自我认知**——模型在启动时评估自身能力，将评估结果写入路由决策的权重矩阵中。

### 4.2 自适应评估矩阵

在迁移配置文件中嵌入以下自我评估逻辑：

```json
{
  "self_adaptive": {
    "model_name": "{{MODEL_NAME}}",
    "self_assessment": {
      "multimodal": {
        "text": 0.95,
        "image_input": 0.9,
        "audio_input": 0.3,
        "video_input": 0.1,
        "code_generation": 0.95
      },
      "performance": {
        "latency_per_token_ms": 15,
        "max_context_tokens": 128000,
        "max_output_tokens": 4096,
        "supports_streaming": true
      },
      "reasoning": {
        "logical_reasoning": 0.9,
        "creative_generation": 0.85,
        "mathematical": 0.8,
        "translation": 0.9
      }
    },
    "routing_weights": {
      "A_agent_weight": "multimodal.text * 0.4 + reasoning.logical_reasoning * 0.3 + multimodal.image_input * 0.3",
      "B_agent_weight": "multimodal.code_generation * 0.4 + performance.latency_per_token_ms * 0.3 + reasoning.logical_reasoning * 0.3",
      "C_agent_weight": "multimodal.code_generation * 0.5 + performance.max_context_tokens * 0.3 + reasoning.creative_generation * 0.2"
    },
    "adaptive_rules": {
      "if_latency_high": "优先走 L0/L1 直通，减少 L3 调用",
      "if_multimodal_strong": "增加 A 类 Agent 权重，充分发挥多模态分析能力",
      "if_code_strong": "增加 B/C 类 Agent 权重，优先处理编码任务",
      "if_context_limited": "启用分块策略，减少单次 Token 消耗",
      "if_local_model": "禁用 L3 LLM 裁决，全量使用 L0/L1/L2"
    }
  }
}
```

### 4.3 自适应路由的 Python 实现

以下是一个可嵌入迁移配置中的自适应路由决策器：

```python
class SelfAdaptiveRouter:
    """嵌入式模型自适应路由决策器"""
    
    def __init__(self, model_profile=None):
        self.profile = model_profile or self._self_assess()
        self.gravity_basin = {}
        self.feature_dict = {}
        
    def _self_assess(self):
        """模型自我能力评估"""
        # 在实际部署中，这些值由模型在初始化时填充
        return {
            "multimodal_score": 0.85,      # 多模态能力评分
            "code_score": 0.92,             # 代码能力评分
            "latency_score": 0.75,          # 延迟评分（越高越好）
            "context_capacity": 128000,     # 上下文容量
            "reasoning_score": 0.88,        # 推理能力评分
            "is_local": False,              # 是否为本地模型
        }
    
    def route(self, request):
        """自适应路由决策"""
        profile = self.profile
        
        # 1. 查引力盆（所有模型通用）
        route_key = self._hash_request(request)
        if route_key in self.gravity_basin:
            return self.gravity_basin[route_key]  # L0 直通
        
        # 2. 特征词典匹配
        domain = self._match_domain(request)
        if domain and domain in self.feature_dict:
            return self.feature_dict[domain]  # L1 直通
        
        # 3. 自适应 Agent 分配
        if profile["is_local"]:
            # 本地模型：只走 L0/L1，不使用 L3
            return self._fallback_local(request)
        
        if profile["code_score"] > 0.85:
            # 代码能力强：优先分配 B/C 类 Agent
            agent_type = self._weighted_choice({
                "A": 0.2 * profile["multimodal_score"],
                "B": 0.4 * profile["code_score"],
                "C": 0.4 * profile["code_score"]
            })
        elif profile["multimodal_score"] > 0.8:
            # 多模态强：优先分配 A 类 Agent
            agent_type = self._weighted_choice({
                "A": 0.5 * profile["multimodal_score"],
                "B": 0.25 * profile["reasoning_score"],
                "C": 0.25 * profile["code_score"]
            })
        else:
            # 均衡分配
            agent_type = self._weighted_choice({
                "A": 0.33,
                "B": 0.33,
                "C": 0.34
            })
        
        # 4. 执行并记录（供引力盆学习）
        result = self._execute(agent_type, request)
        if result["success"]:
            self.gravity_basin[route_key] = {
                "agent_type": agent_type,
                "domain": domain,
                "success_count": 1
            }
        return result
    
    def _weighted_choice(self, weights):
        """加权随机选择"""
        import random
        total = sum(weights.values())
        r = random.uniform(0, total)
        upto = 0
        for agent, weight in weights.items():
            upto += weight
            if upto >= r:
                return agent
        return list(weights.keys())[-1]
    
    def _hash_request(self, request):
        """请求特征哈希"""
        import hashlib
        return hashlib.md5(request.encode()).hexdigest()[:16]
    
    def _match_domain(self, request):
        """域匹配"""
        domain_keywords = {
            "文档": ["文档", "报告", "论文", "合同", "docx"],
            "演示": ["演示", "幻灯片", "演讲", "ppt", "pptx"],
            "数据": ["表格", "数据", "统计", "分析", "xlsx"],
            "代码": ["代码", "开发", "编程", "调试", "函数"],
            "搜索": ["搜索", "研究", "调研", "查找", "查询"],
            "写作": ["写作", "PRD", "需求", "文档", "方案"]
        }
        for domain, keywords in domain_keywords.items():
            for kw in keywords:
                if kw in request:
                    return domain
        return None
    
    def _fallback_local(self, request):
        """本地模型降级策略"""
        domain = self._match_domain(request)
        if domain:
            return {"route": "L1", "agent": "C", "token_cost": 0}
        return {"route": "L0", "agent": "C", "token_cost": 0}
    
    def _execute(self, agent_type, request):
        """执行请求（桩函数，实际部署替换为真实逻辑）"""
        # 在实际部署中，这里调用对应 Agent 执行
        return {"success": True, "agent_type": agent_type, "token_cost": 50}
```

### 4.4 自适应机制在迁移配置中的嵌入方案

在目标平台支持的自定义指令文件中，嵌入以下自适应逻辑：

```markdown
## 自适应路由规则（嵌入到迁移配置中）

### 步骤 1：模型能力自检
在每次会话开始时，评估以下指标：
- 我是否支持多模态（图像输入）？→ 影响 A 类 Agent 权重
- 我的代码生成能力如何？→ 影响 B/C 类 Agent 权重
- 我的响应延迟是快还是慢？→ 影响是否使用 L3 LLM 裁决
- 我的上下文窗口大小？→ 影响分块策略

### 步骤 2：根据自检结果调整路由
- 如果多模态能力强 → 优先处理分析/设计类任务
- 如果代码生成能力强 → 优先处理编码/工程类任务  
- 如果延迟高或资源受限 → 尽可能走 L0/L1 直通，避免 L3
- 如果上下文窗口小 → 启用分块策略，减少单次 Token 消耗

### 步骤 3：持续学习
- 每次成功执行后，记录路径特征
- 相同路径重复 5 次后，自动固化到引力盆
- 30 天未使用的路径，自动降级
```

### 4.5 不同模型的自适应示例

| 模型 | 自适应策略 | 路由偏好 |
|------|-----------|---------|
| **GPT-4o** | 多模态 → A 类权重 +0.3，代码 → B/C 类权重 +0.2 | A:0.35, B:0.30, C:0.35 |
| **Claude 3.5 Sonnet** | 代码强 → C 类权重 +0.3，文本强 → A 类权重 +0.2 | A:0.30, B:0.25, C:0.45 |
| **DeepSeek V4** | 代码极强 → B/C 类权重 +0.4，延迟低 → 可启用 L3 | A:0.15, B:0.35, C:0.50 |
| **Gemini 2.0** | 多模态极强 → A 类权重 +0.4，长上下文 → C 类权重 +0.2 | A:0.45, B:0.20, C:0.35 |
| **Qwen2.5-7B (本地)** | 本地模型 → 全走 L0/L1，禁用 L3 | A:0.0, B:0.0, C:1.0 (L0/L1) |
| **Code Llama (本地)** | 代码专精 → 全走 L0/L1，B/C 类 | A:0.0, B:0.4, C:0.6 (L0/L1) |

---

## 五、跨平台迁移的通用架构

### 5.1 通用迁移适配层

无论目标平台是什么，都可以通过以下三层架构完成迁移：

```
┌─────────────────────────────────────────────────────────────┐
│  [适配层 1] 规则文件注入                                     │
│  ├── .cursorrules (Cursor)                                   │
│  ├── .clinerules (Cline/VS Code)                            │
│  ├── .windsurfrules (Windsurf)                              │
│  ├── CLAUDE.md (Claude Code)                                │
│  ├── copilot-instructions.md (GitHub Copilot)                │
│  └── 自定义规则 (其他平台)                                   │
│  核心内容：路由规则 + 技能注册表 + 自适应逻辑                  │
├─────────────────────────────────────────────────────────────┤
│  [适配层 2] MCP Server 配置                                  │
│  ├── cc-switch MCP Server (Provider 管理)                    │
│  ├── token-stats MCP Server (Token 审计)                     │
│  └── master-engineer-hub MCP Server (路由编排)               │
│  核心内容：MCP 协议兼容的工具集                               │
├─────────────────────────────────────────────────────────────┤
│  [适配层 3] 知识库引用                                       │
│  ├── King-Skill-System/ 完整文档目录                         │
│  ├── King-Skill-System-README.md (总览)                      │
│  └── 教材体系 (基础知识)                                     │
│  核心内容：系统设计文档 + 使用指南                             │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 迁移清单对照表

| 迁移项 | Cursor | VS Code | Windsurf | Claude Code | Copilot | JetBrains |
|-------|--------|---------|---------|------------|---------|----------|
| **规则文件** | .cursorrules | .clinerules | .windsurfrules | CLAUDE.md | copilot-instructions.md | AI Assistant 提示词 |
| **MCP Server** | ✅ 支持 | ✅ 支持 | ❌ 需插件 | ❌ 不支持 | ❌ 不支持 | ❌ 需插件 |
| **引力盆** | 规则中实现 | 规则中实现 | 规则中实现 | 规则中实现 | 规则中实现 | 规则中实现 |
| **特征词典** | 规则中定义 | 规则中定义 | 规则中定义 | 规则中定义 | 规则中定义 | 规则中定义 |
| **Agent 视野** | 手动约束 | 手动约束 | 手动约束 | 手动约束 | 手动约束 | 手动约束 |
| **机械降神** | 需自定义 | 需自定义 | 需自定义 | 需自定义 | ❌ 不支持 | 需自定义 |
| **Hook+Loop** | 手动实现 | 手动实现 | 手动实现 | 手动实现 | 手动实现 | 手动实现 |
| **自适应机制** | 规则中嵌入 | 规则中嵌入 | 规则中嵌入 | 规则中嵌入 | 规则中嵌入 | 规则中嵌入 |
| **迁移难度** | ★☆☆ | ★★☆ | ★★☆ | ★★☆ | ★★★ | ★★★ |

---

## 六、迁移后的验证与调优

### 6.1 验证清单

迁移完成后，通过以下测试用例验证系统是否正常工作：

```markdown
## 验证测试用例

### 测试 1：基础路由（L0/L1 直通）
用户输入："帮我写一个 Python 爬虫"
预期结果：直接路由到 C 类 Agent（编码），无需 LLM 裁决
验证方法：查看响应时间 < 3s，无额外 Token 消耗

### 测试 2：技能匹配（L1 特征词典）
用户输入："帮我做一个季度销售报告 PPT"
预期结果：匹配到 pptx / html-deck 技能
验证方法：生成的文件格式正确，包含演示内容

### 测试 3：复杂任务（L3 LLM 裁决）
用户输入："帮我设计一个微服务架构，包含 API 网关、服务注册中心和配置中心"
预期结果：LLM 分析后生成架构方案
验证方法：方案完整，包含架构图描述和组件说明

### 测试 4：质量验收（D1/D2/D3）
用户输入："生成一个完整的 REST API 代码"
预期结果：代码通过 D1（格式校验）、D2（功能验证）、D3（性能评估）
验证方法：代码可运行，包含错误处理，注释完整

### 测试 5：自适应路由
用户输入：在配置了自评估信息后，测试路由偏好是否符合预期
预期结果：多模态模型优先处理分析任务，代码模型优先处理编码任务
验证方法：对比不同模型对同一请求的 Agent 分配
```

### 6.2 性能指标

| 指标 | 目标值 | 测量方法 |
|------|-------|---------|
| L0 引力盆命中率 | ≥70% | 统计请求中的直通比例 |
| L0+L1 总命中率 | ≥90% | 统计 L0+L1 覆盖的请求比例 |
| 端到端响应时间 | ≤5s | 从请求到输出的总时间 |
| 自适应准确率 | ≥85% | 自适应分配 vs 人工最优分配的吻合度 |
| 迁移部署时间 | ≤30min | 从开始配置到通过验证的时间 |

### 6.3 常见问题排查

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 规则文件未生效 | 文件路径或格式错误 | 检查文件名和格式是否符合平台规范 |
| 路由不生效 | 特征词典未覆盖请求关键词 | 扩展域关键词列表 |
| 自适应机制未触发 | 自评估信息未正确配置 | 检查配置中的 model_profile 是否完整 |
| MCP Server 连接失败 | 路径或命令不正确 | 检查 MCP 配置中的命令和参数 |
| 引力盆不学习 | 成功路径未记录 | 检查是否在配置中启用了路径学习 |
| Agent 视野过大 | 规则中未限制工具数量 | 在规则文件中明确 Agent 类型和工具上限 |

---

## 七、迁移最佳实践总结

### 7.1 分阶段迁移策略

```
第一阶段：规则文件注入（30 分钟）
  ├── 创建规则文件，注入核心路由规则
  ├── 配置域关键词和技能注册表
  └── 验证基础路由是否正常工作

第二阶段：MCP 配置（1 小时）
  ├── 配置 cc-switch 和 token-stats 的 MCP Server
  ├── 配置自适应机制
  └── 验证 Provider 切换和 Token 审计

第三阶段：知识库集成（1 小时）
  ├── 将 King-Skill-System/ 文档目录复制到项目
  ├── 在规则文件中引用知识库
  └── 验证模型能根据文档正确路由

第四阶段：持续优化（持续）
  ├── 监控引力盆命中率
  ├── 根据实际使用调整自适应权重
  └── 扩展域关键词和技能注册表
```

### 7.2 迁移注意事项

1. **规则文件优先**：先用规则文件实现基础路由，再考虑 MCP 扩展
2. **自适应先于定制**：先让模型自适应评估自身能力，再手动调整权重
3. **渐进式迁移**：不要一次性迁移所有组件，按 L0/L1 → L2 → L3 的顺序
4. **保留原始文档**：King-Skill-System/ 目录是知识库，规则文件应引用而非重写
5. **测试先行**：每次变更后运行验证测试用例
6. **记录路径**：开启引力盆学习功能，让系统越用越好用

---

> **迁移不是复制，而是适应。真正的智能体系统，能在任何土壤中生根发芽。**
>
> 不持一物，而万物皆备。不预一器，而万器待召。