---
title: hexo-butterfly主题魔改记录之二
keywords: 'hexo,美化,教程'
top_img: img/hzw.webp
cover: img/hzw.webp
sticky: 2
categories:
  - hexo
tags:
  - hexo
abbrlink: 48701
date: 2022-07-17 01:08:27
updated: 2022-07-17 01:08:27
---

# hexo-butterfly主题魔改记录之二

## Front-matter

Front-matter 是 markdown 文件最上方以 --- 分隔的区域，用于指定个别档案的变数。

- Page Front-matter 用于页面配置

- Post Front-matter 用于文章页配置

> 如果标注可选的参数，可根据自己需要添加，不用全部都写在markdown里

### Page Front-matter

```front-matter
---
title:
date:
updated:
type:
comments:
description:
keywords:
top_img:
mathjax:
katex:
aside:
aplayer:
highlight_shrink:
---
```

![Page Front-matter](https://s3.bmp.ovh/imgs/2022/07/16/d524c25e6514356f.png)

### Post Front-matter

```
---
title:
date:
updated:
tags:
categories:
keywords:
description:
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
```

![Post Front-matter](https://s3.bmp.ovh/imgs/2022/07/16/6996a4c36d4e06f4.png)

## 标签页

1. 前往你的 Hexo 博客的根目录
2. 输入`hexo new page tags`

3. 你会找到source/tags/index.md这个文件

4. 修改这个文件：

5. 记得添加 `type: "tags"`


```
---
title: tags
date: 2022-07-16 23:17:51
type: "tags"
---

```

## 分类页

1. 前往你的 Hexo 博客的根目录

2. 输入`hexo new page categories`

3. 你会找到source/categories/index.md这个文件

4. 修改这个文件：

5. 记得添加 `type: "categories"`

   ```
   ---
   title: categories
   date: 2022-07-16 23:19:37
   type: "categories"
   ---
   ```

## 友情链接

为你的博客创建一个友情链接！

### 创建友情链接页面

1. 前往你的 Hexo 博客的根目录
2. 输入 `hexo new page link`

3. 你会找到source/link/index.md这个文件

4. 修改这个文件：

5. 记得添加 `type: "link"`


```
---
title: link
date: 2022-07-16 23:21:21
type: "link"
---
```

### 友情链接添加

1. 本地生成

   > 在Hexo博客目录中的source/_data（如果没有 _data 文件夹，请自行创建），创建一个文件link.yml

   ```yml
   - class_name: 友情链接
     class_desc: 那些人，那些事
     link_list:
       - name: Hexo
         link: https://hexo.io/zh-tw/
         avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
         descr: 快速、简单且强大的网誌框架
   
   - class_name: 网站
     class_desc: 值得推荐的网站
     link_list:
       - name: Youtube
         link: https://www.youtube.com/
         avatar: https://i.loli.net/2020/05/14/9ZkGg8v3azHJfM1.png
         descr: 视频网站
       - name: Weibo
         link: https://www.weibo.com/
         avatar: https://i.loli.net/2020/05/14/TLJBum386vcnI1P.png
         descr: 中国最大社交分享平台
       - name: Twitter
         link: https://twitter.com/
         avatar: https://i.loli.net/2020/05/14/5VyHPQqR6LWF39a.png
         descr: 社交分享平台
   ```

   `class_name` 和 `class_desc` 支持 html 格式书写，如不需要，也可以留空。

2. 远程拉取

   在 `source/link/index.md` 这个文件的 front-matter 添加远程链接

   ```
   flink_url: xxxxx
   ```

   Json 的格式

   ```json
   [
     {
       "class_name": "友情链接",
       "class_desc": "那些人，那些事",
       "link_list": [
         {
           "name": "Hexo",
           "link": "https://hexo.io/zh-tw/",
           "avatar": "https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg",
           "descr": "快速、简单且强大的网誌框架"
         }
       ]
     },
     {
       "class_name": "网站",
       "class_desc": "值得推荐的网站",
       "link_list": [
         {
           "name": "Youtube",
           "link": "https://www.youtube.com/",
           "avatar": "https://i.loli.net/2020/05/14/9ZkGg8v3azHJfM1.png",
           "descr": "视频网站"
         },
         {
           "name": "Weibo",
           "link": "https://www.weibo.com/",
           "avatar": "https://i.loli.net/2020/05/14/TLJBum386vcnI1P.png",
           "descr": "中国最大社交分享平台"
         },
         {
           "name": "Twitter",
           "link": "https://twitter.com/",
           "avatar": "https://i.loli.net/2020/05/14/5VyHPQqR6LWF39a.png",
           "descr": "社交分享平台"
         }
       ]
     }
   ]
   ```

### 友情链接界面设置

由 2.2.0 起，友情链接界面可以由用户自己自定义，只需要在友情链接的md档设置就行，以普通的Markdown格式书写。

## 图库

图库页面只是普通的页面，你只需要`hexo n page xxxxx` 创建你的页面就行

然后使用标签外挂 **galleryGroup**

```yml
<div class="gallery-group-main">
{% galleryGroup '壁纸' '收藏的一些壁纸' '/Gallery/wallpaper' https://i.loli.net/2019/11/10/T7Mu8Aod3egmC4Q.png %}
{% galleryGroup '漫威' '关于漫威的图片' '/Gallery/marvel' https://i.loli.net/2019/12/25/8t97aVlp4hgyBGu.jpg %}
{% galleryGroup 'OH MY GIRL' '关于OH MY GIRL的图片' '/Gallery/ohmygirl' https://i.loli.net/2019/12/25/hOqbQ3BIwa6KWpo.jpg %}
</div>
```

### 子页面

子页面也是普通的页面，你只需要`hexo n page xxxxx` 创建你的页面就行

然后使用标签外挂 **gallery**

```yml
{% gallery %}
![](https://i.loli.net/2019/12/25/Fze9jchtnyJXMHN.jpg)
![](https://i.loli.net/2019/12/25/ryLVePaqkYm4TEK.jpg)
![](https://i.loli.net/2019/12/25/gEy5Zc1Ai6VuO4N.jpg)
![](https://i.loli.net/2019/12/25/d6QHbytlSYO4FBG.jpg)
![](https://i.loli.net/2019/12/25/6nepIJ1xTgufatZ.jpg)
![](https://i.loli.net/2019/12/25/E7Jvr4eIPwUNmzq.jpg)
![](https://i.loli.net/2019/12/25/mh19anwBSWIkGlH.jpg)
![](https://i.loli.net/2019/12/25/2tu9JC8ewpBFagv.jpg)
{% endgallery %}
```

