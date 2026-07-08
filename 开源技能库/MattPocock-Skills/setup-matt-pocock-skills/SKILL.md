# Setup Matt Pocock's Skills（设置 Matt Pocock 技能）

来源：Matt Pocock Skills 原版，英文入口保留在 `SKILL.original.md`。

为这些工程技能预设每个仓库需要的配置：

- **Issue tracker**：issue 放在哪里。默认是 GitHub，也原生支持本地 Markdown。
- **Triage labels**：五个 canonical triage role 使用的字符串。
- **Domain docs**：`CONTEXT.md` 和 ADR 放在哪里，以及读取它们的消费规则。

这是 prompt 驱动的技能，不是确定性脚本。先探索，展示你发现的内容，和用户确认，然后再写入。

## 流程

### 1. 探索

查看当前仓库，理解起始状态。读已经存在的东西，不要假设：

- `git remote -v` 和 `.git/config`：这是 GitHub 仓库吗？是哪一个？
- 根目录的 `AGENTS.md` 和 `CLAUDE.md`：是否存在？其中是否已经有 `## Agent skills` 区块？
- 根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`。
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录。
- `docs/agents/`：这个技能之前是否已经输出过内容？
- `.scratch/`：这可能表示仓库已经在使用本地 Markdown issue tracker 约定。

### 2. 展示发现并提问

总结已存在和缺失的东西。然后带用户逐个完成三个决策。**一次只展示一个 section**，拿到用户回答后再进入下一个。不要一次把三个都丢出来。

假设用户不知道这些术语是什么意思。每个 section 都要以短解释开头：它是什么、为什么这些技能需要它、选择不同方案会改变什么。然后展示选项和默认值。

**Section A — Issue tracker。**

说明：Issue tracker 是这个仓库放 issue 的地方。`to-issues`、`triage`、`to-prd`、`qa` 等技能会读写它，所以它们需要知道应该调用 `gh issue create`、在 `.scratch/` 下写 Markdown 文件，还是遵循你描述的其他流程。选择你实际用来跟踪工作的地方。

默认姿态：这些技能是按 GitHub 设计的。如果 `git remote` 指向 GitHub，就建议 GitHub。如果 `git remote` 指向 GitLab，包括 `gitlab.com` 或自托管 GitLab，就建议 GitLab。否则，或者用户偏好别的方式，提供：

- **GitHub**：issue 放在仓库的 GitHub Issues 中，使用 `gh` CLI。
- **GitLab**：issue 放在仓库的 GitLab Issues 中，使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI。
- **Local markdown**：issue 作为文件放在这个仓库的 `.scratch/<feature>/` 下，适合个人项目或没有远程仓库的项目。
- **Other**（Jira、Linear 等）：请用户用一段话描述工作流；技能会把它记录成自由文本。

**Section B — Triage label vocabulary。**

说明：`triage` 技能处理新 issue 时，会让它穿过一个状态机：需要评估、等待报告者补信息、已准备好给 AFK agent、已准备好给人类、不会处理。为了做到这一点，它需要应用和你实际配置一致的 label，或 issue tracker 中等价的东西。如果仓库已经使用不同 label 名，例如 `bug:triage` 而不是 `needs-triage`，就在这里映射，避免技能创建重复 label。

五个 canonical role：

- `needs-triage`：维护者需要评估。
- `needs-info`：等待报告者补充信息。
- `ready-for-agent`：规格已经完整，AFK-ready，Agent 可以在没有额外人工上下文的情况下接手。
- `ready-for-human`：需要人类实现。
- `wontfix`：不会处理。

默认值：每个 role 的字符串等于它自己的名字。询问用户是否要覆盖任何一个。如果 issue tracker 还没有现有 label，默认值就可以。

**Section C — Domain docs。**

说明：一些技能，例如 `improve-codebase-architecture`、`diagnose`、`tdd`，会读取 `CONTEXT.md` 来学习项目的领域语言，并读取 `docs/adr/` 来了解过去的架构决策。它们需要知道仓库是一个全局 context，还是多个 context，例如 monorepo 中前后端各自有 context，这样才能看对地方。

确认布局：

- **Single-context**：根目录一个 `CONTEXT.md` + `docs/adr/`。大多数仓库是这种。
- **Multi-context**：根目录有 `CONTEXT-MAP.md`，指向各 context 的 `CONTEXT.md`，通常出现在 monorepo。

### 3. 确认并编辑

展示草稿给用户看：

- 要添加到 `CLAUDE.md` / `AGENTS.md` 的 `## Agent skills` 区块。选择规则见步骤 4。
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容。

写入前让用户修改。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则，如果 `AGENTS.md` 存在，编辑它。
- 如果两个都不存在，问用户要创建哪一个，不要替用户选。

当 `CLAUDE.md` 已经存在时，不要创建 `AGENTS.md`，反过来也一样。始终编辑已经存在的那个。

如果目标文件中已经有 `## Agent skills` 区块，就原地更新内容，而不是追加重复区块。不要覆盖周围的用户编辑。

区块：

```markdown
## Agent skills

### Issue tracker

[一句话总结 issue 跟踪位置]。见 `docs/agents/issue-tracker.md`。

### Triage labels

[一句话总结 label 词汇]。见 `docs/agents/triage-labels.md`。

### Domain docs

[一句话总结布局，例如 single-context 或 multi-context]。见 `docs/agents/domain.md`。

```


然后用这个技能目录中的 seed template 作为起点，写入三个 docs 文件：

- [issue-tracker-github.md](./issue-tracker-github.md)：GitHub issue tracker。
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md)：GitLab issue tracker。
- [issue-tracker-local.md](./issue-tracker-local.md)：本地 Markdown issue tracker。
- [triage-labels.md](./triage-labels.md)：label mapping。
- [domain.md](./domain.md)：domain doc 消费规则和布局。

对于 “other” issue tracker，基于用户描述从零写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户 setup 已完成，并说明哪些工程技能现在会读取这些文件。提醒他们之后可以直接编辑 `docs/agents/*.md`；只有想切换 issue tracker 或从头重置时，才需要重新运行这个技能。
