# Knowledge Repository

## 📝 概述

面向全栈工程师的知识体系，覆盖前端、后端、计算机基础、工程实践与开发工具等。

🌐 **在线站点**：[dieatmore.github.io/knowledge-repo](https://dieatmore.github.io/knowledge-repo/) — 基于 VitePress 构建，支持全文搜索。

## 🚀 本地运行

```bash
npm install            # 安装依赖
npm run docs:dev       # 启动开发服务器
npm run docs:build     # 构建静态站点
npm run docs:preview   # 预览构建结果
```

推送 `main` 分支后，GitHub Actions 自动部署到 GitHub Pages。


---

## 目录结构

```
knowledge-repo/
│
├── asset/                                🖼️ 静态资源：架构图 / 流程图 / 示意图
│
├── backend/                              ⚙️ 后端
│   └── database/                         🗄️ 数据存储
│         └── sql/                          MySQL / PostgreSQL：索引 / 事务 / 锁 / SQL 优化
│         └── redis/     
├── cs/                                   📖 计算机基础
│   ├── algorithms/                       算法：排序 / 搜索 / 动态规划 / 贪心 / 回溯
│   ├── data-structures/                  数据结构：数组 / 链表 / 树 / 图 / 哈希表 / 堆
│   ├── network/                          计算机网络：TCP/IP / HTTP/HTTPS / DNS / 抓包
│   └── os/                               操作系统：进程线程 / 内存管理 / 文件系统 / IO 多路复用
│
├── engineering/                          🛠️ 软件工程（跨语言跨端的通用实践）
│   └── security/                         Web 安全：XSS / CSRF / SQL 注入 / HTTPS&TLS
│
├── frontend/                             🖥️ 前端
│   ├── browser/                          浏览器原理：渲染流程 / Event Loop / 缓存 / 跨域 / 安全
│   ├── frameworks/                       🧩 前端框架
│   │   └── vue/                          Vue / Composition API / Pinia / Nuxt / 生态
│   ├── html-css/                         HTML5 / CSS3 / Flex&Grid 布局 / 响应式 / 动画
│   └── javascript/                       ES6+ / 异步编程 / 模块化 / TypeScript
│
├── real-world/                           🌍 实战记录
│   └── real-business/                    真实业务：业务逻辑 / 边界 case / 踩坑记录
│       ├── didi/                         滴滴相关业务
│       └── nefu-data-center/             林科楼数据中心
│
├── share/                                🔗 好文分享
│
├── templates/                            📝 笔记模板
│
├── tools/                                🔨 开发工具
│   └── git/                              Git：常用命令 / 分支策略 / rebase vs merge
│
└── README.md
```

---

## 笔记规范

> 推荐 **Foam（VS Code 扩展）**，原生支持 frontmatter 标签面板，标签名支持中文等

每篇 `.md` 文件以 YAML frontmatter 开头，只需 4 个字段：

```yaml
---
title: "文章标题"
tags:
  - 标签1
  - 标签2
created: xxxx-xx-xx
summary: "一句话摘要"
---
```

| 字段 | 说明 |
|------|------|
| `title` | 文章标题 |
| `tags` | 打标，用于搜索聚合 |
| `created` | 更新日期 |
| `summary` | 简洁摘要 |

---
## 技术栈

本站基于 **VitePress** 构建，GitHub Actions 自动部署至 GitHub Pages。

YAML frontmatter + 标准 Markdown 链接，可无缝迁移至：

- **Docusaurus** — 功能全面
- **Obsidian** — 本地笔记
- **Hugo** — 静态站点

---

## ✅ update

**2026-07-21**

- 新增 `redis/` 目录并更新：redis相关笔记
- 更新：MySQL事务、ACID、隔离级别、MVCC 面试笔记

**2026-07-19**

- 搭建 VitePress 静态站点：自动侧边栏、本地搜索、首页导航
- 配置 GitHub Actions 自动部署至 GitHub Pages
- 清理空目录：删除 26 个仅有 `.gitkeep` 的空目录，精简仓库结构

**2026-07-17**

- 新增 `share/` 目录：优质技术文章链接收藏
- 迁移原知识库：CSS(21)、JS(39)、Vue(2)、、网络(3)、补充知识点(8)、基础(7) 共 87 篇笔记

**2026-07-16**

- iframe 详解：使用、安全与最佳实践
- 更新：MySQL 索引原理、失效场景与面试高频题
- 业务整理
  - 中后台UI一致性
  - KOP 办公网域名适配
  - 林科楼文档迁移

**2026-07-15**

- 初始化仓库
- 创建飞书群聊机器人——知识库更新提醒
- 更新Git 常用命令、分支策略
