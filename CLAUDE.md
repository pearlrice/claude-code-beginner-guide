# Claude Code 新手全方位教程 — 项目文档

## 项目目标
面向编程小白（纯新手），从零开始学习使用 Claude Code 的全方位中文教程。
参考 Notion 笔记风格，章节清晰，每步配有图示说明。

## 教程大纲

### 第一章：认识终端和命令行
- 终端是什么（类比：给电脑发文字短信的窗口）
- Windows Terminal vs Tabby Terminal
- 如何打开终端、如何在当前文件夹打开终端
- 基本命令：cd, ls/dir, mkdir, pwd
- 什么是工作区（项目目录）

### 第二章：Claude Code 是什么 — 概念对比
- Claude Code 能做什么
- 命令行 vs 桌面程序 vs IDE 插件 vs 网页版
- 为什么推荐命令行版（账户风控/验证问题）
- ChatGPT 网页版 vs Claude Code：本质区别
- "用 Codex App 和用 Claude Code 有什么区别？"

### 第三章：安装 Claude Code
- 前置条件：Git for Windows、Node.js
- Windows 安装步骤（PowerShell / CMD）
- 验证安装成功
- 常见安装问题排查

### 第四章：第一次运行和基本对话
- 认证登录（Anthropic Console API Key / 第三方 API）
- 启动第一个对话
- 基本交互：提问、让 Claude 读文件、写代码
- 权限模式选择（Plan / Auto / Accept Edits / YOLO）

### 第五章：核心概念和名词解释
- Agent Loop（智能体循环）
- Context Window（上下文窗口）
- Tools（工具：Read, Write, Edit, Bash, Glob, Grep）
- Permissions（权限系统）
- CLAUDE.md（项目记忆文件）
- Hooks（自动化钩子）
- MCP（模型上下文协议）
- API 调用 vs 订阅制 vs PAYG 计费

### 第六章：Skills 完全指南
- Skill 是什么（类比：给 AI 装的专业工具箱）
- 内置 Skill（bundled skills）
- 创建自定义 Skill
- Skill 的作用域（个人/项目/企业）
- 实际案例

### 第七章：实用能力教程
- 整理文件和组织代码
- 设计绘图（ASCII 图、流程图）
- 创建 PPT/演示文稿
- 用 Claude Code 写教程文档
- 常见工作流（修 Bug、写测试、提交代码、创建 PR）

### 第八章：第三方模型接入与计费
- API Key 是什么
- 为什么有 API 调用的计费模式
- Claude Pro/Max 订阅 vs API PAYG 的区别
- 如何配置第三方 API（OpenAI、DeepSeek 等兼容接口）
- 成本管理和使用建议

## 文件结构
```
claude_tut/
├── CLAUDE.md                    # 项目计划（本文件）
├── tutorial/
│   ├── 00-目录.md              # 总目录
│   ├── 01-认识终端和命令行.md
│   ├── 02-Claude-Code是什么.md
│   ├── 03-安装Claude-Code.md
│   ├── 04-第一次运行.md
│   ├── 05-核心概念.md
│   ├── 06-Skills完全指南.md
│   ├── 07-实用能力教程.md
│   └── 08-第三方模型和计费.md
└── images/                      # 截图和图表
```

## 写作风格
- 面向纯新手，用生活化类比解释技术概念
- 每步骤配有说明（截图/ASCII图表/示意图）
- 关键术语用 **粗体** 标注，附带括号解释
- 代码块标注语言类型
- 使用引用块标注"注意"和"提示"
- 每章末尾有"本章小结"

## 参考来源
- Claude Code 官方文档: https://code.claude.com/docs/en/overview
- 官方文档完整索引: https://code.claude.com/docs/llms.txt
