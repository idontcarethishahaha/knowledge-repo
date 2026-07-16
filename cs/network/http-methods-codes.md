---
title: "http请求方法状态码"
tags:
  - 八股
  - 计算机网络
  - 计算机基础
  - 前端
created: 2026-07-17
summary: "http请求方法状态码"
---

GET 拿，POST 交，PUT 全改，PATCH 局部改，DELETE 删，HEAD 只拿头
幂等：GET、PUT、DELETE、HEAD
非幂等：POST、PATCH

1xx 继续
2xx 成功
3xx 跳转缓存（301 永久、302 临时、304 缓存）
4xx 前端错
5xx 服务器崩
最易混对比
401 vs 403
401：没登录
403：登录了，但没权限
502 vs 504
502：后端服务挂了
504：代理连后端超时
301 vs 302
301 永久重定向（缓存）
302 临时重定向（不缓存）