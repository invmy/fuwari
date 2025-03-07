---
title: fuwari主题DecapCMS配置config
published: 2025-03-03
description: fuwari主题无头 DecapCMS配置config
category: '折腾'
tags: ["DecapCMS"]
---

## 项目
https://github.com/i40west/netlify-cms-cloudflare-pages

## 准备

Cloudflare 账号

Github 账号

## 创建Github OAuth

https://github.com/settings/developers

创建一个Oauth，所有Url均填写你的网站链接

列如https://0.123456.xyz

保存页面的两个数据备用

Client ID和Client secrets

## CMS程序
下载项目文件

admin放到公开目录，astro在public

functions 放到根目录 /

## 编辑config.yml
编辑配置以适配你的md文档格式。具体询问chatgpt或者抄别人的。

按需修改，name不要动。主要修改域名

```
backend:
  name: github
  repo: user/astro-theme-cactus
  branch: main # Branch to update (optional; defaults to master)
  site_domain: https://0.123456.xyz
  base_url: https://0.123456.xyz
  auth_endpoint: /api/auth

media_folder: public/images
public_folder: /images

collections:
  - name: 'posts'
    label: 'posts'
    folder: 'src/content/posts'
    create: true
    editor:
      preview: true
      frame: true
    sortable_fields: ["published","updated","title"]
    summary: "{{published}}  {{title}}"
    fields:
      - { label: "封面",name: "image",widget: "image",required: false, choose_url: true,hint: "可以上传也可以插入链接",}
      - { label: '标题', name: 'title', widget: 'string' ,required: true}
      - { label: '发布日期', name: 'published', widget: 'datetime',required: true , date_format: "YYYY-MM-DD"}
      - { label: '更新日期', name: 'updated', widget: 'datetime',required: false,  date_format: "YYYY-MM-DD"}
      - { label: '描述', name: 'description', widget: 'string',required: false}
      - { label: "分类", name: "category", widget: "string",required: false, hint: "输入一个分类名，不可多个分类名"}
      - { label: "标签", name: "tags", widget: "list",required: false, hint: "输入标签使用英文逗号分割，无限制数量。列如：标签1,标签2,标签3" }
      - { label: '正文', name: 'body', widget: 'markdown'}

  - name: 'spec'
    label: 'spec'
    folder: 'src/content/spec'
    create: true
    editor:
      preview: true
      frame: true
    summary: "{{filename}}"
    sortable_fields: ["title"]
    fields:
      - { label: '路径', name: "title", widget: "string",required: true, hint: "使用小写英文路径 , 仅创建时有效。创建完不支持修改，必须手动修改src/content/spec/下的文件名" }
      - { label: '正文', name: 'body',widget: 'markdown'}
```

## 部署到cloudflare Pages
确保你的程序再次之前能正常编译。

创建时添加上方提到的两个变量。
```
GITHUB_CLIENT_ID = Client ID
GITHUB_CLIENT_SECRET = Client secrets
```

这时访问你的网站/admin

例如https://0.123456.xyz/admin

## 无法验证OA
检测是否填错网址

如果你的文件放在github想套CDN 

## 给图片套CDN

修改config

```
media_folder: public/images
public_folder: "https://cdn.jsdelivr.net/gh/user/repo@main/public/images"
```