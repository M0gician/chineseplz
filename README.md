# chineseplz

chineseplz 是一个 Agent Skill（名称 write-plain-chinese，显示名「说人话」），用于中文交流和中文输出，覆盖问答、代码说明、调试与审查、进度汇报、文档、总结、表格和改写。

直接说明实际情况，精简防御性表达，尽可能使用正向陈述句。保留事实、数字、单位、范围、否定关系、要求强度和不确定性，结论以已有资料和证据为依据。

## 写作规则

Skill 包含十四条规则：

1. 直接说明实际情况，精简防御性表达。
2. 尽可能使用正向陈述句。
3. 准确保留原意和证据强度。
4. 使用准确、一致、可检索的术语。
5. 用具体、中性的书面表达描述对象。
6. 明确主体、对象和指代。
7. 直接使用表达动作的动词。
8. 一句承载一个主要信息。
9. 先交代必要条件，再说明动作或结论。
10. 清楚表达数量、比较和变化。
11. 按执行顺序组织操作说明。
12. 每段围绕一个主题，按理解顺序展开。
13. 表头、分类名和状态标签保持中性、一致。
14. 完整回答当前问题，信息充分时结束。

第一条独立要求：将否定对照改为对实际情况的直接说明，删去防御性铺垫、自我辩护、对未提出问题的排除说明、假设性的边界讨论和重复确认。

每条规则的详细说明和基础改写示例见 [SKILL.md](SKILL.md)。[原有术语与改写示例](references/original-examples.md)保留原版全部示例；[术语解释与新增示例](references/terminology-examples.md)补充“物化”“绿”“止血”“读作”“台账”“对账”“门控”等表达的含义、语境和改写方式。

## 安装

将本仓库中的 `SKILL.md`、`agents/` 和 `references/` 复制到所用框架的用户级 skills 目录，目录名保持 `write-plain-chinese`。各框架的用户级 skills 目录如下：

| 框架 | 用户级 skills 目录 |
| --- | --- |
| Claude Code | `~/.claude/skills/` |
| Codex | `~/.agents/skills/`，兼容 `~/.codex/skills/` |
| Kimi Code | `~/.kimi-code/skills/` 或 `~/.agents/skills/` |
| Pi coding agent | `~/.pi/agent/skills/` 或 `~/.agents/skills/` |
| ZCode | `~/.zcode/skills/` |
| WorkBuddy | `~/.workbuddy/skills/` |

以 Kimi Code 为例：

```bash
git clone https://github.com/M0gician/chineseplz.git
mkdir -p ~/.kimi-code/skills/write-plain-chinese
cp -R chineseplz/SKILL.md chineseplz/agents chineseplz/references ~/.kimi-code/skills/write-plain-chinese/
```

使用其他框架时，将命令中的目标路径替换为表中对应的目录。Codex、Kimi Code 和 Pi 跨工具共用时，可以统一安装到 `~/.agents/skills/write-plain-chinese/`。

安装或更新后，在新会话中检查技能列表。Pi 的现有会话可以用 `/reload` 刷新。

## 使用

### 自动调用

Agent 根据技能描述选择是否加载正文。本 Skill 将“中文交流或需要中文输出”放在描述开头，覆盖普通任务中的过程说明和最终结果。任务按用户原目标完成，中文表达遵循写作规则。

共享 `description` 保持在 250 个字符以内，适应 Kimi 的描述截断；Codex 的 `short_description` 同样前置中文适用条件。默认提示词覆盖当前任务的中文输出。

### 显式调用

保持完整技能名称，名称与任务之间使用半角空格。Kimi 和 Pi 的 `/skill:` 命令放在消息开头：

```
$write-plain-chinese 解释这段代码          # Codex、ZCode
/skill:write-plain-chinese 解释这段代码    # Kimi Code、Pi
/write-plain-chinese 解释这段代码          # Claude Code
```

WorkBuddy 通过“设置 → 技能”或 SkillHub 导入文件夹后，在对话中调用。

### 持续用于中文任务

在所用框架的全局指令文件中加入以下内容，让每次中文任务都有明确的调用要求：

> 中文交流或需要中文输出时，主动使用 write-plain-chinese，并读取其 SKILL.md。用该技能组织当前任务的中文过程说明和最终结果，后续中文回答继续沿用这些规则。

| 框架 | 默认全局指令文件 |
| --- | --- |
| Codex | `~/.codex/AGENTS.md` |
| Kimi Code | `~/.kimi-code/AGENTS.md` |
| Pi coding agent | `~/.pi/agent/AGENTS.md` |
| Claude Code | `~/.claude/CLAUDE.md` |

指令文件随会话加载，读取技能由模型执行。需要程序直接注入正文时，可使用 Codex 的 `SessionStart` hook、Kimi 插件的 `sessionStart.skill` 或 Pi 扩展的 `before_agent_start`。

机制与配置参见 [Codex Skills](https://learn.chatgpt.com/docs/build-skills)、[Codex Hooks](https://learn.chatgpt.com/docs/hooks)、[Kimi Skills](https://moonshotai.github.io/kimi-code/en/customization/skills.html)、[Kimi Plugins](https://moonshotai.github.io/kimi-code/en/customization/plugins.html) 和 [Pi Skills](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/skills.md)。

## 文件结构

```
├── SKILL.md            # Skill 定义：执行流程、十四条规则、改写示例、交付前检查
├── references/
│   ├── original-examples.md     # 原有术语表和八条规则的全部示例
│   └── terminology-examples.md  # 新增术语解释、语境和改写示例
└── agents/
    └── openai.yaml     # 界面元数据：显示名、简介、默认提示词
```

## License

本项目使用 [MIT](LICENSE) 许可证。
