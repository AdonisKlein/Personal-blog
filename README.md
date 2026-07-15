# Personal Blog 维护说明

这是一个基于 Hexo 的个人博客项目，当前使用主题为 `shana`。

本地服务启动后地址：

```text
http://localhost:4000/Personal-blog/
```

线上地址：

```text
https://AdonisKlein.github.io/Personal-blog/
```

部署配置文件： [_config.yml](./_config.yml) [路径](./)

当前字段：

```yml
url: https://AdonisKlein.github.io/Personal-blog
root: /Personal-blog/

deploy:
  type: git
  repository: https://github.com/AdonisKlein/Personal-blog.git
  branch: gh-pages
```

## 常用命令

本地预览： `npm run server` 或 `hexo s`

清理旧生成文件： `npm run clean` 或 `hexo clean`

生成静态网站： `npm run build` 或 `hexo g`

部署到 GitHub Pages： `npm run deploy` 或 `hexo d`

## 更新流程

修改文章、图片、主题或配置后操作：

```bash
hexo clean
hexo g
hexo s
```

本地确认无误后，部署线上网站：

```bash
hexo d
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
themes/shana/source/css/images/        主题图片根目录
themes/shana/source/css/images/background/  五张全屏轮播背景
themes/shana/source/css/images/pointer/     自定义鼠标指针
themes/shana/source/plugin/bganimation/bg.css  全屏轮播背景
themes/shana/layout/_partial/          主题页面模板
public/                                Hexo 生成结果，不要手动维护
.deploy_git/                           部署缓存目录，不要手动维护
```

## 新建文章

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

### 按文章启用数学公式

KaTeX 样式默认不加载。只需在包含公式的文章 Front Matter 中添加：

```yaml
---
title: 数学公式示例
date: 2026-07-14 12:00:00
katex: true
---
```

主题同时兼容旧的 `math: true` 写法。不包含 `katex: true` 或 `math: true` 的页面不会请求
KaTeX CSS。这个条件只控制样式加载；Markdown 公式能否转换为 HTML 仍取决于数学渲染插件。

## 修改站点信息

### 修改站点基本信息

配置文件： [_config.yml](./_config.yml) 

当前站点标题、作者、语言、部署地址字段：

```yml
title: Adonis's Personal Blog
subtitle: 'A Personal Blog'
description: ''
author: 'Adonis Klein'
language: zh-CN
```

注意：因为网站部署在 `/Personal-blog/` 下，下面两项不要随意删除：

```yml
url: https://AdonisKlein.github.io/Personal-blog
root: /Personal-blog/
```

否则线上可能出现 CSS、JS、图片路径错误，页面像“主题丢失”。

### 修改头像

配置文件：[_config.yml](./themes/shana/_config.yml) [路径](./themes/shana) 

当前字段：

``` 
avatar: /uploads/avatar.jpg
```

头像文件： [avatar.jpg](./source/uploads/avatar.jpg) [路径](source/uploads) 

当前主题模板已经使用 `url_for()` 处理路径，所以线上会自动生成：

```text
/Personal-blog/uploads/avatar.jpg
```

### 修改顶部横幅图

配置文件： [_variables.sty](./themes/shana/source/css/variables.styl) [路径](./themes/shana/source/css/) 

当前字段：

```stylus
banner-height = 300px
banner-url = "images/banner.webp"
```

图片文件：[banner.webp](./themes/shana/source/css/images/banner.webp) [路径](./themes/shana/source/css/images/) 

顶部横幅由 `themes/shana/layout/_partial/header.ejs` 中的 `#banner` 元素展示，样式位于 `themes/shana/source/css/_partial/header.styl`。`banner.jpg` 会被 Hexo 复制到 `public/`，但因为 CSS 没有引用，浏览器不会主动下载。

如果修改了 `_variables.styl` 后生成结果仍引用旧文件，必须清理 Hexo 缓存：

```bash
hexo clean
hexo g
```

### 修改全屏轮播背景

配置文件： [bg.css](./themes/shana/source/plugin/bganimation/bg.css) [路径](./themes/shana/source/plugin/bganimation/) 

当前字段：

```css
.cb-slideshow li:nth-child(1) span {
  background-image: url('../../css/images/background/background_1.webp');
}

.cb-slideshow li:nth-child(2) span {
  background-image: url('../../css/images/background/background_2.webp');
}

.cb-slideshow li:nth-child(3) span {
  background-image: url('../../css/images/background/background_3.webp');
}

.cb-slideshow li:nth-child(4) span {
  background-image: url('../../css/images/background/background_4.webp');
}

.cb-slideshow li:nth-child(5) span {
  background-image: url('../../css/images/background/background_5.webp');
}
```

对应图片位于：[路径](./themes/shana/source/css/images/background/)

如果替换图片，保持文件名不变，直接替换这 5 个文件。

如果改文件名，需要同时修改 `bg.css` 里的路径。

如果增加或减少轮播图片，还需要同步修改：

```text
themes/shana/layout/_partial/bganimation.ejs
```

当前 5 张图对应 5 个 `<li>`：

```html
<li><span>1</span></li>
<li><span>2</span></li>
<li><span>3</span></li>
<li><span>4</span></li>
<li><span>5</span></li>
```

当前动画周期是 30 秒，每张间隔 6 秒。相关配置在 `bg.css` 中：

```css
animation: imageAnimation 30s linear infinite 0s;
```

五张图片的延迟依次为 `0s`、`6s`、`12s`、`18s`、`24s`。修改数量或单张间隔时，需同时调整：

```text
总周期 = 图片数量 × 单张间隔
第 n 张延迟 = (n - 1) × 单张间隔
```

例如 5 张图每张间隔 8 秒，应把总时长改为 `40s`，后续图片延迟改为：

```css
8s, 16s, 24s, 32s
```

背景图会以 `background-size: cover` 覆盖视口。

### 修改网站字体

正文使用系统字体栈，不下载额外的中文字体文件：

```stylus
font-sans = "PingFang SC", "Microsoft YaHei", "Noto Sans CJK SC", "Helvetica Neue", Arial, sans-serif
```

配置位于 `themes/shana/source/css/_variables.styl`。macOS 和 iOS 优先使用苹方，Windows
优先使用微软雅黑，其他系统依次使用思源黑体或通用无衬线字体。

### 修改自定义鼠标指针

鼠标指针资源位于： [路径](./themes/shana/source/css/images/pointer/) 

映射规则位于： [style.styl](./themes/shana/source/css/style.styl) [路径](./themes/shana/source/css/)

当前自动映射：

| 页面状态 | 指针文件 | 触发元素 |
| --- | --- | --- |
| 普通 | `Normal.cur` | 页面默认区域 |
| 链接 | `Link.cur` | 链接、按钮、下拉框、按钮角色元素 |
| 文本 | `Text.cur` | 输入框、文本域、可编辑元素 |
| 禁用 | `Unavailable.cur` | `disabled` 或 `aria-disabled="true"` |
| 工作中 | `Working.cur` | `aria-busy="true"` |
| 移动 | `Move.cur` | `draggable="true"` |

其他指针没有通用 HTML 自动状态，通过 CSS 工具类使用：

| 工具类 | 指针文件 | 用途 |
| --- | --- | --- |
| `.cursor-busy` | `Busy.cur` | 阻塞等待 |
| `.cursor-help` | `Help.cur` | 帮助信息 |
| `.cursor-precision` | `Precision.cur` | 精确选取 |
| `.cursor-resize-vertical` | `Vertical.cur` | 垂直缩放 |
| `.cursor-resize-horizontal` | `Horizontal.cur` | 水平缩放 |
| `.cursor-resize-diagonal-1` | `Diagonal1.cur` | 西北至东南缩放 |
| `.cursor-resize-diagonal-2` | `Diagonal2.cur` | 东北至西南缩放 |
| `.cursor-handwriting` | `Handwriting.cur` | 手写输入 |
| `.cursor-alternate` | `Alternate.cur` | 备用选择 |
| `.cursor-person` | `Person.cur` | 人员相关操作 |
| `.cursor-pin` | `Pin.cur` | 位置相关操作 |

使用示例：

```html
<div class="cursor-help">帮助内容</div>
<div class="cursor-resize-horizontal">可水平调整的区域</div>
```

更换整套指针时，可以保留当前文件名直接覆盖 `.cur` 文件。如果文件名变化，还要同步修改 `style.styl` 中对应的 `url('images/pointer/文件名.cur')`。所有声明都保留了标准 CSS 光标作为回退，因此浏览器不支持 `.cur` 时仍可正常使用。

### 修改菜单

主菜单配置文件： [_config.yml](./themes/shana/_config.yml) [路径](./themes/shana/)

当前字段：

```yml
menu:
  Home: /
  Archives: /archives
  Categories: /categories
  Tags: /tags
```

右键圆形菜单配置字段：

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

站内路径形式为 `/archives`、`/tags` 等。

### 修改社交链接和友情链接

配置文件： [_config.yml](./themes/shana/_config.yml) [路径](./themes/shana/)

当前社交链接字段：

```yml
social:
- url: https://github.com/你的用户名
  name: Github
```

友情链接字段：

```yml
link_title: 友情链接
links:
  名称: https://example.com/
```

## 本地 jQuery

主题使用的 jQuery 2.1.4 已从外部百度静态资源地址改为本地托管：

```text
themes/shana/source/js/vendor/jquery-2.1.4.min.js
```

引用位于 `themes/shana/layout/_partial/after-footer.ejs`：

```ejs
<%- js('js/vendor/jquery-2.1.4.min') %>
```

Hexo 构建后会生成 `public/js/vendor/jquery-2.1.4.min.js`，并自动补充站点的
`/Personal-blog/` 根路径。jQuery 必须保持在 Fancybox、GalMenu 和 `js/script.js` 之前加载。
升级 jQuery 时应先使用新文件名并修改此引用，再检查图片预览、右键菜单、移动导航和分享功能。

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

要引用轮播目录中的：

```text
css/images/background/background_1.webp
```

应该写：

```css
url('../../css/images/background/background_1.webp')
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
https://AdonisKlein.github.io/Personal-blog/css/images/background/background_1.webp
https://AdonisKlein.github.io/Personal-blog/css/images/MyGO.png
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
