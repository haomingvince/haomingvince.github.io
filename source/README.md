# [haomingzhang.com](https://haomingzhang.com)

基于 [Hexo](https://hexo.io/) 的个人技术博客，托管于 GitHub Pages。

## 项目结构

```
haomingvince.github.io/
├── .gitignore              # source 分支：排除 node_modules/ public/ .deploy_git/
├── _config.yml             # Hexo 站点配置
├── _config.next.yml        # NexT 主题配置 (Muse 风格)
├── package.json            # npm 依赖
├── scaffolds/              # 文章模板
├── source/                 # 内容源文件
│   ├── CNAME               # 自定义域名
│   ├── .gitignore          # 主分支忽略规则
│   ├── README.md           # 本文件
│   ├── _posts/             # Markdown 文章
│   ├── tags/               # 标签页
│   ├── categories/         # 分类页
│   └── guestbook/          # 留言板
└── public/                 # hexo generate 生成目录 (git 忽略)
```

## 分支职责

| 分支 | 内容 | 用途 |
|------|------|------|
| `source` | Hexo 源码（Markdown、配置、主题） | **日常工作分支**：写文章、改配置、安装插件 |
| `master` | 纯静态文件（HTML/CSS/JS） | **仅部署写入**：由 `hexo deploy` 自动推送，GitHub Pages 从此分支 serve |

## 技术栈

| 组件 | 版本 |
|------|------|
| Hexo | 8.1.1 |
| NexT 主题 | 8.27.0 (Muse) |
| Node.js | >= 20.19.0 |
| 评论系统 | Valine（需手动配置） |
| 搜索 | 本地搜索 (hexo-generator-searchdb) |
| 站点地图 | Google + Baidu |
| 部署 | hexo-deployer-git |

## 日常工作流

```bash
# 写新文章
hexo new post "文章标题"
vim source/_posts/文章标题.md

# 本地预览
hexo server
# 浏览器打开 http://localhost:4000

# 发布到线上
hexo clean && hexo deploy

# 备份源码
git add -A && git commit -m "描述修改内容"
git push origin source
```

## 更新 Hexo 及插件

```bash
# 升级 hexo-cli
npm update -g hexo-cli

# 查看可更新的包
npm outdated

# 升级依赖
npm update --save

# 升级 NexT 主题
npm update hexo-theme-next --save

# 重新生成验证
hexo clean && hexo generate
```

## 迁移历史

| 迁移项 | 旧版本 | 新版本 |
|--------|--------|--------|
| Hexo | 5.1.1 | 8.1.1 |
| NexT | 8.0.0 | 8.27.0 |
| 分支策略 | 单分支 (master) | 双分支 (source + master) |

## 注意事项

- **不要直接修改 `master` 分支**：所有内容变更都在 `source` 分支完成，通过 `hexo deploy` 推送到 `master`。
- **`source/` 目录下的文件会被复制到站点根目录**：CNAME、.gitignore、README.md 等放在这里才会随部署一起发布。
- **图片通过 jsDelivr CDN 外链托管**：图床仓库为 `haomingvince/ghcdn`。
- **Valine 在新版 NexT 中已移除内置支持**：如需使用需手动注入 JS 脚本，或考虑切换到 Gitalk（NexT 原生支持）。
