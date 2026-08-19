---
title: 使用 GitHub Pages + Hexo 搭建博客
date: 2024-10-20 15:20:00
tags:
  - Hexo
  - GitHub Pages
  - 博客搭建
categories:
  - 技术
---


## 🌱 一、先导步骤

### 1. 安装 Node.js
前往 [Node.js 官网](https://nodejs.org/)，下载 LTS 版本并安装。  
安装完成后检查版本：

```bash
node -v
npm -v
```

### 2. 安装 Git

前往 [Git 官网](https://git-scm.com/)，安装后检查是否成功：

```bash
git --version
```

### 3. 安装 Hexo

```bash
npm install hexo-cli -g
hexo -v
```

若成功显示版本号，说明 Hexo 已安装完成。

------

## ⚙️ 二、博客构建

### 1. 初始化项目

创建一个新的文件夹作为博客目录（例如 `myblogs`）：

```bash
hexo init myblogs
cd myblogs
npm install
```

### 2. 本地测试

```bash
hexo generate
hexo server
```

打开浏览器访问 [http://localhost:4000](http://localhost:4000/)，
 你就能看到默认的 Hexo 博客主页啦 🎉

------

## 🚀 三、GitHub 部署

### 1. 注册 GitHub

访问 [GitHub](https://github.com/) 注册账号。

创建一个新的公开仓库，命名格式为：

```
username.github.io
```

其中 `username` 是你的 GitHub 用户名。

### 2. 修改 `_config.yml`

在 Hexo 博客根目录打开 `_config.yml` 文件，找到末尾的 `deploy` 部分并修改（username是你的github用户名）：

```yaml
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: gh-pages
```

完成后将部署分支切换到 gh-pages，如图

![2552X1122/屏幕截图_2025-10-22_203615.png](https://tc.z.wiki/autoupload/f/zpduUOTToOUwtaMnTYFBl8kJiVcdDX0rG09M-a0gARI/20251022/vNWB/2552X1122/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE_2025-10-22_203615.png)

### 3. 安装部署插件

```bash
npm install hexo-deployer-git --save
```

### 4. 部署命令

```bash
hexo clean
hexo generate
hexo deploy
```

简写命令：

```bash
hexo cl
hexo g
hexo d
```

之后在浏览器中输入：

```
https://username.github.io
```

你的博客就可以在线访问了 🚀

> 提示：
>
> - Pages 设置中请选择 **gh-pages** 分支部署。
> - 仓库必须保持 **Public** 状态。

------

## 🎨 四、博客优化与美化

### 1. 更换主题

推荐使用 Hexo 的 Butterfly 主题：

```bash
git clone https://github.com/jerryc127/hexo-theme-butterfly themes/butterfly
```

修改 `_config.yml`：

```yaml
theme: butterfly
```

重新生成并部署：

```bash
hexo clean 
hexo g 
hexo d
```

关于butterfly主题的配置可以参考[Butterfly基础美化_butterfly设置友链-CSDN博客](https://blog.csdn.net/AAbbccdd12345685/article/details/117871129)

------

### 2. 网站信息配置

在 `_config.yml` 中设置：

```yaml
title: 网站名称
subtitle: '子标题'
description: '网站描述'
keywords:
author: 你的名字
language: zh-CN
timezone: ''
```

------

### 3. 菜单配置

编辑主题配置文件 themes：

```yaml
menu:
  首页: / || fas fa-home
  分类: /categories/ || fas fa-folder-open
  时间轴: /archives/ || fas fa-archive
  标签: /tags/ || fas fa-tags
  友情链接: /link/ || fas fa-link
  关于我: /about/ || fas fa-heart
```

------

### 4. 创建关键页面

进入博客根目录执行：

```bash
hexo new page pagename
```

例如：

```bash
hexo new page categories
hexo new page archives
hexo new page tags
hexo new page link
hexo new page about
```



## 🌐 五、发布成功效果

部署完成后，你的博客就可以通过以下地址访问：

```
https://username.github.io
```

它是一个完全静态的网站，可以直接在全球范围内访问。
 你可以随时通过本地修改 Markdown 文件、执行三步命令更新网站：

```bash
hexo clean
hexo generate
hexo deploy
```

------

## 🪄 六、总结

| 功能          | 命令                  |
| ------------- | --------------------- |
| 新建文章      | `hexo new "文章标题"` |
| 生成页面      | `hexo generate`       |
| 本地预览      | `hexo server`         |
| 部署到 GitHub | `hexo deploy`         |
| 清理缓存      | `hexo clean`          |

------

