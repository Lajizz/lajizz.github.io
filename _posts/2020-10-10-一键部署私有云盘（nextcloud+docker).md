---
title: 一键部署私有云盘nextCloud
tags: SomeNotes
---

# 使用docker部署自己的云盘
## 先行条件
* 公网服务器
* 安装docker
## 步骤
* 一、`docker pull docker.io/nextcloud`
* 二、`docker run -d --restart=always --name nextcloud -p 80:80 -v /root/nextcloud:/data docker.io/nextcloud`
* 三、浏览器输入ip:port
## 使用
* ios android web皆可使用