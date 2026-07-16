---
title: "中后台UI一致性"
tags:
  - 前端
  - html
  - 跨域
  - CSS
  - 业务
created: 2026-07-16
summary: "工作台基座工程通过iframe注入实现跨域子页面UI一致性"
---

相关笔记:
  - [iframe 详解：使用、安全与最佳实践](../../../frontend/html-css/iframe.md)

---
## 设计方案

核心：基座通过iframe向子工程注入统一CSS，利用选择器优先级覆盖 ant-desigh-vue默认样式。子工程 **零代码改动，零发版**

---
### 一、背景与目标

背景：基座工程通过iframe嵌入 4+ 个子工程、100+ 个子页面。各工程独立开发维护，视觉风格差异显著，用户体验割裂

目标：尽量不改造子工程代码的前提下，实现基座内所有iframe子工程的视觉统一

核心约束：
  - 子工程技术以 Vue 3 + ant-desigh-vue 4.x 为主
  - 子工程数量多、维护团队分散，逐一发版改造成本高
  - 部分子工程与基座**跨域部署**，受浏览器限制

---
### 二、方案选型

| 方案 | 核心机制 | 子工程侵入性 | 维护成本 |
|------|----------|-------------|---------|
| **CSS 注入**（✅ 采用） | iframe插入link标签 | ✅ 零侵入 | 中 |
| **ConfigProvider** | 子工程配置 antd token | 每个子工程改代码发版 | 低|
| **CSS变量+postMessage** | 基座下发变量，子工程消费 | 每个子工程改代码发版 | 中 |
| **UI组件库统一** | 子工程引入基座组件包 | 每个子工程改代码发版 | 高 |

**选型决策：**
  - 成本低
  - 落地速度快，基座发一版即可全局生效
  - 后续维护极优，只需维护 2 个csn文件
  - 回滚能力强，只需要回滚cdn文件

---
### 三、核心原理

#### CSS 覆盖

antd 4.x 使用 CSS-in-JS 动态生成样式，组件名被 :where(.hash) 包裹，优先级被刻意降低

```css
:where(.css-abc123).ant-btn-primary {
   background-color: #1677ff;
   border-color: #1677ff
}
```

解决方案：在选择器前加 html 标签
```css
html .ant-btn-primary {
   background-color: #1677ff;
   border-color: #1677ff
}
```

但是后来实际发现 antd 选择器层级过多，可能三个或四个层级

所以改为用 `html:not(#_)` 提高优先级
```css
  html:not(#_) .ant-btn-primary {
   background-color: #1677ff;
   border-color: #1677ff
}
```

#### 注入逻辑

同源：DOM直接注入  *(一开始踩坑以为同域就可以)*
> `iframe.contentDocument.head.appendChild(link)` 子工程完全无感

<br/>

跨域：URL参数透传
> iframe src 附加 `?_dj_css=CDN地址`，子工程通过 bridge.js 加载 **（子工程只需要通过一行 script 引入bridge.js）**

---
### 四、实现

代码没有，只能口述记录了

同源场景
> 基座通过`iframe.contentDocument`直接操作子工程DOM，在head中插入link标签加载css

<br/>

跨域场景
> 基座在iframe src后附加`_dj_css`参数、子工程通过bridge.js加载css
> 注入函数需要兼容原有的iframe url逻辑
> 收到 DJ_READY 消息后，postMessage补发主题

<br/>

安全校验
> 为防止恶意 CSS 注入，基座需要对 CSS 地址进行白名单校验

<br/>

bridge.js
> - 子工程通过一行 script 引入
> - 内置url检测，防止重复加载
> - 发送 DJ_READY 消息给基座(支持实时主题切换，基座收到后补发当前主题状态)

---
### 五、CSS文件编写

设计同学提供了对应的UI和交互规范文件，无具体类名、规范极多。最后结果共近千行css

直接通过 claude 帮我完成，样式不准确，自习添加或缺少等缺点

最后配置figma mcp，通过 figma-mpx(滴滴开源的Figma 设计稿自动转 Mpx 跨端代码)完成，效果很好，第一次有80%样式达到预期效果