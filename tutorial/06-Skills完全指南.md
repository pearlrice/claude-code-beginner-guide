# 第六章：Skills 完全指南

> **本章目标**：理解 Skill 是什么，学会使用内置 Skill，创建自己的 Skill。

---

## 6.1 Skill 是什么？

### 用一个生活类比

想象你是医院里的一位主治医生。你有一个"工具箱"系统：

![[Gemini_Generated_Image_a7ybmha7ybmha7yb.png]]

**Skill 就是给 Claude Code 装的"专业工具箱"**。每个 Skill 包含：
1. **名称**：叫什么（变成 `/skill-name` 命令）
2. **描述**：干什么用的（Claude 据此判断什么时候用它）
3. **指令**：具体怎么做（分步骤的操作指南）

---

## 6.2 Skill 的四种形态

```
┌────────────────────────────────────────────────────────────┐
│                  Skill 的四种形态                           │
│                                                            │
│  ┌────────────────┐  ┌────────────────┐                   │
│  │ 内置命令         │  │ 内置 Skill      │                   │
│  │ (Built-in)      │  │ (Bundled)      │                   │
│  ├────────────────┤  ├────────────────┤                   │
│  │ 固定逻辑，代码实现│  │ Prompt驱动，     │                   │
│  │ /help /compact  │  │ 可被学习参考     │                   │
│  │ /clear /exit    │  │ /simplify       │                   │
│  │                 │  │ /debug /batch   │                   │
│  └────────────────┘  └────────────────┘                   │
│                                                            │
│  ┌────────────────┐  ┌────────────────┐                   │
│  │ 自定义 Skill     │  │ 插件 Skill      │                   │
│  │ (Custom)        │  │ (Plugin)       │                   │
│  ├────────────────┤  ├────────────────┤                   │
│  │ 你自己写的 SKILL.md│ │ 从插件市场安装   │                   │
│  │ /my-skill       │  │ plugin:skill   │                   │
│  └────────────────┘  └────────────────┘                   │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## 6.3 常用内置 Skill 速查

在 Claude Code 中输入 `/` 就能看到所有可用命令。以下是最常用的：

| Skill/命令 | 作用 | 什么时候用 |
|-----------|------|-----------|
| `/help` | 显示帮助信息 | 忘了命令怎么用时 |
| `/compact` | 压缩上下文，释放记忆空间 | 对话太长，AI 开始"忘事"时 |
| `/clear` | 清空当前对话 | 想从头开始一个新话题 |
| `/init` | 为当前项目生成 CLAUDE.md | 新项目，想快速建立项目说明 |
| `/simplify` | 审查代码，修复质量问题 | "帮我看看这段代码有没有问题" |
| `/review` | 审查 PR（代码合并请求） | 提交代码前让别人（AI）帮你看看 |
| `/debug` | 系统性调试问题 | 遇到 Bug，不知道怎么排查 |
| `/model` | 切换 AI 模型 | 想换更聪明或更快的模型 |
| `/permissions` | 调整权限设置 | 想让 Claude 更多或更少自动操作 |
| `/status` | 查看当前会话状态 | 看看用了多少 Token、什么模型 |
| `/cost` | 查看本次会话费用 | 想知道花了多少钱 |
| `/loop` | 定时重复执行 | "每 5 分钟检查一次部署状态" |

---

## 6.4 创建你的第一个自定义 Skill

### 实例一：标准化 Commit 的 Skill

每次提交代码都要写规范的 commit message，很烦。写个 Skill 自动搞定。

**步骤 1**：创建目录结构

```powershell
mkdir -p ~/.claude/skills/commit
```

> `~` 在 PowerShell/Unix 中代表你的用户主目录（`C:\Users\你的用户名`）。  
> `~/.claude/skills/` 下的 Skill **对所有项目都有效**。

**步骤 2**：创建 `SKILL.md`

创建文件 `C:\Users\你的用户名\.claude\skills\commit\SKILL.md`：

```markdown
---
name: commit
description: Stage and commit current changes with a proper commit message. Use when user asks to commit, save, or submit changes.
disable-model-invocation: true
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *) Bash(git diff *)
---

# Commit Skill

Create a well-formatted commit for the current changes.

## Steps

1. Run `git status` to see what's changed
2. Run `git diff --staged` to see staged changes, and `git diff` for unstaged ones
3. Write a concise commit message in this format:
   - Type prefix: feat/fix/refactor/docs/chore
   - Present-tense, under 72 characters
   - Body explains WHY, not WHAT
4. Stage the relevant files with `git add`
5. Commit with the message

## Commit Message Format


<type>: <short description>

<optional body explaining why>

Co-Authored-By: Claude Code <noreply@anthropic.com>


Types: feat (new feature), fix (bug fix), refactor (code improvement), docs (documentation), chore (maintenance)
```

**步骤 3**：测试 Skill

```powershell
# 在项目中做了些改动后
/commit
```

Claude Code 会：
1. 查看改动内容
2. 按你规定的格式写好 commit message
3. 自动提交

> 注意 `disable-model-invocation: true` 的作用：这个 Skill 只能你手动调用（`/commit`），Claude 不会自己决定"该提交了"。这是对的——提交代码应该是你的决定。

---

### 实例二：代码审查 Skill

```powershell
mkdir -p ~/.claude/skills/code-review
```

创建 `~/.claude/skills/code-review/SKILL.md`：

```markdown
---
name: code-review
description: Review code for bugs, security issues, and code quality problems
context: fork
agent: Explore
---

# Code Review

Review the provided code changes for the following issues:

## Checklist

1. **Bugs**: Logic errors, off-by-one, null/undefined handling
2. **Security**: Injection risks, exposed secrets, auth bypasses
3. **Performance**: N+1 queries, unnecessary loops, memory leaks
4. **Readability**: Unclear names, missing guard clauses, deep nesting
5. **Architecture**: SRP violations, tight coupling, circular dependencies

## Output Format

For each issue found, include:
- **Severity**: 🔴 Critical / 🟡 Warning / 🔵 Suggestion
- **Location**: File path and line number
- **Issue**: What's wrong
- **Fix**: How to fix it
- **Why**: Why it matters

Start with a summary, then list issues by severity.
```

> `context: fork` 让这个 Skill 在独立的子代理中运行，不影响主对话的上下文。

---

### 实例三：项目特定 Skill（放项目里）

如果你想一个 Skill 只在某个项目里用，放在项目的 `.claude/skills/` 下：

```powershell
# 在项目根目录下
mkdir -p .claude/skills/deploy
```

创建 `.claude/skills/deploy/SKILL.md`：

```markdown
---
name: deploy
description: Deploy this project to production
disable-model-invocation: true
---

# Deploy this project

Run these steps in order:

1. `npm test` — all tests must pass
2. `npm run build` — build the production bundle
3. `npm run deploy` — push to production

If any step fails, stop and report the error. Do not proceed to the next step.
```

> 放在 `.claude/skills/` 下，可以跟项目一起放到 Git 仓库，团队成员都能用。

---

## 6.5 Skill 存放位置与作用域

```
┌────────────────────────────────────────────────────────────┐
│              Skill 的四个级别（优先级从高到低）               │
│                                                            │
│  企业级  ~/.claude/skills/ (由服务器推送)                    │
│  ↑ 覆盖下面的同名 Skill                                     │
│  │                                                         │
│  用户级  ~/.claude/skills/                                 │
│  ↑ 你对所有项目都有效的 Skill                                │
│  │                                                         │
│  项目级  项目/.claude/skills/                               │
│  ↑ 只对当前项目生效，团队共享                                 │
│  │                                                         │
│  插件级  (从插件加载的 Skill)                                │
│  ↑ 用 plugin-name:skill-name 命名空间，不冲突               │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## 6.6 Skill 高级功能

### 传参数给 Skill

你调用 Skill 时可以传参数：

```
/fix-issue 123
```

在 SKILL.md 中用 `$ARGUMENTS` 或 `$0`, `$1` 引用：

```markdown
---
name: fix-issue
description: Fix a GitHub issue by number
---

Fix GitHub issue $ARGUMENTS. Read the issue, understand the problem, implement the fix, add tests.
```

`/fix-issue 123` → Claude 收到 "Fix GitHub issue 123. Read the issue..."

### 命名参数

```yaml
---
name: migrate
description: Migrate a component
arguments: [component, from_framework, to_framework]
---

Migrate $component from $from_framework to $to_framework.
```

调用：`/migrate SearchBar React Vue`

### 动态注入上下文（高级）

Skill 里可以包含 shell 命令，**在 Skill 内容发送给 Claude 之前先执行**，把输出插进去：

```markdown
## 当前环境信息
- Node 版本：!`node --version`
- Git 状态：
```!
git status --short
```
```

`!` 开头的行和 ` ```! ` 代码块里的命令会提前执行，Claude 看到的已经是结果了。

### 在子代理中运行 Skill

加 `context: fork` 让 Skill 在独立子代理中执行：

```yaml
---
name: deep-research
description: Research a topic in the codebase
context: fork
agent: Explore
---
```

**适合**：代码探索、代码审查等只读任务——不影响主对话上下文。

---

## 6.7 Skill 生命周期

```
 Skill 被调用
 ↓
 SKILL.md 内容加载到对话中
 ↓
 Claude 按照 Skill 指令执行
 ↓
 Skill 内容保持在对话中（整个会话）
 ↓
 如果对话被压缩（compact），Skill 的前 5000 tokens 被保留
 ↓
 如果压缩后 Skill 看起来"失效"了，重新调用 /skill-name 即可
```

---

## 本章小结

| 你学到了什么 | 要点 |
|-------------|------|
| Skill 是什么 | 给 Claude Code 装的专业工具箱，包含名称、描述、指令 |
| 内置 Skill | `/simplify`、`/debug`、`/review`、`/init` 等 |
| 创建自定义 Skill | 建目录 → 写 SKILL.md → `/skill-name` 调用 |
| 三个存放级别 | 用户级（`~/.claude/skills/`）→ 项目级（`.claude/skills/`）→ 企业级 |
| 高级功能 | 传参（`$ARGUMENTS`）、动态注入（`!` 命令）、子代理（`context: fork`） |

> 📌 **下一章**：[第七章：实用能力教程](./07-实用能力教程.md)  
> 真实场景实操：整理文件、设计绘图、创建 PPT、用 Claude Code 写教程。
