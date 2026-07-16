---
title: "iframe 详解：使用、安全与最佳实践"
tags:
  - html
  - security
  - 跨域
  - 前端
created: 2026-07-16
summary: "梳理 iframe 的核心属性、父子窗口通信（postMessage）、跨域限制、X-Frame-Options 安全策略及常见应用场景。"
---

# iframe 详解：使用、安全与最佳实践

## 一、iframe 基础

​<iframe>​​ 标签是 HTML5 中的一部分，用于嵌入另一个 HTML 页面到当前页面中

### 1. 核心属性

| 属性 | 说明 |
|------|------|
| `src` | 嵌入页面的 URL |
| `width` / `height` | 尺寸 |
| `sandbox` | 沙箱限制权限 |
| `allow` | 权限策略如 `allow="fullscreen; camera"` |
| `loading` | `lazy` / `eager` 延迟加载 |
| `referrerpolicy` | 控制 Referer 头的发送策略 |
| `srcdoc` | 直接内嵌 HTML 内容（替代 src） |

### 2. 基础示例

```html
<iframe
  src="https://example.com/widget"
  width="600"
  height="400"
  loading="lazy"
  sandbox="allow-scripts allow-same-origin"
  allow="fullscreen"
></iframe>
```

---

## 二、父子窗口通信（postMessage）

### 父窗口 → iframe

```js
// 父窗口发送消息
const iframe = document.getElementById('myFrame');
iframe.contentWindow.postMessage(
  { type: 'greeting', data: 'hello' },
  'https://target-origin.com'  // 指定目标源
);
```

### iframe → 父窗口

```js
// iframe 内部发送消息
window.parent.postMessage(
  { type: 'response', data: 'world' },
  'https://parent-origin.com'
);
```

### 接收消息

```js
// 双方都要验证来源
window.addEventListener('message', (event) => {
  // 务必校验 origin
  if (event.origin !== 'https://expected-origin.com') return;

  // 校验数据结构
  if (event.data?.type === 'greeting') {
    console.log(event.data.data);
  }
});
```

---

## 三、安全策略

### 1. 同源策略限制

不同源时，父页面与 iframe **禁止**互相访问 DOM、Cookie、localStorage 等。必须通过 `postMessage` 通信。

### 2. X-Frame-Options（服务端响应头）

**被嵌入的页面**的服务器返回这个响应头，告诉浏览器"我能不能被别人用 iframe 嵌入"。控制权在被嵌入方。

举个例子 —— 你是银行网站 `bank.com` 的开发者，不希望钓鱼网站 `evil.com` 用 iframe 嵌套你的登录页骗取用户密码。这时你在 `bank.com` 的服务端配置：

| 值 | 效果 | 例：evil.com 嵌入 bank.com | 例：bank.com 嵌入 bank.com |
|----|------|---------------------------|---------------------------|
| `DENY` | 任何人都不能嵌入我 | ❌ 阻止 | ❌ 阻止 |
| `SAMEORIGIN` | 只有同源页面可以嵌入我 | ❌ 阻止 | ✅ 允许 |

`ALLOW-FROM` 曾用于指定白名单域名，但主流浏览器已废弃——用下面的 CSP 替代。

**实际响应示例**（Nginx 配置）：

```nginx
# 完全禁止被嵌入
add_header X-Frame-Options "DENY";

# 只允许同源嵌入（常用，比如自己站内嵌套自己的页面）
add_header X-Frame-Options "SAMEORIGIN";
```

### 3. CSP: frame-ancestors（推荐替代 X-Frame-Options）

X-Frame-Options 只能设置 1 个规则（要么 DENY 要么 SAMEORIGIN），且 `ALLOW-FROM` 已被废弃。CSP 的 `frame-ancestors` 是它的替代方案，**支持指定多个白名单域名**。

```http
Content-Security-Policy: frame-ancestors 'self' https://trusted.com
```

逐段拆解这行：

```
Content-Security-Policy: frame-ancestors 'self' https://trusted.com
│                        │               │       │
│                        │               │       └── 也允许 trusted.com 嵌入我
│                        │               └── 允许我自己的网站嵌入我（同源）
│                        └── "谁能用 iframe 嵌套我？"
└── CSP 响应头
```

只有 `bank.com` 自己和 `trusted.com` 可以用 iframe 嵌入当前页面

**Nginx 配置示例**：

```nginx
# 只允许同源（最常见的配置）
add_header Content-Security-Policy "frame-ancestors 'self'";

# 允许同源 + 指定合作方
add_header Content-Security-Policy "frame-ancestors 'self' https://partner.com";
```

### 4. sandbox 属性

| 值 | 说明 |
|----|------|
| `allow-scripts` | 允许执行 JS |
| `allow-same-origin` | 同源策略不隔离（慎用） |
| `allow-forms` | 允许表单提交 |
| `allow-popups` | 允许弹窗 |
| `allow-top-navigation` | 允许修改顶层窗口 URL |

> ⚠️ `allow-scripts` + `allow-same-origin` 同时开启时，iframe 可以移除自身 sandbox 属性，等于没有沙箱。

### 5. 点击劫持防御（Clickjacking）

攻击者将目标网站通过透明 iframe 覆盖到恶意页面上，诱导用户点击。防御方式：服务端设置 `X-Frame-Options: DENY` 或使用 CSP `frame-ancestors`。

---

## 四、常见应用场景

| 场景 | 说明 |
|------|------|
| 第三方支付 | 支付宝/微信支付在 iframe 中完成 |
| 地图嵌入 | 内嵌 Google Maps、高德地图 |
| 视频播放 | 嵌入 YouTube / B站 播放器 |
| 广告系统 | 隔离广告 JS，防止影响宿主页面 |
| 微前端 | qiankun 等方案基于 iframe 或沙箱隔离子应用 |
| 在线代码预览 | CodeSandbox / StackBlitz 嵌入 |
| 跨系统集成 | 后台管理嵌入不同系统页面 |

### 单体 SPA vs 基座 + iframe 子页面

Vue/React 项目如果所有页面打成一个包，工程变大后每次构建都是全量编译，改一行代码也要重编整个项目，CI/CD 时间越来越长。

拆成**基座工程 + 多个子页面工程**，通过 iframe 嵌入，各子页面独立部署：

```
┌──────────────────────────────────┐
│  基座（主工程）                      │
│  ┌─────────┐  ┌─────────┐        │
│  │ 子页面 A  │  │ 子页面 B  │  ...   │
│  │ (iframe) │  │ (iframe) │        │
│  └─────────┘  └─────────┘        │
└──────────────────────────────────┘
```

| 维度 | 单体 SPA | 基座 + iframe |
|------|----------|--------------|
| 构建速度 | 全量编译，越改越慢 | 只构建有变化的子工程 |
| 部署粒度 | 一次上线所有页面 | 子页面独立上线，互不影响 |
| 技术栈 | 统一版本 | 各子工程可不同技术栈（Vue 2 / Vue 3 / React 混用） |
| 故障隔离 | 一个页面崩可能带崩全局 | 单个 iframe 挂了不影响其他页面 |
| 团队协作 | 多人改同工程，冲突多 | 各团队独立仓库，不打架 |
| 首屏加载 | 全量资源一次加载 | 按需加载当前子页面 |

**代价**：
- 页面间共享数据需要走 `postMessage` 或统一的状态管理方案，比单体 SPA 直接用 store 麻烦
- iframe 多开时浏览器内存、CPU 消耗更高
- 埋点、权限、全局 Loading 等公共逻辑需要在基座和子页面间约定好规范

**适合用 iframe 的场景**：后台管理系统中模块多、团队多、迭代频率不同的情况。比如主站 + 客服后台 + 数据看板 + CMS，四个工程各自独立迭代。

---

## 五、常见问题

### 1. iframe 阻塞主页面加载

iframe 的 `onload` 会阻塞父页面的 `onload` 事件。解决方案：
- 使用 `loading="lazy"` 延迟加载
- 动态创建 iframe，等主页面加载完毕再插入

### 2. 白屏或空白

检查：是否跨域、X-Frame-Options 是否禁止嵌入、src 是否返回了 404。

### 3. 双重滚动条

给 iframe 设置 `display: block`（去除行内元素的基线空隙），或用 `scrolling="no"` 去掉 iframe 自身滚动条。

---
