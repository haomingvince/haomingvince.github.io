---
title: Hexo 5 到 8 迁移记录：升级框架、更换评论、双分支工作流
date: 2026-05-05 21:00:00
categories: 技术
tags:
  - Hexo
  - 杂谈
---

## 背景

本博客最早搭建于 2020 年，基于 Hexo 5.1.1 + NexT 8.0.0。时隔近六年，Hexo 已发布 8.1.1，NexT 也迭代到了 8.27.0。原 Node.js 版本早已 EOL，Valine 评论系统在新版 NexT 中也已被移除。借此机会做一次完整的升级迁移。

<!-- more -->

## 迁移内容总览

| 组件 | 旧版本 | 新版本 |
|------|--------|--------|
| Hexo | 5.1.1 | 8.1.1 |
| hexo-cli | 旧版 | 4.3.2 |
| NexT 主题 | 8.0.0 | 8.27.0 |
| Node.js | 旧版 | 25.9.0 |
| 评论系统 | Valine | Gitalk |
| 分支策略 | 单分支 master | 双分支 source + master |

## 升级步骤

### 1. 环境准备

Hexo 8.x 要求 Node.js >= 20.19.0。使用 nvm 安装最新 LTS：

```bash
nvm install 22
nvm use 22
npm install -g hexo-cli@latest
```

### 2. 初始化新项目

```bash
hexo init blog-source
cd blog-source
npm install
```

### 3. 安装主题和插件

```bash
# NexT 主题
npm install hexo-theme-next@latest --save

# 插件
npm install hexo-deployer-git hexo-server \
  hexo-generator-sitemap hexo-generator-baidu-sitemap \
  hexo-generator-searchdb hexo-word-counter --save
```

### 4. 迁移文章

从旧站点的 HTML 和 `search.xml` 中提取内容，重建 7 篇 Markdown 文章。使用 `permalink` 字段保持旧 URL 路径不变（如 `/hexo_1/`、`/gta_boost/` 等）。

### 5. 配置 NexT 主题

从 `node_modules/hexo-theme-next/_config.yml` 复制配置模板，按旧站点风格调整：

- **Scheme**: Muse
- **菜单**: 首页 / 标签 / 分类 / 归档 / 留言板
- **侧边栏**: 右侧显示
- **头像**: 通过 jsDelivr CDN 加载
- **社交链接**: GitHub、LinkedIn
- **搜索**: 本地搜索（hexo-generator-searchdb）
- **备案**: 琼ICP备2022015317号-1

### 6. 切换到双分支工作流

旧的单分支模式是将 Hexo 源码和静态产出混在一起。新版采用 Hexo 官方推荐的双分支策略：

```
分支 source → Hexo 源码（Markdown、配置、主题）
分支 master  → 纯静态文件（hexo deploy 自动推送）
```

工作流：

```bash
# 写文章
hexo new post "标题"

# 本地预览
hexo server

# 发布
hexo clean && hexo deploy    # 推静态文件到 master

# 备份源码
git add -A && git commit -m "..."
git push origin source       # 推源码到 source
```

### 7. 更换评论系统

新版 NexT 8.27.0 不再内置 Valine。对比了支持的选项后选择了 **Gitalk**：

- 基于 GitHub Issues，无需第三方服务
- 配置简单，NexT 原生支持
- 读者用 GitHub 账号即可评论，天然防 spam

需要创建一个 GitHub OAuth App（Settings → Developer settings → OAuth Apps），回调地址填博客域名，拿到 Client ID 和 Secret 后填入 `_config.next.yml` 即可。

## 踩坑记录

**Node.js 版本不匹配**：Hexo 8.x 要求 Node.js >= 20.19.0，如果用旧版 Node 会直接报错。

**Valine 在新版 NexT 中消失**：受支持的评论系统只有 Disqus、DisqusJS、Livere、Gitalk、Utterances、Isso、Changyan，Valine 需要手动注入 JS。

**hexo deploy 强制推送 master**：如果 repo 是 private，GitHub Pages 不会生效，需要把仓库设为 Public。

**隐藏文件不随部署发布**：`.gitignore` 等隐藏文件需要加入 `_config.yml` 的 `include` 列表才会被复制到产出目录。

## 小结

整体迁移比较顺利，核心变化：
- Hexo 大版本升级后 API 兼容性良好，配置文件结构基本不变
- NexT 主题配置向后兼容，旧配置大部分可直接使用
- 双分支工作流让源码备份和部署解耦，更清晰
- Gitalk 免去了 Valine 对 LeanCloud 的依赖，评论数据自有

如果你也在做类似的迁移，欢迎在评论区交流。
