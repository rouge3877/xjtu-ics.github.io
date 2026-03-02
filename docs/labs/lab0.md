# Lab0: Environment Setup

## 实验简介

> 工欲善其事，必先利其器

Lab0的目的是帮助大家熟悉环境并掌握一些基础工具的使用；我们也鼓励大家更多的进行一些课外的动手实践，并为大家整理/提供了一些额外的学习资源，有兴趣可自行学习/动手实践，强大动手能力会是大家学习CS路上的左膀右臂。

本文内容总共包含以下几个部分：

- Linux环境准备
- 使用VSCode进行开发
- 一些其他有用的学习资源

## Linux环境准备

本课程的***所有实验都必须在Linux环境下进行***，本小结简单介绍如何搭建并在Linux环境下进行开发。

### ICS-Server

考虑到大部分同学第一次接触Linux，在第一次搭建环境的过程中可能会遇到很多困难，ICS课程团队为每位同学准备了校内的Linux服务器环境，并称这个环境为ICS-Server。

ICS-Server是该课程所有实验的标准环境，后续的具体实验任务也会直接分发到各个同学的服务器目录中。因此，不论你选择使用ICS Server还是搭建本地Linux环境进行后续实验，都建议首先熟悉ICS Server的使用。

ICS-Server的具体登录和使用方法，详见[ICS-Server登录与使用](../resources/ICS-Server.md)

### 本地Linux环境构建方法简介

本课程的实验在稳定的Linux环境下均可以顺利完成，考虑到课程结束后ICS Server账号将回收，并且Linux也是后续大家在学习/深造中重要开发平台与学习工具，我们非常鼓励大家自行搭建属于自己的Linux环境。

本地Linux环境构建方法详见[本地Linux构建文档](../resources/LinuxInstallation.md)

## VSCode配置

Visual Studio Code（简称 VSCode）是一款由微软开发且跨平台的免费源代码编辑器。

VSCode强大之处在于其“远程”开发能力，这里的远程包括：

- 登录到远程的服务器上进行开发，真正地work remotely
- 登录到本地的虚拟机/docker环境开发

此外，到目前为止，VSCode对于一些AI工具的插件支持也非常不错。

当然，我们也推荐大家采用自己习惯的工具进行开发。

有关VSCode远程开发的配置与使用详见[VSCode远程开发](../resources/VScodeRemote-SSH.md)

## 实用工具学习

本小节提供一些优质的学习资源，以便同学们对Linux开发环境的快速上手。

开发工具的学习不必像学习其他理论课程一样教条，关键在于多上手，多查阅，多探索，用到什么学什么，而不是全部学完再上手，**切勿死记硬背**。

### AI工具使用

已经6202年了，各类AI模型以及各种AI Agent已经可以构建强大的自动化workflow，极大的提升自己的工作效率，这里推荐一些好用的AI工具：

- Reading/Slides/Image: [NotebookLM](notebooklm.google)，[NanoBanana](https://gemini.google.com)
- Coding: [Codex CLI](https://developers.openai.com/codex/cli/), [Claude Code](https://code.claude.com/docs/en/overview), [Antigravity](https://antigravity.google/)
- Noting: [Obsidian](https://obsidian.md/)/[Notion](https://www.notion.com/) + AI
- Others: [Gemini CLI](https://geminicli.com/), [OpenCode](https://opencode.ai/), [OpenClaw](https://openclaw.ai/)...

有关这些工具的配置和使用方法可以参考[]，或者其他网络上的优质博客等，这里不过多解释。

### Linux使用入门

考虑到绝大多数同学可能并没有Linux使用经验，这里简要为大家准备Linux入门相关资源，请大家结合自身情况进行学习：

- [Linux Journey Command Line](https://labex.io/lesson/the-shell)：把命令分成“入门”、“进阶”和“专家”级，每个知识点后都有小测验。

- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)：一个通过命令通关的游戏，任务是找出隐藏在系统里的下一关密码，对于熟悉常见命令很有帮助。

- [The Linux Command Line for Beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)：Ubuntu 官方教程，适合使用ubuntu的同学

- [ArchWiki](https://wiki.archlinux.org/title/Main_page)：ArchLinux的有关文档，有些内容十分硬核

### MIT 6.NULL

这是MIT为计算机本科生开的一门课《你计算机科学教育中遗失的一学期》，主要用于系统的教学一些基本的工具使用，教学非常全面，大家可以作为简单入门Linux之后完整的计算机工具课。

B站中有对应的公开课[视频资源](https://www.bilibili.com/video/BV14E411J7n2/?spm_id_from=333.337.search-card.all.click&vd_source=f03c62aca712083bc8b849d1d0e7cdb5)。

这门课可以学到很多有用的工具，但是由于这门课相对比较深入，更加建议以它作为工具类课程的进阶部分。

### 其他的一些coding tools

- vim/neovim/emacs等：可以用于修改小文件，如常见的配置文件等，基于终端使用非常方便
- zed/antigravity等：更好的集成了一些AI工具，可以加速开发效率等
- git/tmux等：一些实用的终端工具

网络上可以查找到非常多的教程，这里就不再赘述，有兴趣的同学可以自行搜索。

## 评分方法与代码提交

本实验为环境搭建与工具熟悉实验，并不占最后实验分值计算，也无需提交任何形式的文件代码。

但是本实验是后续所有实验的前置基础，请同学们务必详细阅读并准备好基于Linux的开发环境。
