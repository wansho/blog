---
layout: post
title: "我的工具箱"
category: daily
---

ninehills 那篇 [Toolbox](https://github.com/ninehills/blog/wiki/Toolbox) 的价值，不在于列了多少软件，而在于它用一份工具清单，准确映射了作者的工作流。

我也整理了一下自己当前这台 Mac 的工具箱。这里不追求完整罗列，只保留真正高频、能代表我当前技术工作流的部分。

## 1. 开发主轴：Agent + IDE

我现在的开发环境是双层结构：

* Agent 层：`Codex`、`Cherry Studio`、`CC Switch`、`TokenUsage`、`Ollama`
* IDE 层：`VS Code`、`IntelliJ IDEA`、`PyCharm`、`WebStorm`、`DataGrip`

Agent 层负责模型入口、配置切换、上下文协作和本地模型补充；IDE 层负责代码编辑、调试、重构和语言服务。两者不是替代关系，而是分工关系。

这个变化很明显。过去的默认入口是 IDE，现在的默认入口更接近“先决定用哪个 Agent，再决定在哪个编辑器里落地”。

## 2. 本地环境仍然是核心

虽然 AI 工具占比越来越高，但我的工作流并没有转向纯云端。本地环境依然是核心执行面。

终端和系统侧的常用工具包括：

* 终端：`Ghostty`、`iTerm`
* 启动与系统增强：`Raycast`、`iStat Menus`、`Hidden Bar`
* 输入与交互优化：`Mos`、`Input Source Pro`

命令行工具则更能说明问题：

* 基础开发：`git`、`go`、`deno`、`pnpm`、`maven`
* 数据与服务：`mysql`、`redis`
* 运行时管理：`pyenv`、多版本 `python`、`jenv`、`nvm`、`rbenv`
* 终端工作流：`tmux`、`fish`、`btop`
* 内容处理：`ffmpeg`、`pandoc`、`tesseract`
* 安全与网络：`trivy`、`wget`、`websocat`

这套配置说明我对本地可控性要求比较高。很多任务仍然优先在本机完成，而不是依赖 SaaS 或远端环境。

## 3. 知识管理单独成链路

除了开发链路，我还单独保留了一条信息处理链路：

* 阅读：`NetNewsWire`、`Skim`
* 收藏：`Cubox`
* 笔记：`Obsidian`
* 写作：`Typora`

这里的设计原则很简单：输入、筛选、沉淀、输出分开处理。不同工具负责不同阶段，避免单个软件承担全部职责。

## 4. 这套工具箱的几个特征

从结果上看，我现在的工作流有几个比较稳定的特点：

* 第一，开发入口已经从单一 IDE 变成 Agent + IDE 协作。
* 第二，本地工具链仍然是基础设施，不是备用方案。
* 第三，环境是多语言、多运行时的，重点是兼容性和可切换性。
* 第四，知识管理独立于开发环境，形成单独闭环。

如果要用一句话概括，这套工具箱不是为了追求软件数量，而是为了把常见任务拆成几条稳定、低摩擦的处理链路。
