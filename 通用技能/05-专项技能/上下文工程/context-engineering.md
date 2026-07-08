# 中文版 AI Skill 使用说明

> **Skill 文件**：SKILL.md · SKILL.original.md

这是古法编程整理的中文版 AI Skill 下载包。

这个包里可能包含：

- `SKILL.md`：默认入口文件，Agent 通常识别这个文件名。
- `*.original.md`：原始英文文件。想用英文原版时，可以把对应的 `.original.md` 文件改回标准文件名。
- `REFERENCE.md`、`EXAMPLES.md`：参考资料或示例，可能是中文翻译，也可能保留原版。
- `scripts/`：原始脚本目录，通常不要翻译文件名或脚本内容。

使用方式：

1. 目录型 skill：把整个 skill 目录放到目标工具支持的 skills 目录，确保入口文件名是 `SKILL.md`。
2. 项目规则：也可以把 `SKILL.md` 内容改写进 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。
3. 英文原版：如果你英文阅读没问题，或担心中文指令增加模型 token 消耗，可以改用 `.original.md` 里的原版内容。

具体来源见 `SKILL.md` 文件顶部。

更多 AI 技能文章：https://studynil.com/ai/skills/
