# Personal Blog 维护说明

这是一个基于 Hexo 的个人博客项目，当前使用主题为 `shana`。

线上地址：

```text
https://AdonisKlein.github.io/Personal-blog/
```

当前部署配置位于 [_config.yml](./_config.yml)：

```yml
url: https://AdonisKlein.github.io/Personal-blog
root: /Personal-blog/

deploy:
  type: git
  repository: https://github.com/AdonisKlein/Personal-blog.git
  branch: gh-pages
```

## 常用命令

本地预览：

```bash
npm run server
```

或：

```bash
hexo s
```

默认访问：

```text
http://localhost:4000
```

清理旧生成文件：

```bash
npm run clean
```

生成静态网站：

```bash
npm run build
```

部署到 GitHub Pages：

```bash
npm run deploy
```

等价 Hexo 命令：

```bash
hexo clean
hexo g
hexo d
```

## 推荐更新流程

修改文章、图片、主题或配置后，建议按这个顺序操作：

```bash
npm run clean
npm run build
npm run server
```

本地确认无误后，部署线上网站：

```bash
npm run deploy
```

最后保存源码到 GitHub 的 `main` 分支：

```bash
git add .
git commit -m "Update blog"
git push origin main
```

两类上传的区别：

```text
npm run deploy      -> 更新 gh-pages 分支，也就是线上网站内容
git push origin main -> 保存 main 分支，也就是博客源码、主题、文章、图片
```

## 目录说明

```text
source/_posts/                         文章目录
source/uploads/                        站点通用图片，例如头像
themes/shana/                          当前主题
themes/shana/_config.yml               主题配置
themes/shana/source/css/images/        主题图片，例如背景图
themes/shana/source/plugin/bganimation/bg.css  全屏轮播背景
themes/shana/layout/_partial/          主题页面模板
public/                                Hexo 生成结果，不要手动维护
.deploy_git/                           部署缓存目录，不要手动维护
```

## 修改文章

文章放在：

```text
source/_posts/
```

新建文章：

```bash
npx hexo new "文章标题"
```

也可以直接在 `source/_posts/` 下创建 Markdown 文件。

常见文章头部示例：

```markdown
---
title: 文章标题
date: 2026-07-11 23:00:00
tags:
  - Hexo
categories:
  - Blog
---

正文内容
```

## 修改站点基础信息

站点标题、作者、语言、部署地址在 [_config.yml](./_config.yml)。

常改字段：

```yml
title: Adonis
subtitle: 'A Personal Blog'
description: ''
author: John Doe
language: zh-CN
```

注意：因为网站部署在 `/Personal-blog/` 下，下面两项不要随意删除：

```yml
url: https://AdonisKlein.github.io/Personal-blog
root: /Personal-blog/
```

否则线上可能出现 CSS、JS、图片路径错误，页面像“主题丢失”。

## 修改头像

头像文件建议放在：

```text
source/uploads/avatar.jpg
```

然后修改主题配置：

```text
themes/shana/_config.yml
```

对应字段：

```yml
avatar: /uploads/avatar.jpg
```

当前主题模板已经使用 `url_for()` 处理路径，所以线上会自动生成：

```text
/Personal-blog/uploads/avatar.jpg
```

## 修改顶部横幅图

顶部横幅不是全屏轮播背景。

配置文件：

```text
themes/shana/source/css/_variables.styl
```

当前字段：

```stylus
banner-height = 300px
banner-url = "images/MyGO.jpg"
```

图片建议放在：

```text
themes/shana/source/css/images/
```

例如：

```text
themes/shana/source/css/images/my-banner.jpg
```

然后改为：

```stylus
banner-url = "images/my-banner.jpg"
```

## 修改全屏轮播背景

你看到的多张底图轮换来自这个文件：

```text
themes/shana/source/plugin/bganimation/bg.css
```

当前轮播使用 5 张图片：

```css
.cb-slideshow li:nth-child(1) span {
  background-image: url('../../css/images/1.png');
}

.cb-slideshow li:nth-child(2) span {
  background-image: url('../../css/images/2.png');
}

.cb-slideshow li:nth-child(3) span {
  background-image: url('../../css/images/3.jpg');
}

.cb-slideshow li:nth-child(4) span {
  background-image: url('../../css/images/4.png');
}

.cb-slideshow li:nth-child(5) span {
  background-image: url('../../css/images/5.png');
}
```

对应图片位于：

```text
themes/shana/source/css/images/1.png
themes/shana/source/css/images/2.png
themes/shana/source/css/images/3.jpg
themes/shana/source/css/images/4.png
themes/shana/source/css/images/5.png
```

如果替换图片，最简单的方式是保持文件名不变，直接替换这 5 个文件。

如果改文件名，需要同时修改 `bg.css` 里的路径。

如果增加或减少轮播图片，还需要同步修改：

```text
themes/shana/layout/_partial/bganimation.ejs
```

例如 5 张图时，里面应该有 5 个 `<li>`：

```html
<li><span>1</span></li>
<li><span>2</span></li>
<li><span>3</span></li>
<li><span>4</span></li>
<li><span>5</span></li>
```

当前动画周期是 30 秒，每张约 6 秒。相关配置在 `bg.css` 中：

```css
animation: imageAnimation 30s linear infinite 0s;
```

如果想 5 张图每张显示 8 秒，可以把总时长改为 `40s`，并把后续图片延迟改为：

```css
8s, 16s, 24s, 32s
```

## 修改菜单

主菜单配置在：

```text
themes/shana/_config.yml
```

字段：

```yml
menu:
  Home: /
  Archives: /archives
  Categories: /categories
  Tags: /tags
```

右键圆形菜单配置也在同一个文件：

```yml
galmenu: true
mp3play: false
gaymenu_item:
- name: 首页
  url : /
- name: 标签
  url : /tags
- name: 分类
  url : /categories
- name: 归档
  url : /archives
```

站内路径建议写 `/archives`、`/tags` 这种形式，不要手动写 `/Personal-blog/archives`。主题模板会自动补上 `root`。

## 修改社交链接和友情链接

配置文件：

```text
themes/shana/_config.yml
```

社交链接：

```yml
social:
- url: https://github.com/你的用户名
  name: Github
```

友情链接：

```yml
link_title: 友情链接
links:
  名称: https://example.com/
```

## 图片路径规则

这个项目部署在：

```text
/Personal-blog/
```

因此不要在 HTML 或配置里随便写死完整根路径，例如：

```text
/css/images/a.jpg
```

站内配置推荐写：

```text
/uploads/avatar.jpg
/archives
/tags
```

主题模板会通过 `url_for()` 自动变成：

```text
/Personal-blog/uploads/avatar.jpg
/Personal-blog/archives
/Personal-blog/tags
```

CSS 文件里的图片路径建议使用相对路径。例如 `bg.css` 位于：

```text
plugin/bganimation/bg.css
```

要引用：

```text
css/images/1.png
```

应该写：

```css
url('../../css/images/1.png')
```

## 部署后没有变化怎么办

先检查本地是否生成正确：

```bash
npm run clean
npm run build
```

检查生成文件：

```text
public/index.html
public/css/style.css
public/plugin/bganimation/bg.css
```

确认无误后部署：

```bash
npm run deploy
```

如果线上仍然没变：

1. 等待 GitHub Pages 刷新 1 到 3 分钟。
2. 浏览器按 `Ctrl + F5` 强制刷新。
3. 直接访问图片地址确认是否更新，例如：

```text
https://AdonisKlein.github.io/Personal-blog/uploads/avatar.jpg
https://AdonisKlein.github.io/Personal-blog/css/images/1.png
```

4. 到 GitHub 仓库 Settings -> Pages，确认发布分支是 `gh-pages`。

## 不建议手动修改的目录

不要手动维护：

```text
public/
.deploy_git/
node_modules/
```

原因：

```text
public/      由 hexo generate 自动生成
.deploy_git/ 由 hexo deploy 自动维护
node_modules/ 由 npm install 自动安装
```

真正应该修改和提交的是：

```text
_config.yml
source/
themes/shana/
package.json
package-lock.json
README.md
```

## 新电脑恢复项目

克隆仓库后安装依赖：

```bash
npm install
```

本地预览：

```bash
npm run clean
npm run build
npm run server
```

部署：

```bash
npm run deploy
```

如果部署需要登录 GitHub，按弹出的 GitHub 登录窗口完成授权。
