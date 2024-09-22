---
title: Hexo & Github 博客搭建
date: 2024-09-12
tags:
  - 教程
  - 前端
---
# Hexo & Github 博客搭建
## 安装 Node.js 和 Git 必要软件
- Node.js: [https://nodejs.org/en](https://nodejs.org/en)
- Git: [https://git-scm.com/download/win](https://git-scm.com/download/win)
- cmd测试命令(检测node安装)
```
node -v
npm -v
git --version
```
## 打开Github，重新配置新的密钥
- 配置SSH,一般位于C盘用户.ssh(id_rsa.pub)
```
ssh-keygen -t rsa -C"email"
```
- 打开GitHub账户,创建新的SSH链接
- 验证链接账户邮箱是否成功
```
ssh -T git@github.com
```
- 此外您还需要如下配置:
```
git config --global user.name “name”
git config --golbal user.email"email"
```
## 安装hexo框架
- 在 cmd 下输入下面指令安装 hexo
```
npm install hexo-cli -g
```
- 如果网络被阉，可以使用阿里云镜像源进行安装
```
npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm install -g hexo-cli
```
- 初始化hexo, 在 D:\blog 文件夹下,右键打开 Git Bash Here，输入以下命令
```
hexo init    /*进行初始化*/
```
## 安装依赖
在 D:\blog 文件夹下右键打开 Git Bash Here，输入以下命令  
```
npm install  // 安装依赖
npm install hexo-deployer-git --save  // 文章部署到 git 的模块
（下面为选择安装）
npm install hexo-generator-feed --save  // 建立 RSS 订阅
npm install hexo-generator-search --save  // 建立文章搜索
npm install hexo-generator-sitemap --save // 建立站点地图
```
Tips: 备用阿里云镜像源cnpm,卡死了可以直接按下 CTRL+C 终止  
```
git config --global http.sslVerify false  // 出现SSL错误,可绕过证书验证（仅用于开发环境）
```
## 更换主题
Hexo主题：[Themes | Hexo](https://hexo.io/themes/)
## 部署发布文章
- 在 D:\blog 文件夹下右键打开 Git Bash Here，输入以下命令
```
npm install hexo-deployer-git --save    /*安装hexo部署插件*/
hexo clean        /*清除缓存 网页正常情况下可以忽略此条命令*/  
hexo g            /*生成静态网页*/  
hexo s            /*本地部署*/  
hexo d            /*开始部署*/  
```
## 云端部署与同步
- 本地文件可以在OneDrive创建
- Zeabur或者Vercel部署
- Github默认的pages页面部署