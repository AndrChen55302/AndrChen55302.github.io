---
title: 使用GitHub Action 自动部署
date: 2021-09-06
cover: https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630918531000.jpg
tags: GitHub Action
categories: GitHub
---
## 前言

在我们使用hexo上传文章到仓库的时候 经常需要输入各种命令还有可能遇到各种错误

github也有解决方案  action自动部署

### 1.准备工作

1. GitHub 官方的 action：[GitHub Actions](https://github.com/features/actions)
2. **GitHub 仓库**
   一般命名为 `{{username}}.github.io` 这种形式。
   在本仓库上再创建一个分支用于保存 Hexo 开发源码。
   使用建好的分支进行 Hexo 源码备份，使用 `master` 分支进行博客源码部署。
   这里也可以建两个仓库分别进行博客源码和 Hexo 开发源码的保存，跟建两个分支一样。

确认 `_config.yml` 文件中有类似如下的 `GitHub Pages` 配置：

```yaml
deploy:
  type: git
  repository: git@github.com:xpnobug/blog.git
  branch: master
```

> ​	注意：将 `repository` 修改为自己的仓库地址。

### 2.**创建 GitHub Personal Access Token**（创建个人访问令牌）

用于 GitHub Actions 所构建得虚拟系统可以内容推送到仓库。
要使用令牌从命令行访问仓库，请选择 **repo（仓库）**。

### 3.**设置仓库 Secrets**

将创建好的 Personal Access Token 添加到仓库的 Secrets 中 设置名称：ACCESS_TOKEN

> 当然名字可以随便起

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630915432000.png)

> 应确保这里正确

![](https://cdn.jsdelivr.net/gh/AndrChen55302/CDN@main/img/hpp_upload/1630915665000.png)

### 4.创建 workflow 脚本

```yaml
name: Blog CI/CD

# 触发条件：在 push 到 hexo-blog-backup 分支后触发
on:
  push:
    branches: 
      - master

env:
  TZ: Asia/Shanghai

jobs:
  blog-cicd:
    name: Hexo blog build & deploy
    runs-on: ubuntu-latest # 使用最新的 Ubuntu 系统作为编译部署的环境

    steps:
    - name: Checkout codes
      uses: actions/checkout@v2

    - name: Setup node
      # 设置 node.js 环境
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'

    - name: Cache node modules
      # 设置包缓存目录，避免每次下载
      uses: actions/cache@v1
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

    - name: Install hexo dependencies
      # 下载 hexo-cli 脚手架及相关安装包
      run: |
        npm install -g hexo-cli
        npm install
    - name: Generate files
      # 编译 markdown 文件
      run: |
        hexo clean
        hexo generate
    - name: Deploy hexo blog
      env: 
        # Github 仓库
        GITHUB_REPO: github.com/AndrChen55302/AndrChen55302.github.io
      # 将编译后的博客文件推送到指定仓库
      run: |
        cd ./public && git init && git add .
        git config user.name "AndrChen55302"
        git config user.email "2736933203@qq.com"
        git add .
        git commit -m "GitHub Actions Auto Builder at $(date +'%Y-%m-%d %H:%M:%S')"
        git push --force --quiet "https://${{ secrets.ACCESS_TOKEN }}@$GITHUB_REPO" master:hexo-blog
```

workflow 详细语法见： [GitHub 操作的工作流程语法](https://link.zhihu.com/?target=https://docs.github.com/cn/actions/reference/workflow-syntax-for-github-actions)

最后，只需要将源码推送到指定分支，GitHub Actions 就会自动部署项目.

在最后，使用 [hexoplus 后台管理部署步骤：](https://hexoplusplus.js.org/start/)
