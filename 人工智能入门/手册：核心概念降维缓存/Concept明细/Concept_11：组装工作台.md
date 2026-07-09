# Concept_11：组装工作台

## 数学/逻辑本质
**原创智能体设计（System Prompt + Skill + 路由 + MCP 四层架构）**

## 降维直觉
不是买成品工具箱，是自己在工作台上用标准零件组装专属工具。

## 展开说明
原创智能体 = 你在 VS Code / Claude Code / Trae 等工具中**自己设计**的 AI 助手，而非调用厂商预设的"成品助手"。

组装工作台有四个抽屉：
1. **System Prompt（宪法）**：定死行为边界——"我只做代码审查，不做代码生成"
2. **Skill（部门法）**：场景化规则——"检查到 SQL 注入风险时，标记为 CRITICAL"
3. **路由（司法）**：根据输入特征选 Skill——"是 Python 代码？走安全检查 Skill"
4. **MCP（对外接口）**：标准化工具协议——"通过 MCP 调用静态分析工具"

本质是**将大模型的能力"约束"到特定工具域中**，就像在工作台上把通用零件组装成专用工具——零件还是那些零件（LLM 能力），但组合方式和约束条件决定了最终工具的用途。

## 教材对应章节
→ [[教材：The Automated Mind/Phase 3：模型生态与Agent实践/Vol AG-07：跨AI编程工具原创智能体设计与System Prompt路由架构|Vol AG-07：跨AI编程工具原创智能体设计与System Prompt路由架构]]
→ [[教材：The Automated Mind/Phase 3：模型生态与Agent实践/Vol AG-03：工程项目构建Agent|AG-03 §Hermes Hook+Loop]]
→ [[教材：The Automated Mind/Phase 3：模型生态与Agent实践/Vol 08：Agent部署|Vol 08 §ReAct框架]]