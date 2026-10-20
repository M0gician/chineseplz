# chineseplz

chineseplz 是一个 Agent Skill（名称 write-plain-chinese，显示名「说人话」），用于撰写、改写和审校中文技术回答、说明、实验报告、研究总结及表格，使表达标准、直接、中性、可核验。

优先使用正向陈述，直接说明事实、结论、理由、建议和行动。改写保留原文的事实、数字、单位、范围、否定关系、要求强度和不确定性，结论以已有资料和证据为依据。

## 写作规则

Skill 包含十三条规则：

1. 尽可能使用正向陈述句。
2. 准确保留原意和证据强度。
3. 使用准确、一致、可检索的术语。
4. 用具体、中性的书面表达描述对象。
5. 明确主体、对象和指代。
6. 直接使用表达动作的动词。
7. 一句承载一个主要信息。
8. 先交代必要条件，再说明动作或结论。
9. 清楚表达数量、比较和变化。
10. 按执行顺序组织操作说明。
11. 每段围绕一个主题，按理解顺序展开。
12. 表头、分类名和状态标签保持中性、一致。
13. 完整回答当前问题，信息充分时结束。

正向陈述规则用于精简防御性铺垫、自我辩护、无关的排除说明、假设性的边界讨论和重复确认。术语按专业语境和已知定义选择，句长按理解难度调整。

每条规则的详细说明和基础改写示例见 [SKILL.md](SKILL.md)。[原有术语与改写示例](references/original-examples.md)保留原版全部示例；[术语解释与新增示例](references/terminology-examples.md)补充“物化”“绿”“止血”“读作”“台账”“对账”“门控”等表达的含义、语境和改写方式。

## 安装

将本仓库中的 `SKILL.md`、`agents/` 和 `references/` 复制到所用框架的用户级 skills 目录，目录名保持 `write-plain-chinese`。各框架的用户级 skills 目录如下：

| 框架 | 用户级 skills 目录 |
| --- | --- |
| Claude Code | `~/.claude/skills/` |
| Codex | `~/.codex/skills/`（官方文档同时列出 `~/.agents/skills/`） |
| Kimi Code | `~/.kimi-code/skills/` |
| ZCode | `~/.zcode/skills/` |
| WorkBuddy | `~/.workbuddy/skills/` |

以 Kimi Code 为例：

```bash
git clone https://github.com/M0gician/chineseplz.git
mkdir -p ~/.kimi-code/skills/write-plain-chinese
cp -R chineseplz/SKILL.md chineseplz/agents chineseplz/references ~/.kimi-code/skills/write-plain-chinese/
```

使用其他框架时，将命令中的目标路径替换为表中对应的目录。

## 使用

在对话中要求“说人话”、撰写、润色或审校中文技术内容时，Agent 会加载该 Skill。也可以通过命令直接调用，调用方式因框架而异：

```
/write-plain-chinese <待改写文本>   # Claude Code、Kimi Code
$write-plain-chinese <待改写文本>   # Codex、ZCode
```

WorkBuddy 通过“设置 → 技能”或 SkillHub 导入文件夹后，在对话中调用。

## 文件结构

```
├── SKILL.md            # Skill 定义：执行流程、十三条规则、改写示例、交付前检查
├── references/
│   ├── original-examples.md     # 原有术语表和八条规则的全部示例
│   └── terminology-examples.md  # 新增术语解释、语境和改写示例
└── agents/
    └── openai.yaml     # 界面元数据：显示名、简介、默认提示词
```

## License

本项目使用 [MIT](LICENSE) 许可证。
