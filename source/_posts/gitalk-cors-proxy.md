---
title: Gitalk 国内加速：用 Cloudflare Worker 自建 OAuth 代理
date: 2026-05-06 10:00:00
categories: 技术
tags:
  - Hexo
  - Gitalk
---

## 问题

博客迁移到 Gitalk 后设置一切正常，但国内用户打开文章页面时评论区域完全空白。浏览器 F12 Console 显示 Gitalk 依赖的 OAuth 代理 `cors-anywhere.azm.workers.dev` 请求 403。

Gitalk 基于 GitHub Issues，但 GitHub OAuth 不允许纯前端完成 token 交换，必须有一个服务端代理转发 `client_id` 和 `client_secret`。社区常用的公共代理 `cors-anywhere` 已被严重限流，国内更是基本不可用。

<!-- more -->

## 方案

用 Cloudflare Worker 自建一个 OAuth 代理，部署在 Cloudflare 全球边缘节点，国内用户也能正常访问。

## 创建 Worker

打开 [Cloudflare 控制台](https://dash.cloudflare.com) → Workers 和 Pages → 创建 Worker，命名后粘贴代码：

```javascript
export default {
  async fetch(request) {
    if (request.method === 'OPTIONS') {
      return new Response(null, {
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Methods': 'POST, OPTIONS',
          'Access-Control-Allow-Headers': 'Content-Type',
        }
      });
    }

    const url = new URL(request.url);
    const targetUrl = url.pathname.slice(1);

    if (!targetUrl.startsWith('https://github.com/login/oauth/access_token')) {
      return new Response(null, { status: 400 });
    }

    const body = await request.text();
    const params = new URLSearchParams(body);
    params.set('client_id', '你的GitHub OAuth Client ID');
    params.set('client_secret', '你的GitHub OAuth Client Secret');

    const resp = await fetch(targetUrl, {
      method: 'POST',
      headers: { 'Accept': 'application/json' },
      body: params.toString()
    });

    return new Response(await resp.text(), {
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json'
      }
    });
  }
};
```

## 配置 NexT

Worker 部署后会得到一个 `xxx.workers.dev` 域名，更新 `_config.next.yml` 中 Gitalk 的 proxy 配置：

```yaml
gitalk:
  proxy: https://你的worker域名/https://github.com/login/oauth/access_token
```

重新部署博客即可。

## 原理

```
用户浏览器 → Gitalk 前端
    ↓ 点击登录
GitHub OAuth 授权 → 拿到 code
    ↓ 需要服务端转发 code + client_secret
Cloudflare Worker → GitHub API → 返回 access_token
    ↓
Gitalk 用 token 拉取/创建 Issue → 评论加载完成
```

相比 `cors-anywhere` 公共代理，自建 Worker 的优势：
- 专属实例，不会被限流
- Cloudflare 全球边缘网络，国内访问正常
- 免费额度每天 10 万次请求，个人博客绰绰有余
- `client_secret` 只在 Worker 服务端使用，浏览器不可见
