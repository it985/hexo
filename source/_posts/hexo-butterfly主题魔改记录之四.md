---
title: hexo-butterfly主题魔改记录之四
keywords: 'hexo,美化,教程'
top_img: img/hzw.webp
cover: img/hzw.webp
sticky: 4
categories:
  - hexo
tags:
  - hexo
abbrlink: 8244
date: 2022-07-18 02:43:52
updated: 2022-07-18 02:43:52
---

# hexo-butterfly主题魔改记录之四

## 评论

> 从3.0.0开始，开启评论需要在comments-use中填写你需要的评论。
>
> 支持双评论显示，只需要配置两个评论（第一个为默认显示）

1. 通用设置

   1. 修改配置

      ```
      comments:
        # Up to two comments system, the first will be shown as default
        # Choose: Disqus/Disqusjs/Livere/Gitalk/Valine/Waline/Utterances/Facebook Comments/Twikoo
        use: 
            # - Valine
            # - Disqus
        text: true # Display the comment name next to the button
        # lazyload: The comment system will be load when comment element enters the browser's viewport.
        # If you set it to true, the comment count will be invalid
        lazyload: true
        count: true # Display comment count in top_img
        card_post_count: false # Display comment count in Home Page
      ```

   2. 参数说明

      use：	使用的评论（请注意，最多支持两个，如果不需要请留空）
      注意：双评论不能是 Disqus 和 Disqusjs 一起，由于其共用同一个 ID，会出错
      text：	是否显示评论服务商的名字
      lazyload：	是否为评论开启lazyload，开启后，只有滚动到评论位置时才会加载评论所需要的资源（开启 lazyload 后，评论数将不显示）
      count：	是否在文章顶部显示评论数，livere、Giscus 和 utterances 不支持评论数显示
      card_post_count：	是否在首页文章卡片显示评论数，gitalk、livere 、Giscus 和 utterances 不支持评论数显示

2. 推荐

   1. Valine

      1. 遵循 [Valine](https://github.com/xCss/Valine)的指示去配置你的 LeanCloud 应用。以及查看相应的配置说明。

      2. 然后修改 主题配置文件

         ```yml
         valine:
           appId: # leancloud application app id
           appKey: # leancloud application app key
           avatar: monsterid # gravatar style https://valine.js.org/#/avatar
           serverURLs: # This configuration is suitable for domestic custom domain name users, overseas version will be automatically detected (no need to manually fill in)
           bg: # valine background
           visitor: false
           option:
         ```

      3. 自定义表情

         Valine于 v1.4.5 开始支持自定义表情，如果你需要自行配置，请在emojiCDN配置表情 CDN。

         同时在Hexo 工作目录下的source/_data/创建一个json文件valine.json,等同于 Valine 需要配置的emojiMaps，valine.json配置方式可参考如下

         ```json
         { 
         "tv_doge": "6ea59c827c414b4a2955fe79e0f6fd3dcd515e24.png",
         "tv_亲亲": "a8111ad55953ef5e3be3327ef94eb4a39d535d06.png",
         "tv_偷笑": "bb690d4107620f1c15cff29509db529a73aee261.png",
         "tv_再见": "180129b8ea851044ce71caf55cc8ce44bd4a4fc8.png",
         "tv_冷漠": "b9cbc755c2b3ee43be07ca13de84e5b699a3f101.png",
         "tv_发怒": "34ba3cd204d5b05fec70ce08fa9fa0dd612409ff.png",
         "tv_发财": "34db290afd2963723c6eb3c4560667db7253a21a.png",
         "tv_可爱": "9e55fd9b500ac4b96613539f1ce2f9499e314ed9.png",
         "tv_吐血": "09dd16a7aa59b77baa1155d47484409624470c77.png",
         "tv_呆": "fe1179ebaa191569b0d31cecafe7a2cd1c951c9d.png",
         "tv_呕吐": "9f996894a39e282ccf5e66856af49483f81870f3.png",
         "tv_困": "241ee304e44c0af029adceb294399391e4737ef2.png",
         "tv_坏笑": "1f0b87f731a671079842116e0991c91c2c88645a.png",
         "tv_大佬": "093c1e2c490161aca397afc45573c877cdead616.png",
         "tv_大哭": "23269aeb35f99daee28dda129676f6e9ea87934f.png",
         "tv_委屈": "d04dba7b5465779e9755d2ab6f0a897b9b33bb77.png",
         "tv_害羞": "a37683fb5642fa3ddfc7f4e5525fd13e42a2bdb1.png",
         "tv_尴尬": "7cfa62dafc59798a3d3fb262d421eeeff166cfa4.png",
         "tv_微笑": "70dc5c7b56f93eb61bddba11e28fb1d18fddcd4c.png",
         "tv_思考": "90cf159733e558137ed20aa04d09964436f618a1.png",
         "tv_惊吓": "0d15c7e2ee58e935adc6a7193ee042388adc22af.png"
         } 
         ```

   2. Waline 

      Waline - 一款从 Valine 衍生的带后端评论系统。可以将 Waline 等价成 With backend Valine。

      具体配置可参考 [waline 文档](https://waline.js.org/)

      然后修改 **主题配置文件**

      ```yml
      waline:
        serverURL: # Waline server address url
        bg: # waline background
        pageview: false
        option:
      ```

   3. Twikoo 

      Twikoo 是一个简洁、安全、无后端的静态网站评论系统，基于腾讯云开发。

      具体如何配置评论，请查看 [Twikoo文档](https://twikoo.js.org/quick-start.html#%E7%8E%AF%E5%A2%83%E5%88%9D%E5%A7%8B%E5%8C%96)

      你只需要把获取到的 *环境ID (envId)* 填写到配置上去就行

      修改 **主题配置文件**

      ```yml
      twikoo:
        envId:
        region:
        visitor: false
        option:
      ```

      envId	环境 ID
      region	环境地域，默认为 ap-shanghai，如果您的环境地域不是上海，需传此参数
      visitor	是否显示文章閲读数
      option	可选配置

## 搜索系统

> 记得运行 hexo clean

1. algolia

   1. 你需要安装 [hexo-algolia](https://github.com/oncletom/hexo-algolia)或 [hexo-algoliasearch](https://github.com/LouisBarranqueiro/hexo-algoliasearch). 根据它们的説明文档去做相应的配置。

   2. 修改 主题配置文件

      ```yml
      algolia_search:
        enable: true
        hits:
          per_page: 6
      ```

2. 本地搜索

   1. 你需要安装 [hexo-generator-search](https://github.com/PaicHyperionDev/hexo-generator-search)，根据它的文档去做相应配置

   2. 修改 主题配置文件

      ```yml
      local_search:
        enable: false
        preload: false
        CDN:
      #enable	是否开启本地搜索
      #preload	预加载，开启后，进入网页后会自动加载搜索文件。关闭时，只有点击搜索按钮后，才会加载搜索文件
      #CDN	搜索文件的 CDN 地址（默认使用的本地链接）
      ```

## Math 数学

### mathjax

> 不要在标题里使用 mathjax 语法，toc 目录不一定能正确显示 mathjax，可能显示 mathjax 代码
>
> 建议使用 KaTex 获得更好的效果，下文有介绍！

修改 主题配置文件:

```yml
mathjax:
  enable: true
  # true 表示每一页都加载mathjax.js
  # false 需要时加载，须在使用的Markdown Front-matter 加上 mathjax: true
  per_page: false
```

> 如果 per_page 设为 true,则每一页都会加载 Mathjax 服务。设为 false，则需要在文章 Front-matter 添加 mathjax: true,对应的文章才会加载 Mathjax 服务。

然后你需要修改一下默认的 markdown 渲染引擎来实现 MathJax 的效果。

以下操作在你 hexo 博客的目录下 (不是 Butterfly 的目录):

安装插件

```sh
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

配置

```yml
kramed:
  gfm: true
  pedantic: false
  sanitize: false
  tables: true
  breaks: true
  smartLists: true
  smartypants: true
```

### KaTeX

> 不要在标题里使用 KaTeX 语法，toc 目录不能正确显示 KaTeX。
>

首先禁用MathJax（如果你配置过 MathJax 的话），然后修改你的主题配置文件以便加载katex.min.css:

```yml
katex:
  enable: true
  # true 表示每一页都加载katex.js
  # false 需要时加载，须在使用的Markdown Front-matter 加上 katex: true
  per_page: false
  hide_scrollbar: true
```

你不需要添加 katex.min.js 来渲染数学方程。相应的你需要卸载你之前的 hexo 的 markdown 渲染器，然后安装其它插件。

1. hexo-renderer-markdown-it(建议使用)

   1. 卸载掉 marked 插件，安装 [hexo-renderer-markdown-it](https://github.com/hexojs/hexo-renderer-markdown-it)

      ```
      npm un hexo-renderer-marked --save # 如果有安装这个的话，卸载
      npm un hexo-renderer-kramed --save # 如果有安装这个的话，卸载
      
      npm i hexo-renderer-markdown-it --save # 需要安装这个渲染插件
      npm install @neilsustc/markdown-it-katex --save #需要安装这个katex插件
      ```

   2. 在 hexo 的**根目录**的 *_config.yml* 中配置

      ```
      markdown:
        plugins:
          - plugin:
            name: '@neilsustc/markdown-it-katex'
            options:
              strict: false
      ```

2. hexo-renderer-markdown-it-plus

> 注意，此方法生成的 katex 没有斜体

卸载掉 marked 插件，然后安装新的hexo-renderer-markdown-it-plus:

```
# 替换 `hexo-renderer-kramed` 或者 `hexo-renderer-marked` 等hexo的markdown渲染器
# 你可以在你的package.json里找到hexo的markdwon渲染器，并将其卸载
npm un hexo-renderer-marked --save
# or
npm un hexo-renderer-kramed --save
# 然后安装 `hexo-renderer-markdown-it-plus`
npm i @upupming/hexo-renderer-markdown-it-plus --save
```

注意到 `hexo-renderer-markdown-it-plus`已经无人持续维护, 所以我们使用 `@upupming/hexo-renderer-markdown-it-plus`。 这份 fork 的代码使用了 `@neilsustc/markdown-it-katex`同时它也是 VSCode 的插件 [Markdown All in One](https://github.com/yzhang-gh/vscode-markdown)所使用的, 所以我们可以获得最新的 KaTex 功能例如 `\tag{}`。

你还可以通过 `@neilsustc/markdown-it-katex`控制 KaTeX 的设置，所有可配置的选项参见 https://katex.org/docs/options.html。 比如你想要禁用掉 KaTeX 在命令行上输出的宂长的警告信息，你可以在根目录的 *_config.yml* 中使用下面的配置将 `strict` 设置为 false

```yml
markdown_it_plus:
  plugins:
    - plugin:
      name: '@neilsustc/markdown-it-katex'
      enable: true
      options:
        strict: false
```

## 美化/特效

### 自定义主题色

可以修改大部分UI颜色

修改 **主题配置文件**，比如：

> 颜色值必须被双引号包裹，就像"#000"而不是#000。否则将会在构建的时候报错！

```yml
theme_color:
  enable: true
  main: "#49B1F5"
  paginator: "#00c4b6"
  button_hover: "#FF7242"
  text_selection: "#00c4b6"
  link_color: "#99a9bf"
  meta_color: "#858585"
  hr_color: "#A4D8FA"
  code_foreground: "#F47466"
  code_background: "rgba(27, 31, 35, .05)"
  toc_color: "#00c4b6"
  blockquote_padding_color: "#49b1f5"
  blockquote_background_color: "#49b1f5"
  scrollbar_color: "#49b1f5"
```

### 网站背景

默认显示白色，可设置图片或者颜色

修改 **主题配置文件**

```
# 图片格式 url(http://xxxxxx.com/xxx.jpg)
# 颜色（HEX值/RGB值/顔色单词/渐变色)
# 留空 不显示背景
background:
```

留意: 如果你的网站根目录不是'/',使用本地图片时，需加上你的根目录。
例如：网站是 `https://yoursite.com/blog`,引用一张`img/xx.png`图片，则设置background为 `url(/blog/img/xx.png)`

### footer 背景

修改 **主题配置文件**

```YAML
# footer是否显示图片背景(与top_img一致)
footer_bg: true
```

留空/false													显示默认的顔色
		img链接													图片的链接，显示所配置的图片
		顔色(HEX值,RGB值 ,顔色单词,渐变色	对应的顔色
		true															显示跟 top_img 一样

### 打字效果

修改 **主题配置文件**

```YML
# Typewriter Effect (打字效果)
# https://github.com/disjukr/activate-power-mode
activate_power_mode:
  enable: true
  colorful: true # open particle animation (冒光特效)
  shake: true #  open shake (抖动特效)
  mobile: false
```

### 背景特效

#### 静止彩带

```yml
canvas_ribbon:
  enable: false
  size: 150
  alpha: 0.6
  zIndex: -1
  click_to_change: false  #设置是否每次点击都更换綵带
  mobile: false # false 手机端不显示 true 手机端显示
```

#### 动态彩带

```YML
canvas_fluttering_ribbon:
  enable: true
  mobile: false # false 手机端不显示 true 手机端显示
```

#### canvas-nest

```YML
canvas_nest:
  enable: true
  color: '0,0,255' #color of lines, default: '0,0,0'; RGB values: (R,G,B).(note: use ',' to separate.)
  opacity: 0.7 # the opacity of line (0~1), default: 0.5.
  zIndex: -1 # z-index property of the background, default: -1.
  count: 99 # the number of lines, default: 99.
  mobile: false # false 手机端不显示 true 手机端显示
```

### 鼠标点击效果

#### 烟火

zIndex建议只在-1和9999上选
		-1 代表烟火效果在底部
		9999 代表烟火效果在前面

```yml
fireworks:
  enable: true
  zIndex: 9999 # -1 or 9999
  mobile: false
```

#### 爱心

```YML
# 点击出现爱心
click_heart:
  enable: true
  mobile: false
```

#### 文字

```YML
# 点击出现文字，文字可自行修改
ClickShowText:
  enable: false
  text:
    - I
    - LOVE
    - YOU
  fontSize: 15px
  random: false # 文字随机显示
  mobile: false
```

### 页面美化

会改变ol、ul、h1-h5的样式

field配置生效的区域

- post 只在文章页生效
- site 在全站生效

```yml
# 美化页面显示
beautify:
  enable: true
  field: site # site/post
  title-prefix-icon: '\f0c1'
  title-prefix-icon-color: "#F47466"
```

`title-prefix-icon`填写的是`fontawesome`的icon的Unicode数。

### 自定义字体和字体大小

#### 全局字体

可自行设置字体的`font-family`。如不需要配置，请留空

```yml
# Global font settings
# Don't modify the following settings unless you know how they work (非必要不要修改)
font:
  global-font-size:
  code-font-size:
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Lato, Roboto, "PingFang SC", "Microsoft JhengHei", "Microsoft YaHei", sans-serif
  code-font-family: consolas, Menlo, "PingFang SC", "Microsoft JhengHei", "Microsoft YaHei", sans-serif
```

#### Blog 标题字体

可自行设置字体的`font-family`。如不需要配置，请留空。如不需要使用网络字体，只需要把`font_link`留空就行

```yml
# Font settings for the site title and site subtitle
# 左上角网站名字 主页居中网站名字
blog_title_font:
  font_link: https://fonts.googleapis.com/css?family=Titillium+Web&display=swap
  font-family: Titillium Web, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft JhengHei', 'Microsoft YaHei', sans-serif
```

### 网站副标题

可设置主页中显示的网站副标题或者喜欢的座右铭。

```
# 主页subtitle
subtitle:
  enable: false
  # Typewriter Effect (打字效果)
  effect: true
  # loop (循环打字)
  loop: true
  # source 调用第三方服务
  # source: false 关闭调用
  # source: 1  调用一言网的一句话（简体） https://hitokoto.cn/
  # source: 2  调用一句网（简体） http://yijuzhan.com/
  # source: 3  调用今日诗词（简体） https://www.jinrishici.com/
  # subtitle 会先显示 source , 再显示 sub 的内容
  source: false
  # 如果关闭打字效果，subtitle 只会显示 sub 的第一行文字
  sub:
    - 今日事&#44;今日毕
    - Never put off till tomorrow what you can do today
```

### 主页top_img显示大小

> 适用于 版本号 >= V1.2.0

默认的显示为全屏。site-info的区域会居中显示

```YML
# 主页设置
# 默认top_img全屏，site_info在中间
# 使用默认, 都无需填写（建议默认）
index_site_info_top: # 主页标题距离顶部距离  例如 300px/300em/300rem/10%
index_top_img_height:  #主页top_img高度 例如 300px/300em/300rem  不能使用百分比
```

**注意**：index_top_img_height的值不能使用百分比。2个都不填的话，会使用默认值

### 页面加载动画preloader

当进入网页时，因为加载速度的问题，可能会导致top_img图片出现断层显示，或者网页加载不全而出现等待时间，开启preloader后，会显示加载动画，等页面加载完，加载动画会消失。

```YML
# 加载动画 Loading Animation
preloader: true
```

### 字数统计

要为Butterfly配上字数统计特性, 你需要如下几个步骤:

1. 打开 hexo 工作目录

2. `npm install hexo-wordcount --save` or `yarn add hexo-wordcount`

3. 修改 **主题配置文件**

   ```yml
   wordcount:
     enable: true
     post_wordcount: true
     min2read: true
     total_wordcount: true
   ```

### Pjax

当用户点击链接，通过ajax更新页面需要变化的部分，然后使用HTML5的pushState修改浏览器的URL地址。

这样可以不用重复加载相同的资源（css/js）， 从而提升网页的加载速度。

```yml
# Pjax [Beta]
# It may contain bugs and unstable, give feedback when you find the bugs.
# https://github.com/MoOx/pjax
pjax: 
  enable: true
  exclude:
    - /music/
    - /no-pjax/
```

> 对于一些第三方插件，有些并不支持 pjax 。
> 你可以把网页加入到 exclude 里，这个网页会被 pjax 排除在外。
> 点击该网页会重新加载网站
>
> 使用pjax后，一些自己DIY的js可能会无效，跳转页面时需要重新调用，请参考[Pjax文档](https://github.com/MoOx/pjax)
> 使用pjax后，一些个别页面加载的js/css，将会改为所有页面都加载

### Inject

> 2.3.0以上支持

如想添加额外的js/css/meta等等东西，可以在Inject里添加，支持添加到head(</body>标籤之前)和bottom(</html>标籤之前)。

请注意：以标准的html格式添加内容

```yml
inject:
  head:
  	- <link rel="stylesheet" href="/self.css">
  bottom:
  	- <script src="xxxx"></script>
```

留意: 如果你的网站根目录不是'/',使用本地图片时，需加上你的根目录。
例如：网站是 `https://yoursite.com/blog`,引用`css/xx.css`，则设置为`<link rel="stylesheet" href="/blog/css/xx.css">`

### CDN

配置文件中最后一部分CDN，里面是主题所引用到的文件，可自行配置CDN。（非必要请勿修改，配置后请确认链接是否能访问）

```yml
CDN:
  # The CDN provider of internal scripts (主题内部 js 的 cdn 配置)
  # option: local/jsdelivr/unpkg/cdnjs/custom
  # Dev version can only choose. ( dev版的主题只能设置为 local )
  internal_provider: local

  # The CDN provider of third party scripts (第三方 js 的 cdn 配置)
  # option: local/jsdelivr/unpkg/cdnjs/custom
  # when set it to local, you need to install hexo-butterfly-extjs
  third_party_provider: jsdelivr

  # Add version number to CDN, true or false  
  version: false

  # Custom format
  # For example: https://cdn.staticfile.org/${cdnjs_name}/${version}/${min_cdnjs_file}
  custom_format:

  option:
    # main_css:
    # main:
    # utils:
    # translate:
    # local_search:
    # algolia_js:
    # algolia_search_v4:
    # instantsearch_v4:
    # pjax:
    # gitalk:
    # gitalk_css:
    # blueimp_md5:
    # valine:
    # disqusjs:
    # disqusjs_css:
    # twikoo:
    # waline_js:
    # waline_css:
    # sharejs:
    # sharejs_css:
    # mathjax:
    # katex:
    # katex_copytex:
    # mermaid:
    # canvas_ribbon:
    # canvas_fluttering_ribbon:
    # canvas_nest:
    # lazyload:
    # instantpage:
    # typed:
    # pangu:
    # fancybox_css_v4:
    # fancybox_v4:
    # medium_zoom:
    # snackbar_css:
    # snackbar:
    # activate_power_mode:
    # fireworks:
    # click_heart:
    # ClickShowText:
    # fontawesomeV6:
    # flickr_justified_gallery_js:
    # flickr_justified_gallery_css:
    # aplayer_css:
    # aplayer_js:
    # meting_js:
    # prismjs_js:
    # prismjs_lineNumber_js:
    # prismjs_autoloader:
```

| 参数                 | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| internal_provider    | 主题内部文件<br/>可选 local/jsdelivr/unpkg/cdnjs/custom<br/>lcoal 为本地加载，custom 为自定义格式，需配置 custom_format<br/>注意: 如果使用的是 Dev 版，只能设置为 local |
| third_party_provider | 第三方文件<br/>可选 local/jsdelivr/unpkg/cdnjs/custom<br/>lcoal 为本地加载，custom 为自定义格式，需配置 custom_format<br/>注意: 如果你选择 local 需要安装 `hexo-butterfly-extjs`插件 |
| version              | true/false 为 cdn 加上指定版本号                             |
| custom_format        | 自定义格式                                                   |
| option               | 你可以在这里更换部分文件,会覆盖原有的配置                    |

#### version

如需修改版本号，可修改**主题目录**的 'plugins.yml' 中对应插件的 version

请确保你修改的版本号，你所使用的 cdn 有收录

#### custom_format

| 参数           | 解释                               |
| -------------- | ---------------------------------- |
| name           | npm 上的包名                       |
| file           | npm 上的文件路径                   |
| min_file       | npm 上的文件路径（压缩过的文件）   |
| cdnjs_name     | cdnjs 上的包名                     |
| cdnjs_file     | cdnjs 上的文件路径                 |
| min_cdnjs_file | cdnjs 上的文件路径（压缩过的文件） |
| version        | 插件版本号                         |

部分可用的第三方 CDN 列表

| 提供商               | 格式                                                         | 备註       |
| -------------------- | ------------------------------------------------------------ | ---------- |
| Staticfile（七牛云） | https://cdn.staticfile.org/${cdnjs_name}/${version}/${min_cdnjs_file} | 同步 cdnjs |
| BootCDN              | https://cdn.bootcdn.net/ajax/libs/${cdnjs_name}/${version}/${min_cdnjs_file} | 同步 cdnjs |
| Baomitu（360）       | 最新版本： https://lib.baomitu.com/${cdnjs_name}/latest/${min_cdnjs_file}<br/>指定版本： https://lib.baomitu.com/${cdnjs_name}/${version}/${min_cdnjs_file} | 同步 cdnjs |
| Elemecdn             | 最新版本： https://npm.elemecdn.com/${name}@latest/${file}<br/>指定版本： https://npm.elemecdn.com/${name}@${version}/${file} | 同步 npm   |






