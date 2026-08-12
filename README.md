# chineseplz

chineseplz 是一个 Agent Skill（名称 write-plain-chinese，显示名「说人话」），用于将中文技术文本、实验报告、研究总结和表格改写为标准、直接、中性、可核验的表述。

改写只调整表达方式，原文的事实、数字、范围、确认程度和不确定性保持不变，不补充原文没有的证据、数字、结论或因果解释。

## 改写规则

Skill 包含八条规则：

1. 使用标准术语，不另造表述。
2. 删除需要读者推断指代对象的比喻，直接说明对象、变化和关系。
3. 表头和分类名使用中性名词，全篇使用同一组章节状态标签。
4. 不使用“是 X，不是 Y”的对比句式，直接陈述检查结果、差异来源或变量关系。
5. 允许条目不包含数字或结论，不为形式统一补充内容。
6. 原因缺少验证时写“原因未查明”，不追加未经验证的解释。
7. 不使用口语词，使用准确、可核验的书面表达。
8. 不使用拟人表述，不把意图、动作或属性赋予模型、指标等无生命对象。

每条规则的详细说明和改写示例见 [SKILL.md](SKILL.md)。

## 安装

将本仓库中的 `SKILL.md` 和 `agents/` 复制到所用框架的用户级 skills 目录，目录名保持 `write-plain-chinese`。各框架的用户级 skills 目录如下：

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
cp -R chineseplz/SKILL.md chineseplz/agents ~/.kimi-code/skills/write-plain-chinese/
```

使用其他框架时，将命令中的目标路径替换为表中对应的目录。

## 使用

在对话中要求“说人话”、润色或审校中文技术内容时，Agent 会加载该 Skill。也可以通过命令直接调用，调用方式因框架而异：

```
/write-plain-chinese <待改写文本>   # Claude Code、Kimi Code
$write-plain-chinese <待改写文本>   # Codex、ZCode
```

WorkBuddy 通过“设置 → 技能”或 SkillHub 导入文件夹后，在对话中调用。

## 文件结构

```
├── SKILL.md            # Skill 定义：执行流程、八条规则、交付前检查
└── agents/
    └── openai.yaml     # 界面元数据：显示名、简介、默认提示词
```

## License

本项目使用 [MIT](LICENSE) 许可证。
