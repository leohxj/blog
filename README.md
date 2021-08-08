# 个人博客

[![Netlify Status](https://api.netlify.com/api/v1/badges/6724e1cd-20ce-4f86-896b-1073301b7529/deploy-status)](https://app.netlify.com/sites/elated-goldstine-2117ce/deploys)

使用 [Hugo](https://gohugo.io/) 构建, 部署在 [netlify](https://www.netlify.com/) 上。

## 更新主题

`git clone https://github.com/xianmin/hugo-theme-jane.git --depth=1 themes/jane`

## Hugo 启动

### 开发环境

开发环境下, 可查看所有.

`hugo server -D`, 访问 `http://127.0.0.1:1313`

### 构建

`hugo`, 构建输出的内容在 public 目录中, 可通过 `-d` 指定输出目录

## Articles

`hugo new posts/my-first-post.md` 默认创建的是 Draft 文件

### Drafts

> Drafts do not get deployed; once you finish a post, update the header of the post to say draft: false. More info here.
