# Knowledge Repository

## 📝 概述

面向全栈工程师的知识体系，覆盖前端、后端、AI、计算机基础、工程实践与开发工具。


---

## 目录结构

```
knowledge-repo/
│
├── ai/                                   🤖 人工智能
│   ├── deep-learning/                    深度学习：CNN / RNN / Transformer / 训练技巧
│   ├── llm/                              大语言模型：RAG / Agent / Fine-tune / 推理优化
│   ├── ml-basics/                        经典机器学习：回归 / 分类 / 聚类 / 特征工程
│   ├── mlops/                            模型部署 / 向量数据库 / ML 流水线
│   └── prompt-engineering/               提示词工程：Prompt 设计 / CoT / 结构化输出
│
├── backend/                              ⚙️ 后端
│   ├── api-design/                       RESTful / GraphQL / gRPC / WebSocket
│   ├── auth/                             JWT / OAuth 2.0 / SSO 单点登录 / RBAC 权限模型
│   ├── database/                         🗄️ 数据存储
│   │   ├── nosql/                        Redis 缓存策略 / MongoDB / Elasticsearch
│   │   └── sql/                          MySQL / PostgreSQL：索引 / 事务 / 锁 / SQL 优化
│   ├── languages/                        🗣️ 语言 & 框架
│   │   ├── go/                           Go / Gin / goroutine & channel 并发模型
│   │   ├── nodejs/                       Node.js / Express / Nest.js / Koa
│   │   └── python/                       Python / Django / FastAPI / Flask / asyncio
│   └── message-queue/                    RabbitMQ / Kafka / 异步解耦 / 事件驱动
│
├── cs/                                   📖 计算机基础
│   ├── algorithms/                       算法：排序 / 搜索 / 动态规划 / 贪心 / 回溯
│   ├── computer-organization/            组成原理：CPU / 内存层次 / 指令集 / 存储
│   ├── data-structures/                  数据结构：数组 / 链表 / 树 / 图 / 哈希表 / 堆
│   ├── network/                          计算机网络：TCP/IP / HTTP/HTTPS / DNS / 抓包
│   └── os/                               操作系统：进程线程 / 内存管理 / 文件系统 / IO 多路复用
│
├── engineering/                          🛠️ 软件工程（跨语言跨端的通用实践）
│   ├── design-patterns/                  设计模式 / SOLID 原则 / 常用模式实战
│   ├── devops/                           Docker / K8s / CI&CD / GitHub Actions
│   ├── observability/                    日志 / 监控（Prometheus）/ 链路追踪
│   ├── security/                         Web 安全：XSS / CSRF / SQL 注入 / HTTPS&TLS
│   ├── system-design/                    系统设计：微服务 / 高可用 / 高并发 / CAP&BASE
│   └── testing/                          单元测试 / 集成测试 / E2E / 压力测试
│
├── frontend/                             🖥️ 前端
│   ├── browser/                          浏览器原理：渲染流程 / Event Loop / 缓存 / 跨域 / 安全
│   ├── build-tools/                      webpack / Vite / Rollup / Babel / esbuild
│   ├── engineering/                      前端工程化：ESLint / 监控 / 测试 / 微前端
│   ├── frameworks/                       🧩 前端框架
│   │   ├── react/                        React / Hooks / 状态管理 / Next.js / 生态
│   │   └── vue/                          Vue / Composition API / Pinia / Nuxt / 生态
│   ├── html-css/                         HTML5 / CSS3 / Flex&Grid 布局 / 响应式 / 动画
│   └── javascript/                       ES6+ / 异步编程 / 模块化 / TypeScript
│
├── real-world/                           🌍 实战记录
│   ├── case-studies/                     项目复盘：从 0 到 1 / 架构演进 / 技术选型
│   └── real-business/                    真实业务：业务逻辑 / 边界 case / 踩坑记录
│
├── share/                                 🔗 好文分享
│   └── link.md                            优质技术文章 / 博客链接收藏
│
├── templates/                            📝 笔记模板
│
├── tools/                                🔨 开发工具
│   ├── editor/                           编辑器：Vim / Neovim / VS Code 配置
│   ├── git/                              Git：常用命令 / 分支策略 / rebase vs merge
│   └── linux/                            Linux：Shell 脚本 / 常用命令 / 权限管理
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

## 迁移兼容性

YAML frontmatter + 标准 Markdown 链接，可无缝迁移至：

- **VitePress** — 推荐，轻量快速
- **Docusaurus** — 功能全面
- **Obsidian** — 本地笔记
- **Hugo** — 静态站点

---

## ✅ update

**2026-07-17**

- 新增 `share/` 目录：优质技术文章链接收藏
- 迁移原知识库：CSS(21)、JS(39)、Vue(2)、、网络(3)、补充知识点(8)、基础(7) 共 87 篇笔记  ——早期整理的，有蛮多不好的地方，可以更新补充

**2026-07-16**

- iframe 详解：使用、安全与最佳实践
- 更新：MySQL 索引原理、失效场景与面试高频题
- 业务整理
  - 中后台UI一致性
  - KOP 办公网域名适配(待更新)
  - 林科楼文档迁移

**2026-07-15**

- 初始化仓库
- 创建飞书群聊机器人——知识库更新提醒
- 更新Git 常用命令、分支策略
