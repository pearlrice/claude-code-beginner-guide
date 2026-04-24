# 第三章：安装 Claude Code

> **本章目标**：在 Windows 上一步步安装 Claude Code 命令行版，验证安装成功。

---

## 3.1 安装前的准备

Claude Code 需要两个前置工具。如果已经装过，可以跳到 3.2。

### 检查你装过没有

打开 PowerShell，输入以下命令：

```powershell
# 检查 Git
git --version

# 检查 Node.js
node --version
```

如果输出类似 `git version 2.47.0` 和 `v22.11.0`，说明已装好，直接跳到 3.2。

如果显示"不是内部或外部命令"，说明还没装。按下面的步骤装。

---

### 安装 Git for Windows

Git 是代码版本管理工具。Claude Code 依赖它来管理文件变更。

```
┌────────────────────────────────────────────┐
│          Git for Windows 安装步骤            │
│                                            │
│  1. 浏览器打开: https://git-scm.com        │
│     ┌──────────────────────────┐           │
│     │  [Download for Windows]  │ ← 点这里  │
│     └──────────────────────────┘           │
│                                            │
│  2. 下载完成后双击运行安装程序               │
│                                            │
│  3. 一路点 "Next"（保持默认选项即可）        │
│     ⚠️ 注意：安装过程全部默认，                │
│     不要改任何选项，尤其是编辑器选择那一步      │
│                                            │
│  4. 安装完毕后，关闭并重新打开 PowerShell      │
│     输入 git --version 验证                 │
│                                            │
└────────────────────────────────────────────┘
```

> 💡 安装包大约 60MB，下载可能需要几分钟。

---

### 安装 Node.js

Node.js 是 JavaScript 运行环境。Claude Code 是用 Node.js 写的，所以需要它。

```
┌────────────────────────────────────────────┐
│          Node.js 安装步骤                    │
│                                            │
│  1. 浏览器打开: https://nodejs.org          │
│     ┌──────────────────────────┐           │
│     │  [Download Node.js (LTS)]│ ← 点这里  │
│     └──────────────────────────┘           │
│     ⚠️ 注意：选 LTS 版本（长期支持版），       │
│     不要选 Current（最新实验版）              │
│                                            │
│  2. 下载完成后双击运行安装程序               │
│                                            │
│  3. 一路点 "Next"（保持默认选项即可）        │
│                                            │
│  4. 安装完毕后，关闭并重新打开 PowerShell     │
│     输入 node --version 验证                │
│                                            │
└────────────────────────────────────────────┘
```

---

## 3.2 安装 Claude Code

确认 Git 和 Node.js 都装好后，开始安装 Claude Code。

### Windows PowerShell 安装（推荐）

```powershell
irm https://claude.ai/install.ps1 | iex
```

**这条命令在干什么？** 用人话翻译一下：

| 命令部分                            | 含义                                  |
| ------------------------------- | ----------------------------------- |
| `irm`                           | 从网上下载文件（Invoke **R**est **M**ethod） |
| `https://claude.ai/install.ps1` | 下载这个安装脚本                            |
| `\|`                            | 把下载的内容"传"给下一步                       |
| `iex`                           | 执行传过来的脚本（**I**nvoke **Ex**pression） |

> 💡 这就是安装命令。整条命令的意思是："去 Claude 官网下载安装脚本，然后运行它。"

### 备用安装方式（CMD 命令提示符）

如果你在 CMD 而不是 PowerShell 中：

```cmd
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

> ⚠️ 怎么判断自己在哪个环境？看提示符：`PS C:\>` 开头 = PowerShell，`C:\>` 开头 = CMD。

---

### 安装过程的预期输出

```
PS C:\Users\chris> irm https://claude.ai/install.ps1 | iex

  Claude Code 安装中...
  ✓ 下载完成
  ✓ 安装完成
  ✓ 已添加到系统路径

  Claude Code 安装成功！
  版本: v1.0.xx

  使用方法: 在任意项目文件夹中运行 claude
```

> ⚠️ 如果 Windows 提示"无法识别"或"安全警告"：
> 1. 安全警告选"允许"即可
> 2. 如果遇到执行策略限制，运行这条命令后重试：
>    `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`

---

## 3.3 验证安装是否成功

关闭并重新打开 PowerShell（这很重要，不然可能找不到新装的命令），然后输入：

```powershell
claude --version
```

如果输出类似以下内容，安装成功：

```
Claude Code v1.0.xx
```

### 如果提示 "claude 不是内部或外部命令"

这说明系统没找到 Claude Code。按顺序排查：

```
排查步骤：

1. 确保关闭了所有 PowerShell 窗口，重新打开一个新的

2. 在新窗口中再次运行 claude --version

3. 如果还是不行，运行：
   claude --install-shell-integration

4. 还是不行？试试用完整路径：
   $env:USERPROFILE\.claude\bin\claude.cmd --version

5. 如果完整路径可以运行，说明路径没加到 PATH 里：
   手动添加 %USERPROFILE%\.claude\bin 到系统环境变量 PATH
```

---

## 3.4 更新 Claude Code

终端版 Claude Code **会自动在后台更新**，你一般不需要手动操作。

如果想手动检查更新：

```powershell
claude --update
```

如果用的是 WinGet 安装的版本（不会自动更新）：

```powershell
winget upgrade Anthropic.ClaudeCode
```

---

## 3.5 卸载方法

如果哪天想卸载：

```powershell
# 用安装时的方式卸载
claude --uninstall

# 或者如果是 WinGet 安装的
winget uninstall Anthropic.ClaudeCode
```

---

## 本章小结

| 步骤 | 操作 |
|------|------|
| 1. 装 Git | 去 git-scm.com 下载安装 |
| 2. 装 Node.js | 去 nodejs.org 下载 LTS 版 |
| 3. 装 Claude Code | PowerShell 运行 `irm https://claude.ai/install.ps1 \| iex` |
| 4. 验证 | `claude --version` 输出版本号即成功 |

> 📌 **下一章**：[第四章：第一次运行和基本对话](./04-第一次运行.md)  
> 安装好了，让我们启动 Claude Code，开始第一次对话。
