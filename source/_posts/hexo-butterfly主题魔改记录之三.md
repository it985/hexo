---
title: hexo-butterfly主题魔改记录之三
date: 2022-07-17 02:43:52.695
updated: 2022-07-17 02:43:52.695
url: 
sticky: 3
categories: 
- hexo
tags: 
- hexo
---

# hexo-butterfly主题魔改记录之三

## 语言

> 修改**根目录**配置文件 `_config.yml`，默认语言是 en

主題支持三种语言

- default(en)
- zh-CN (简体中文)
- zh-TW (繁体中文)

## 网站资料

修改网站各种资料，如标题，副标题，邮箱等个人资料，修改**根目录**的`_config.yml`

```yml
# Site
title:  # 博客名
subtitle:  #副标题
description:   # 个人描述
keywords:    #网站关键字
author:   #博客作者
language: zh-CN  #默认语言
timezone: 'Asia/Shanghai'  #时区
```

## 导航菜单

修改**主题配置文件**`_config.yml`

```
Home: / || fas fa-home
Archives: /archives/ || fas fa-archive
Tags: /tags/ || fas fa-tags
Categories: /categories/ || fas fa-folder-open
List||fas fa-list:
  Music: /music/ || fas fa-music
  Movie: /movies/ || fas fa-video
Link: /link/ || fas fa-link
About: /about/ || fas fa-heart
```

必须是 /xxx/，后面||分开，然后写图标名。

如果不希望显示图标，图标名可不写。

默认子目录是展开的，如果你想要隐藏，在子目录里添加 hide 。

```yml
List||fas fa-list||hide:
  Music: /music/ || fas fa-music
  Movie: /movies/ || fas fa-video
```

注意： 导航的文字可自行更改

例如：

```yml
menu:
  首页: / || fas fa-home
  时间轴: /archives/ || fas fa-archive
  标签: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  清单||fa fa-heartbeat:
    音乐: /music/ || fas fa-music
    照片: /Gallery/ || fas fa-images
    电影: /movies/ || fas fa-video
  友链: /link/ || fas fa-link
  关于: /about/ || fas fa-heart
```

## 代码

### 代码高亮主题

Butterfly 支持6种代码高亮样式：

- darker
- pale night
- light
- ocean
- mac
- mac light

修改 **主题配置文件**

```yml
highlight_theme: light
```

### 代码复制

主题支持代码复制功能

```yml
highlight_copy: true
```

### 代码框展开/关闭

在默认情况下，代码框自动展开，可设置是否所有代码框都关闭状态，点击`>`可展开代码

- true 全部代码框不展开，需点击`>`打开

- false 代码框展开，有`>`点击按钮
- none 不显示`>`按钮

修改 **主题配置文件**

```yml
highlight_shrink: true #代码框不展开，需点击 '>' 打开
```

> 你也可以在post/page页对应的markdown文件front-matter添加highlight_shrink来独立配置。
>
> 当主题配置文件中的 highlight_shrink 设为true时，可在front-matter添加highlight_shrink: false来单独配置文章展开代码框。
>
> 当主题配置文件中的 highlight_shrink 设为false时，可在front-matter添加highlight_shrink: true来单独配置文章收缩代码框。
>

### 代码换行

在默认情况下，Hexo 在编译的时候不会实现代码自动换行。如果你不希望在代码块的区域里有横向滚动条的话，那么你可以考虑开启这个功能。

修改 **主题配置文件**

```yml
code_word_wrap: true
```

如果你是使用 highlight 渲染，需要找到你站点的 Hexo 配置文件_config.yml，将line_number改成false:

```yml
highlight:
  enable: true
  line_number: false # <- 改这里
  auto_detect: false
  tab_replace:
```

如果你是使用 prismjs 渲染，需要找到你站点的 Hexo 配置文件_config.yml，将line_number改成false:

```yml
prismjs:
  enable: false
  preprocess: true
  line_number: false # <- 改这里
  tab_replace: ''
```

### 代码高度限制

> 3.7.0 及以上支持

可配置代码高度限制，超出的部分会隐藏，并显示展开按钮。

```yaml
highlight_height_limit: false # unit: px
```

注意：

1. 单位是 px，直接添加数字，如 200

2. 实际限制高度为 highlight_height_limit + 30 px ，多增加 30px 限制，目的是避免代码高度只超出highlight_height_limit 一点时，出现展开按钮，展开没内容。

3. 不适用于隐藏后的代码块（ css 设置 display: none）

## 社交图标

Butterfly支持 [font-awesome v6](https://fontawesome.com/icons?from=io)图标.

书写格式 图标名：url || 描述性文字

```yml
social:
  fab fa-github: https://github.com/xxxxx || Github
  fas fa-envelope: mailto:xxxxxx@gmail.com || Email
```

## 主页文章节选(自动节选和文章页description)

因为主题UI的关係，主页文章节选只支持自动节选和文章页description。

`description`在`front-matter`里添加

在butterfly里，有四种可供选择

1. description： 只显示description
2. both： 优先选择description，如果没有配置description，则显示自动节选的内容
3. auto_excerpt：只显示自动节选
4. false： 不显示文章内容

修改 **主题配置文件**

```yml
index_post_content:
  method: 3
  length: 500 # if you set method to 2 or 3, the length need to config
```

## 顶部图

> 如果不要显示顶部图，可直接配置 disable_top_img: true

![disable_top_img](https://s3.bmp.ovh/imgs/2022/07/17/1c8c03267c5ea5d4.png)

以上所有的 top_img 可配置以下值

> 3.2.0 以下版本的配置只支持
>
> 留空，true 和 false - 显示默认的顔色
> img链接 - 显示所配置的图片

![image-20220717013930149](https://s3.bmp.ovh/imgs/2022/07/17/cb88245089ef45d6.png)

```yml
tag_per_img：
  aplayer: https://xxxxxx.png
  android: ddddddd.png

category_per_img：
  随想: hdhdh.png
  推荐: ddjdjdjd.png
```

## 文章置顶

【推荐】*hexo-generator-index*从 2.0.0 开始，已经支持文章置顶功能。你可以直接在文章的**front-matter**区域里添加**sticky: 1**属性来把这篇文章置顶。数值越大，置顶的优先级越大。

## 文章封面

文章的markdown文档上,在Front-matter添加cover,并填上要显示的图片地址。
如果不配置cover,可以设置显示默认的cover.

如果不想在首页显示cover,可以设置为false

修改 主题配置文件

```yaml
cover:
  # 是否显示文章封面
  index_enable: true
  aside_enable: true
  archives_enable: true
  # 封面显示的位置
  # 三个值可配置 left , right , both
  position: both
  # 当没有设置cover时，默认的封面显示
  default_cover: 
```

当配置多张图片时,会随机选择一张作为cover.此时写法应为

```yml
default_cover:
  - https://fastly.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg.png
  - https://fastly.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg2.png
  - https://fastly.jsdelivr.net/gh/jerryc127/CDN@latest/cover/default_bg3.png
```

## 文章页相关配置

### 文章meta显示

这个选项是用来显示文章的相关信息的。修改 **主题配置文件**

```yml
post_meta:
  page:
    date_type: both # created or updated or both 主页文章日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 主页是否显示分类
    tags: true # true or false 主页是否显示标籤
    label: true # true or false 显示描述性文字
  post:
    date_type: both # created or updated or both 文章页日期是创建日或者更新日或都显示
    date_format: relative # date/relative 显示日期还是相对日期
    categories: true # true or false 文章页是否显示分类
    tags: true # true or false 文章页是否显示标籤
    label: true # true or false 显示描述性文字。
```

### 文章版权

为你的博客文章展示文章版权和许可协议。

修改 **主题配置文件**

```yml
post_copyright:
  enable: true
  decode: false
  author_href:
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```

由于Hexo 4.1开始，默认对网址进行解码，以至于如果是中文网址，会被解码，可设置decode: true来显示中文网址。

如果有文章（例如：转载文章）不需要显示版权，可以在文章Front-matter单独设置

```yml
copyright: false
```

从3.0.0开始，支持对单独文章设置版权信息，可以在文章Front-matter单独设置

```yml
copyright_author: xxxx
copyright_author_href: https://xxxxxx.com
copyright_url: https://xxxxxx.com
copyright_info: 此文章版权归xxxxx所有，如有转载，请註明来自原作者
```

### 文章打赏

在你每篇文章的结尾，可以添加打赏按钮。相关二维码可以自行配置。

对于没有提供二维码的，可配置一张软件的icon图片，然后在link上添加相应的打赏链接。用户点击图片就会跳转到链接去。

link可以不写，会默认为图片的链接。

修改 **主题配置文件**

```yml
reward:
  enable: true
  QR_code:
    - img: /img/wechat.jpg
      link:
      text: 微信
    - img: /img/alipay.jpg
      link:
      text: 支付宝
```

### TOC

在文章页，会有一个目录，用于显示TOC。

修改 **主题配置文件**

```yml
toc:
  post: true
  page: false
  number: true
  expand: false
  style_simple: false # for post
```

![image-20220717014916812](https://s3.bmp.ovh/imgs/2022/07/17/5f4fc1d8373bb85e.png)

#### 为特定的文章配置

在你的文章md文件的头部，加入`toc_number`和`toc`，并配置`true`或者`false`即可。

主题会优先判断文章Markdown的Front-matter是否有配置，如有，则以Front-matter的配置为准。否则，以主题配置文件中的配置为准

### 相关文章

相关文章推荐的原理是根据文章tags的比重来推荐

修改 **主题配置文件**

```yml
related_post:
  enable: true
  limit: 6 # 显示推荐文章数目
  date_type: created # or created or updated 文章日期显示创建日或者更新日
```

### 文章锚点

开启文章锚点后，当你在文章页进行滚动时，文章链接会根据标题ID进行替换
(注意: 每替换一次，会留下一个歷史记录。所以如果一篇文章有很多锚点的话，网页的歷史记录会很多。)

修改 主题配置文件

```yml
# anchor
# when you scroll in post , the url will update according to header id.
anchor: true
```

### 文章过期提醒

可设置是否显示文章过期提醒，以更新时间为基准。

```yml
# Displays outdated notice for a post (文章过期提醒)
noticeOutdate:
  enable: true
  style: flat # style: simple/flat
  limit_day: 365 # When will it be shown
  position: top # position: top/bottom
  message_prev: It has been
  message_next: days since the last update, the content of the article may be outdated.
```

- `limit_day`： 距离更新时间多少天才显示文章过期提醒
- `message_prev` ： 天数之前的文字
- `message_next`：天数之后的文字

### 文章分页按钮

可设置分页的逻辑，也可以关闭分页显示

```yml
# post_pagination (分页)
# value: 1 || 2 || false
# 1: The 'next post' will link to old post
# 2: The 'next post' will link to new post
# false: disable pagination
post_pagination: 1
```

- `post_pagination`: false	关闭分页按钮
- `post_pagination`: 1	下一篇显示的是旧文章
- `post_pagination`: 2	下一篇显示的是新文章

## 头像

修改 **主题配置文件**

```yml
avatar:
  img: /img/avatar.png
  effect: true # **头像会一直转圈**
```

## 图片描述

可开启图片Figcaption描述文字显示

优先显示图片的 title 属性，然后是 alt 属性

修改 **主题配置文件**

```yml
photofigcaption: true
```

## 复制相关配置

可配置网站是否可以复制、复制的内容是否添加版权信息

```yml
# copy settings
# copyright: Add the copyright information after copied content (复製的内容后面加上版权信息)
copy:
  enable: true
  copyright:
    enable: true
    limit_count: 50
```

1. `enable`	是否开启网站复製权限
2. `copyright`	复製的内容后面加上版权信息
3. `enable`	是否开启复製版权信息添加
4. `limit_count`	字数限制，当复制文字大于这个字数限制时，将在复制的内容后面加上版权信息

## Footer 设置

### 博客年份

since是一个来展示你站点起始时间的选项。它位于页面的最底部。

修改 **主题配置文件**

```YML
footer:
  owner:
    enable: true
    since: 2022
```

### 页脚自定义文本

`custom_text`是一个给你用来在页脚自定义文本的选项。通常你可以在这里写声明文本等。支持 HTML。

修改 **主题配置文件**

```YML
custom_text: Hi, welcome to my <a href="https://tryrun.gitee.io/">blog</a>!
```

对于部分人需要写 ICP 的，也可以写在 `custom_text`里

```yml
custom_text: <a href="icp链接"><img class="icp-icon" src="icp图片"><span>备案号：xxxxxx</span></a>
```

## 右下角按钮

### 简繁转换

简体繁体互换

右下角会有简繁转换按钮。

修改 **主题配置文件**

```yml
translate:
  enable: true
  # 默认按钮显示文字(网站是简体，应设置为'default: 繁')
  default: 简
  #网站默认语言，1: 繁体中文, 2: 简体中文
  defaultEncoding: 1
  #延迟时间,若不在前, 要设定延迟翻译时间, 如100表示100ms,默认为0
  translateDelay: 0
  #当文字是简体时，按钮显示的文字
  msgToTraditionalChinese: "繁"
  #当文字是繁体时，按钮显示的文字
  msgToSimplifiedChinese: "简"
```

### 夜间模式

右下角会有夜间模式按钮

修改 **主题配置文件**

```yml
# dark mode
darkmode:
  enable: true
  # dark mode和 light mode切换按钮
  button: true
  autoChangeMode: false
```

> V2.0.0 开始增加一个选项，可开启自动切换light mode 和 dark mode
>
> 1. autoChangeMode: 1 跟随系统而变化，不支持的浏览器/系统将按照时间晚上6点到早上6点之间切换为 dark mode
>
> 2. autoChangeMode: 2 只按照时间 晚上6点到早上6点之间切换为 dark mode,其余时间为light mode
>
> 3. autoChangeMode: false 取消自动切换
>

### 阅读模式

阅读模式下会去掉除文章外的内容，避免干扰閲读。

只会出现在文章页面，右下角会有閲读模式按钮。

修改 **主题配置文件**

```yml
readmode: true
```

### 按钮排序

```yaml
# Don't modify the following settings unless you know how they work (非必要请不要修改 )
# Choose: readmode,translate,darkmode,hideAside,toc,chat,comment
# Don't repeat 不要重复
rightside_item_order:
  enable: false
  hide: # readmode,translate,darkmode,hideAside
  show: # toc,chat,comment
```

注意： 不要重复

## 侧边栏设置

### 侧边排版

可自行决定哪个项目需要显示，可决定位置，也可以设置不显示侧边栏。

修改 **主题配置文件**

```yml
aside:
  enable: true
  hide: false
  button: true
  mobile: true # 手机页面（ 显示宽度 < 768px ）是否显示aside内容
  position: right # left or right
  display:
    archive: true
    tag: true
    category: true
  card_author:
    enable: true
    description:
    button:
      enable: true
	  icon: fab fa-github
      text: Github
      link: https://github.com/jerryc127/hexo-theme-butterfly
  card_announcement:
    enable: true
    content: This is my Blog
  card_recent_post:
    enable: true
    limit: 5 # if set 0 will show all
    sort: date # date or updated
  card_categories:
    enable: true
    limit: 8 # if set 0 will show all
    expand: none # none/true/false
  card_tags:
    enable: true
    limit: 40 # if set 0 will show all
    color: false
  card_archives:
    enable: true
    type: monthly # yearly or monthly
    format: MMMM YYYY # eg: YYYY年MM月
    order: -1 # Sort of order. 1, asc for ascending; -1, desc for descending
    limit: 8 # if set 0 will show all
  card_webinfo:
    enable: true
    post_count: true
    last_push_date: true
```

### 访问人数 busuanzi (UV 和 PV)

访问 busuanzi 的[官方网站](http://busuanzi.ibruce.info/)查看更多的介绍。

修改 **主题配置文件**

```yml
busuanzi:
  site_uv: true
  site_pv: true
  page_pv: true
```

### 运行时间

网页已运行时间

修改 **主题配置文件**

```yml
runtimeshow:
  enable: true
  publish_date: 6/7/2018 00:00:00  
  ##网页开通时间
  #格式: 月/日/年 时间
  #也可以写成 年/月/日 时间
```

### 最新评论

> 3.1.0 起支持
>
> 最新评论只会在刷新时才会去读取，并不会实时变化
>
> 由于 API 有 访问次数限制，为了避免调用太多，主题默认存取期限为 10 分钟。也就是说，调用后资料会存在 localStorage 里，10分钟内刷新网站只会去 localStorage 读取资料。 10 分钟期限一过，刷新页面时才会去调取 API 读取新的数据。（ 3.6.0 新增了 storage 配置，可自行配置缓存时间）
>

在侧边栏显示最新评论板块

修改 **主题配置文件**

```yml
# Aside widget - Newest Comments
newest_comments:
  enable: true
  sort_order: # Don't modify the setting unless you know how it works
  limit: 6
  storage: 10 # unit: mins, save data to localStorage
  avatar: true
```

- `limit`	显示的数量
- `storage`	设置缓存时间，单位 分钟
- `avatar`	是否显示头像

## 标签外挂（Tag Plugins）

> 标签外挂是Hexo独有的功能，并不是标准的Markdown格式。
>
> 以下的写法，只适用于Butterfly主题，用在其它主题上不会有效果，甚至可能会报错。使用前请留意

### Note (Bootstrap Callout)

1. 通用设置：

   1. 移植于next主题，并进行修改。

   2. 修改 **主题配置文件**

      ```yml
      note:
        # Note tag style values:
        #  - simple    bs-callout old alert style. Default.
        #  - modern    bs-callout new (v2-v3) alert style.
        #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
        #  - disabled  disable all CSS styles import of note tag.
        style: simple
        icons: false
        border_radius: 3
        # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
        # Offset also applied to label tag variables. This option can work with disabled note tag.
        light_bg_offset: 0
      ```

      `icons`和`light_bg_offset`只对方法一生效

2. 用法1：

```
{% note [class] [no-icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

- class	【可选】标识，不同的标识有不同的配色（ default / primary / success / info / warning / danger ）

- no-icon	【可选】不显示 icon

- style	【可选】可以覆盖配置中的 style（simple/modern/flat/disabled）

  

  ```simple
  {% note simple %}
  默认 提示块标籤
  {% endnote %}
  
  {% note default simple %}
  default 提示块标籤
  {% endnote %}
  
  {% note primary simple %}
  primary 提示块标籤
  {% endnote %}
  
  {% note success simple %}
  success 提示块标籤
  {% endnote %}
  
  {% note info simple %}
  info 提示块标籤
  {% endnote %}
  
  {% note warning simple %}
  warning 提示块标籤
  {% endnote %}
  
  {% note danger simple %}
  danger 提示块标籤
  {% endnote %}
  ```

  {% note simple %}
  默认 提示块标籤
  {% endnote %}

  {% note default simple %}
  default 提示块标籤
  {% endnote %}

  {% note primary simple %}
  primary 提示块标籤
  {% endnote %}

  {% note success simple %}
  success 提示块标籤
  {% endnote %}

  {% note info simple %}
  info 提示块标籤
  {% endnote %}

  {% note warning simple %}
  warning 提示块标籤
  {% endnote %}

  {% note danger simple %}
  danger 提示块标籤
  {% endnote %}

  ```modern
  {% note modern %}
  默认 提示块标籤
  {% endnote %}
  
  {% note default modern %}
  default 提示块标籤
  {% endnote %}
  
  {% note primary modern %}
  primary 提示块标籤
  {% endnote %}
  
  {% note success modern %}
  success 提示块标籤
  {% endnote %}
  
  {% note info modern %}
  info 提示块标籤
  {% endnote %}
  
  {% note warning modern %}
  warning 提示块标籤
  {% endnote %}
  
  {% note danger modern %}
  danger 提示块标籤
  {% endnote %}
  ```

  {% note modern %}
  默认 提示块标籤
  {% endnote %}

  {% note default modern %}
  default 提示块标籤
  {% endnote %}

  {% note primary modern %}
  primary 提示块标籤
  {% endnote %}

  {% note success modern %}
  success 提示块标籤
  {% endnote %}

  {% note info modern %}
  info 提示块标籤
  {% endnote %}

  {% note warning modern %}
  warning 提示块标籤
  {% endnote %}

  {% note danger modern %}
  danger 提示块标籤
  {% endnote %}

  ```flat
  {% note flat %}
  默认 提示块标籤
  {% endnote %}
  
  {% note default flat %}
  default 提示块标籤
  {% endnote %}
  
  {% note primary flat %}
  primary 提示块标籤
  {% endnote %}
  
  {% note success flat %}
  success 提示块标籤
  {% endnote %}
  
  {% note info flat %}
  info 提示块标籤
  {% endnote %}
  
  {% note warning flat %}
  warning 提示块标籤
  {% endnote %}
  
  {% note danger flat %}
  danger 提示块标籤
  {% endnote %}
  ```

  {% note flat %}
  默认 提示块标籤
  {% endnote %}

  {% note default flat %}
  default 提示块标籤
  {% endnote %}

  {% note primary flat %}
  primary 提示块标籤
  {% endnote %}

  {% note success flat %}
  success 提示块标籤
  {% endnote %}

  {% note info flat %}
  info 提示块标籤
  {% endnote %}

  {% note warning flat %}
  warning 提示块标籤
  {% endnote %}

  {% note danger flat %}
  danger 提示块标籤
  {% endnote %}

  ```disabled
  {% note disabled %}
  默认 提示块标籤
  {% endnote %}
  
  {% note default disabled %}
  default 提示块标籤
  {% endnote %}
  
  {% note primary disabled %}
  primary 提示块标籤
  {% endnote %}
  
  {% note success disabled %}
  success 提示块标籤
  {% endnote %}
  
  {% note info disabled %}
  info 提示块标籤
  {% endnote %}
  
  {% note warning disabled %}
  warning 提示块标籤
  {% endnote %}
  
  {% note danger disabled %}
  danger 提示块标籤
  {% endnote %}
  ```

  {% note disabled %}
  默认 提示块标籤
  {% endnote %}

  {% note default disabled %}
  default 提示块标籤
  {% endnote %}

  {% note primary disabled %}
  primary 提示块标籤
  {% endnote %}

  {% note success disabled %}
  success 提示块标籤
  {% endnote %}

  {% note info disabled %}
  info 提示块标籤
  {% endnote %}

  {% note warning disabled %}
  warning 提示块标籤
  {% endnote %}

  {% note danger disabled %}
  danger 提示块标籤
  {% endnote %}

  ```
  {% note no-icon %}
  默认 提示块标籤
  {% endnote %}
  
  {% note default no-icon %}
  default 提示块标籤
  {% endnote %}
  
  {% note primary no-icon %}
  primary 提示块标籤
  {% endnote %}
  
  {% note success no-icon %}
  success 提示块标籤
  {% endnote %}
  
  {% note info no-icon %}
  info 提示块标籤
  {% endnote %}
  
  {% note warning no-icon %}
  warning 提示块标籤
  {% endnote %}
  
  {% note danger no-icon %}
  danger 提示块标籤
  {% endnote %}
  ```

  {% note no-icon %}
  默认 提示块标籤
  {% endnote %}

  {% note default no-icon %}
  default 提示块标籤
  {% endnote %}

  {% note primary no-icon %}
  primary 提示块标籤
  {% endnote %}

  {% note success no-icon %}
  success 提示块标籤
  {% endnote %}

  {% note info no-icon %}
  info 提示块标籤
  {% endnote %}

  {% note warning no-icon %}
  warning 提示块标籤
  {% endnote %}

  {% note danger no-icon %}
  danger 提示块标籤
  {% endnote %}

3.用法2：

> 3.2.0 以上版本支持

```
{% note [color] [icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

- `color`	【可选】顔色(default / blue / pink / red / purple / orange / green)
- `icon`	【可选】可配置自定义 icon (只支持 fontawesome 图标, 也可以配置 no-icon )
- `style`	【可选】可以覆盖配置中的 style（simple/modern/flat/disabled）

```simple
{% note 'fab fa-cc-visa' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' simple %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' simple %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' simple%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' simple %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' simple %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' simple %}
		你是刷 Visa 还是 UnionPay
		{% endnote %}
		{% note blue 'fas fa-bullhorn' simple %}
		2021年快到了....
		{% endnote %}
		{% note pink 'fas fa-car-crash' simple %}
		小心开车 安全至上
		{% endnote %}
		{% note red 'fas fa-fan' simple%}
		这是三片呢？还是四片？
		{% endnote %}
		{% note orange 'fas fa-battery-half' simple %}
		你是刷 Visa 还是 UnionPay
		{% endnote %}
		{% note purple 'far fa-hand-scissors' simple %}
		剪刀石头布
		{% endnote %}
		{% note green 'fab fa-internet-explorer' simple %}
		前端最讨厌的浏览器
		{% endnote %}

```endnote
{% note 'fab fa-cc-visa' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' modern %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' modern %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' modern%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' modern %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' modern %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' modern %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note blue 'fas fa-bullhorn' modern %}

2021年快到了....

{% endnote %}

{% note pink 'fas fa-car-crash' modern %}

小心开车 安全至上

{% endnote %}

{% note red 'fas fa-fan' modern%}

这是三片呢？还是四片？

{% endnote %}

{% note orange 'fas fa-battery-half' modern %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note purple 'far fa-hand-scissors' modern %}

剪刀石头布

{% endnote %}

{% note green 'fab fa-internet-explorer' modern %}

前端最讨厌的浏览器

{% endnote %}

```flat
{% note 'fab fa-cc-visa' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' flat %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' flat %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' flat%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' flat %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' flat %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' flat %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note blue 'fas fa-bullhorn' flat %}

2021年快到了....

{% endnote %}

{% note pink 'fas fa-car-crash' flat %}

小心开车 安全至上

{% endnote %}

{% note red 'fas fa-fan' flat%}

这是三片呢？还是四片？

{% endnote %}

{% note orange 'fas fa-battery-half' flat %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note purple 'far fa-hand-scissors' flat %}

剪刀石头布

{% endnote %}

{% note green 'fab fa-internet-explorer' flat %}

前端最讨厌的浏览器

{% endnote %}

```disabled
{% note 'fab fa-cc-visa' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' disabled %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' disabled %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' disabled %}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' disabled %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' disabled %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' disabled %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note blue 'fas fa-bullhorn' disabled %}

2021年快到了....

{% endnote %}

{% note pink 'fas fa-car-crash' disabled %}

小心开车 安全至上

{% endnote %}

{% note red 'fas fa-fan' disabled %}

这是三片呢？还是四片？

{% endnote %}

{% note orange 'fas fa-battery-half' disabled %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note purple 'far fa-hand-scissors' disabled %}

剪刀石头布

{% endnote %}

{% note green 'fab fa-internet-explorer' disabled %}

前端最讨厌的浏览器

{% endnote %}

```no-icon
{% note no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue no-icon %}
2021年快到了....
{% endnote %}
{% note pink no-icon %}
小心开车 安全至上
{% endnote %}
{% note red no-icon %}
这是三片呢？还是四片？
{% endnote %}
{% note orange no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple no-icon %}
剪刀石头布
{% endnote %}
{% note green no-icon %}
前端最讨厌的浏览器
{% endnote %}
```

{% note no-icon %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note blue no-icon %}

2021年快到了....

{% endnote %}

{% note pink no-icon %}

小心开车 安全至上

{% endnote %}

{% note red no-icon %}

这是三片呢？还是四片？

{% endnote %}

{% note orange no-icon %}

你是刷 Visa 还是 UnionPay

{% endnote %}

{% note purple no-icon %}

剪刀石头布

{% endnote %}

{% note green no-icon %}

前端最讨厌的浏览器

{% endnote %}

### Gallery相册图库

> 一个图库集合。

写法：

```
<div class="gallery-group-main">
{% galleryGroup name description link img-url %}
{% galleryGroup name description link img-url %}
{% galleryGroup name description link img-url %}
</div>
```

- name：图库名字
- description：图库描述
- link：连接到对应相册的地址
- img-url：图库封面的地址

### Gallery相册

> 区别于旧版的Gallery相册,新的Gallery相册会自动根据图片长度进行排版，书写也更加方便，与markdown格式一样。可根据需要插入到相应的md。

写法：

```
{% gallery %}
markdown 图片格式
{% endgallery %}
```

### tag-hide

> 2.2.0以上提供
>
> 请注意，tag-hide内的标签外挂content内都不建议有h1 - h6 等标题。因为Toc会把隐藏内容标题也显示出来，而且当滚动屏幕时，如果隐藏内容没有显示出来，会导致Toc的滚动出现异常。

如果你想把一些文字、内容隐藏起来，并提供按钮让用户点击显示。可以使用这个标签外挂。

1. liline

   1. inline 在文本里面添加按钮隐藏内容，只限文字( content不能包含英文逗号，可用&sbquo;)

   2. 写法

      ```
      {% hideInline content,display,bg,color %}
      ```

      ​	  content: 文本内容

      ​     display: 按钮显示的文字(可选)

      ​     bg: 按钮的背景颜色(可选)

      ​     color: 按钮文字的颜色(可选)

   3. demo

      ```
      哪个英文字母最酷？ {% hideInline 因为西装裤(C装酷),查看答案,#FF7242,#fff %}
      
      门里站着一个人? {% hideInline 闪 %}
      ```

      哪个英文字母最酷？ {% hideInline 因为西装裤(C装酷),查看答案,#FF7242,#fff %}

      门里站着一个人? {% hideInline 闪 %}

2. block

   1. block独立的block隐藏内容，可以隐藏很多内容，包括图片，代码块等等

      ( display 不能包含英文逗号，可用&sbquo;)

   2. 写法

      ```
      {% hideBlock display,bg,color %}
      content
      {% endhideBlock %}
      ```

      content: 文本内容
      display: 按钮显示的文字(可选)
      bg: 按钮的背景颜色(可选)
      color: 按钮文字的颜色(可选)

   3. demo

      ```
      查看答案
      {% hideBlock 查看答案 %}
      傻子，怎么可能有答案
      {% endhideBlock %}
      ```

      查看答案
      {% hideBlock 查看答案 %}
      傻子，怎么可能有答案
      {% endhideBlock %}

3. toggle

   1. 如果你需要展示的内容太多，可以把它隐藏在收缩框里，需要时再把它展开。

      ( display 不能包含英文逗号，可用&sbquo;)

   2. 写法

      ```
      {% hideToggle display,bg,color %}
      content
      {% endhideToggle %}
      ```

   3. demo

      ```
      {% hideToggle Butterfly安装方法 %}
      在你的博客根目录里
      
      git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly
      
      如果想要安装比较新的dev分支，可以
      
      git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly
      
      {% endhideToggle %}
      ```

      {% hideToggle Butterfly安装方法 %}
      在你的博客根目录里

      git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

      如果想要安装比较新的dev分支，可以

      git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

      {% endhideToggle %}


### mermaid

使用mermaid标签可以做Flowchart（流程图）、Sequence diagram（时序图 ）、Class Diagram（类别图）、State Diagram（状态图）、Gantt（甘特图）和Pie Chart（圆形图），具体可以查看[mermaid文档](https://mermaid-js.github.io/mermaid/#/)

修改 **主题配置文件**

```yml
# mermaid
# see https://github.com/mermaid-js/mermaid
mermaid:
  enable: true
  # built-in themes: default/forest/dark/neutral
  theme:
    light: default
    dark: dark
```

写法：

```
{% mermaid %}
内容
{% endmermaid %}
```

例如：

```
{% mermaid %}
pie
    title Key elements in Product X
    "Calcium" : 42.96
    "Potassium" : 50.05
    "Magnesium" : 10.01
    "Iron" :  5
{% endmermaid %}
```

{% mermaid %}
pie
    title Key elements in Product X
    "Calcium" : 42.96
    "Potassium" : 50.05
    "Magnesium" : 10.01
    "Iron" :  5
{% endmermaid %}

### Tabs

使用方法

```
{% tabs Unique name, [index] %}
<!-- tab [Tab caption] [@icon] -->
Any content (support inline tags too).
<!-- endtab -->
{% endtabs %}

Unique name   : Unique name of tabs block tag without comma.
                Will be used in #id's as prefix for each tab with their index numbers.
                If there are whitespaces in name, for generate #id all whitespaces will replaced by dashes.
                Only for current url of post/page must be unique!
[index]       : Index number of active tab.
                If not specified, first tab (1) will be selected.
                If index is -1, no tab will be selected. It's will be something like spoiler.
                Optional parameter.
[Tab caption] : Caption of current tab.
                If not caption specified, unique name with tab index suffix will be used as caption of tab.
                If not caption specified, but specified icon, caption will empty.
                Optional parameter.
[@icon]       : FontAwesome icon name (full-name, look like 'fas fa-font')
                Can be specified with or without space; e.g. 'Tab caption @icon' similar to 'Tab caption@icon'.
                Optional parameter.
```

#### Demo 1 - 预设选择第一个【默认】

```
{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
```

{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

#### Demo 2 - 预设选择tabs

```
{% tabs test2, 3 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
```

{% tabs test2, 3 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

#### Demo 3 - 没有预设值

```
{% tabs test3, -1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
```

{% tabs test3, -1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

#### Demo 4 - 自定义Tab名 + 只有icon + icon和Tab名

```
{% tabs test4 %}
<!-- tab 第一个Tab -->
**tab名字为第一个Tab**
<!-- endtab -->

<!-- tab @fab fa-apple-pay -->
**只有图标 没有Tab名字**
<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->
**名字+icon**
<!-- endtab -->
{% endtabs %}
```

{% tabs test4 %}
<!-- tab 第一个Tab -->
**tab名字为第一个Tab**
<!-- endtab -->

<!-- tab @fab fa-apple-pay -->
**只有图标 没有Tab名字**
<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->
**名字+icon**
<!-- endtab -->
{% endtabs %}

### Button

使用方法：

```
{% btn [url],[text],[icon],[color] [style] [layout] [position] [size] %}

[url]         : 链接
[text]        : 按钮文字
[icon]        : [可选] 图标
[color]       : [可选] 按钮背景顔色(默认style时）
                      按钮字体和边框顔色(outline时)
                      default/blue/pink/red/purple/orange/green
[style]       : [可选] 按钮样式 默认实心
                      outline/留空
[layout]      : [可选] 按钮佈局 默认为line
                      block/留空
[position]    : [可选] 按钮位置 前提是设置了layout为block 默认为左边
                      center/right/留空
[size]        : [可选] 按钮大小
                      larger/留空
```

#### demo1

```
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
```

This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}

#### demo2

```
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block center larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block right outline larger %}
```

{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block center larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block right outline larger %}

#### demo3

```
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,green larger %}
```

{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,green larger %}

#### demo4

```
<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline green larger %}
</div>
```

<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline green larger %}
</div>
### inlineImg

主题中的图片都是默认以块级元素显示，如果你想以内联元素显示，可以使用这个标签外挂。

```
{% inlineImg [src] [height] %}
```

[src]      :    图片链接
       [height]   ：   图片高度限制【可选】

例子如下：

```
你看我长得漂亮不

![1](https://i.loli.net/2021/03/19/2P6ivUGsdaEXSFI.png)

我觉得很漂亮 {% inlineImg https://i.loli.net/2021/03/19/5M4jUB3ynq7ePgw.png 150px %}
![1](assets/2P6ivUGsdaEXSFI-16589295963483.png)
```

你看我长得漂亮不

![1](https://i.loli.net/2021/03/19/2P6ivUGsdaEXSFI.png)

我觉得很漂亮 {% inlineImg https://i.loli.net/2021/03/19/5M4jUB3ynq7ePgw.png 150px %}

### label

> 3.7.5 及以上版本适用

高亮所需的文字

```
{% label text color %}
```

text:	文字
      color:	【可选】背景颜色，默认为 default(default/blue/pink/red/purple/orange/green)

例子如下：

```
臣亮言：{% label 先帝 %}创业未半，而{% label 中道崩殂 blue %}。今天下三分，{% label 益州疲敝 pink %}，此诚{% label 危急存亡之秋 red %}也！然侍衞之臣，不懈于内；{% label 忠志之士 purple %}，忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气；不宜妄自菲薄，引喻失义，以塞忠谏之路也。
宫中、府中，俱为一体；陟罚臧否，不宜异同。若有{% label 作奸 orange %}、{% label 犯科 green %}，及为忠善者，宜付有司，论其刑赏，以昭陛下平明之治；不宜偏私，使内外异法也。
```

臣亮言：{% label 先帝 %}创业未半，而{% label 中道崩殂 blue %}。今天下三分，{% label 益州疲敝 pink %}，此诚{% label 危急存亡之秋 red %}也！然侍衞之臣，不懈于内；{% label 忠志之士 purple %}，忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气；不宜妄自菲薄，引喻失义，以塞忠谏之路也。
宫中、府中，俱为一体；陟罚臧否，不宜异同。若有{% label 作奸 orange %}、{% label 犯科 green %}，及为忠善者，宜付有司，论其刑赏，以昭陛下平明之治；不宜偏私，使内外异法也。

### timeline

> 4.0.0 以上支持

```
{% timeline title,color %}
<!-- timeline title -->
xxxxx
<!-- endtimeline -->
<!-- timeline title -->
xxxxx
<!-- endtimeline -->
{% endtimeline %}
```

title:	文字
      color:	【可选】背景颜色，默认为 default(default/blue/pink/red/purple/orange/green)

例子如下：

```
{% timeline 2022 %}
<!-- timeline 01-02 -->
这是测试页面
<!-- endtimeline -->
{% endtimeline %}
```

{% timeline 2022 %}
<!-- timeline 01-02 -->
这是测试页面
<!-- endtimeline -->
{% endtimeline %}

### flink

> 4.1.0 支持

可在任何界面插入类似友情链接列表效果

内容格式与友情链接界面一样，支持 yml 格式

```
{% flink %}
xxxxxx
{% endflink %}
```

比如：

```
{% flink %}
- class_name: 友情链接
  class_desc: 那些人，那些事
  link_list:
    - name: JerryC
      link: https://jerryc.me/
      avatar: https://jerryc.me/img/avatar.png
      descr: 今日事,今日毕
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
{% endflink %}
```

{% flink %}
- class_name: 友情链接
  class_desc: 那些人，那些事
  link_list:
    - name: JerryC
      link: https://jerryc.me/
      avatar: https://jerryc.me/img/avatar.png
      descr: 今日事,今日毕
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
      {% endflink %}
