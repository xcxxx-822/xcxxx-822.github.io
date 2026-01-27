---
title: 第一篇技术博客：解决Hexo的部署与上传
date: 2026-01-19 23:19:18
tags: [Hexo]
---

# Hexo 博客搭建填坑实录：从 404 到成功上线

## 概览
- 环境：Windows 11 / Hexo 7.0 / Node.js / Git

- 主题：Hexo-theme-fluid

- 目标：将博客部署至 GitHub Pages (xcxxx-822.github.io)

- 流程：

1. 准备工作：下载Node.js（Hexo的运行核心） Git（把本地网页上传到github）

![git](/img/26.1.20/git.png)

2. 本地 Hexo 框架搭建

- 安装框架：通过 npm 全局安装 Hexo 工具：

- 初始化博客：在文件夹 C:\Users\xcx\myblog 下执行：

- 产物：此时拥有了一个默认的 Landscape 主题博客。

![本地部署Hexo成功](/img/26.1.20/本地部署hexo.png)

3. Fluid 主题配置

![更换fluid主题 cmd界面](/img/26.1.20/fluid.png)
![更换fluid主题 cmd界面 done](/img/26.1.20/fluid-done.png)
![更换fluid主题 localpost界面](/img/26.1.20/fluid-local.png)

4. GitHub 远程部署

- 创建仓库：在 GitHub 新建名为 xcxxx-822.github.io 的公开仓库。

- 解决权限封锁问题：在 GitHub Settings 生成 Personal Access Token。

![github上最终成功界面](/img/26.1.20/github.png)

### 问题 1：修改配置文件后本地样式无变化

- 现象：在 _config.yml 中修改 theme: fluid 后，执行 hexo g 依然生成默认的 Landscape 主题文件。

- 原因分析：

1. 配置未生效：Hexo 未能正确读取到修改后的配置文件。

2. 主题资源缺失：在新版 Hexo 中，通过 npm 安装的主题有时无法被 Hexo 自动识别路径。

- 解决方法：

1. 物理搬运：将 node_modules/hexo-theme-fluid 文件夹手动复制到 themes/ 目录下，并重命名为 fluid。

2. 强制清理：手动删除 public 文件夹和 db.json 文件，确保 Hexo 重新构建索引。

### 问题 2：部署时提示 Repository not found

- 现象：执行 hexo d 时报错 ERROR: Repository not found 或 fatal: Could not read from remote repository。

- 原因分析：

1. 权限验证失败：SSH Key 未配置或 HTTPS 自动授权弹窗被浏览器/防火墙拦截。

2. 仓库错误：github上的仓库并未建立成功，导致_config.yml 中的 repo 地址错误。

- 解决方法：

1. 使用 Token 验证：在 GitHub 生成 Personal Access Token (classic)。

2. 重构 Repo 链接：在配置文件中将 Token 直接嵌入 URL 中：


### 问题 3：线上页面 404 或样式乱码
- 现象：部署成功后访问域名，显示 GitHub 默认的 404 页面，或者页面没有 CSS 样式。

- 原因分析：

1. 分支设置错误：Hexo 默认推送到了 master 分支，但 GitHub Pages 默认读取的是 main 分支。

2. URL 配置偏差：_config.yml 里的 url 仍为 example.com，导致资源链接失效。

- 解决方法：

修正 URL：将 url 修改为真实的 GitHub Pages 地址。 

```yaml
url: 
https://xcxxx-822.github.io
```

