---
title: hexo-butterfly主题魔改记录之一
keywords: 'hexo,美化,教程'
top_img: img/hzw.webp
cover: img/hzw.webp
sticky: 1
categories:
  - hexo
tags:
  - hexo
abbrlink: 56124
date: 2022-07-16 21:57:21
updated: 2022-07-16 21:57:21
---

# hexo-butterfly主题魔改记录之一

## Windows10安装Hexo教程

>   Hexo是一个快速、简洁且高效的博客框架，本文笔者带大家安装Hexo+gitee。

### 安装前提

- Node.js:https://nodejs.org/
- Git:https://git-scm.com/

### 安装Hexo

```javascript
npm install -g hexo-cli
hexo init <> //<>内填Hexo初始化目录，会自动新建目录。
cd <> //进入刚刚初始化的新目录
npm install //进行安装
```

### 启动Hexo

完成了安装Hexo步骤之后，便可以在本地启动Hexo了。

```javascript
hexo clean //清除缓存
hexo g //生成静态文件
hexo s //在本地部署Hexo
```

执行Hexo s之后，Hexo便会在本地运行，浏览器输入127.0.0.1:4000便可看到效果。

### 部署Hexo到Github，Gitee或Coding

> 如果出现`ERROR Deployer not found: git`，请执行`npm install hexo-deployer-git --save`

如需部署hexo到Github或Coding，需要在Hexo根目录的**_config.yml**文件内填写Github的仓库地址，并在Git中配置用户名及邮箱地址。

```javascript
#Hexo执行清除缓存，生成静态文件，以及推送到Github。
Hexo clean&&hexo g&&hexo d
```

## 安装butterfly

### 安装

1. Git安装(github)

   1. 稳定版：在hexo根目录

      ```shell
      git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
      ```

   2. 测试版：在hexo根目录

      > 测试版可能存在 bug，追求稳定的请安装稳定版

      ```sh
      git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
      ```

   3. 升级:  在hexo根目录下，运行 `git pull`

2. Git安装(gitee)

   1. 稳定版【建议】 ：在hexo根目录

      ```sh
      git clone -b master https://gitee.com/immyw/hexo-theme-butterfly.git themes/butterfly
      ```

   2. 测试版 ：在hexo根目录

      > 测试版可能存在Bugs，追求稳定的请安装稳定版

      ```
      git clone -b dev https://gitee.com/immyw/hexo-theme-butterfly.git themes/butterfly
      ```

   3. 升级 ：在hexo根目录下，运行`git pull`

3. npm安装

   > 此方法只支持 Hexo 5.0.0 以上版本
   >
   > 通过 npm 安装并不会在 themes 里生成主题文件夹，而是在 node_modules 里生成
   >

   在你的 Hexo 根目录里`npm i hexo-theme-butterfly`
   升级方法：在 Hexo 根目录下，运行 `npm update hexo-theme-butterfly`

### 应用主题

修改 Hexo 根目录下的 _config.yml，把主题改为butterfly

```yaml
# theme: landscape
theme: butterfly
```

### 安装插件

如果你没有 pug 以及 stylus 的渲染器，请下载安装

```sh
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 升级建议

为了减少升级主题后带来的不便，请使用以下方法（建议，可以不做）。

在 hexo 的根目录创建一个文件 `_config.butterfly.yml`，并把主题目录的 `_config.yml` 内容复制到 `_config.butterfly.yml` 去。( 注意: **复制**的是***主题***的 _config.yml ,而不是 `hexo` 的 `_config.yml`)

> 注意： 不要把主题目录的 _config.yml 删掉
>
> 注意： 以后只需要在 _config.butterfly.yml进行配置就行。
> 如果使用了 _config.butterfly.yml， 配置主题的 _config.yml 将不会有效果。

Hexo会自动合并主题中的_config.yml和 _config.butterfly.yml里的配置，如果存在同名配置，会使用_config.butterfly.yml的配置，其优先度较高。

### 安装gulp

1、安装gulp

```sh
npm install -g gulp-cli 
npm install gulp --save-dev
```

2、安装gulp模块

```sh
# 压缩html
npm install gulp-htmlclean --save-dev
npm install gulp-html-minifier-terser --save-dev
# 压缩css
npm install gulp-clean-css --save-dev
#压缩js
npm install --save-dev gulp-uglify
npm install --save-dev gulp-babel @babel/core @babel/preset-env
npm install gulp-terser --save-dev
#压缩字体，仅支持ttf
npm install gulp-fontmin --save-dev
```

3、在hexo目录创建gulpfile.js

```js
//用到的各个插件
var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');
var htmlmin = require('gulp-html-minifier-terser');
var htmlclean = require('gulp-htmlclean');
var fontmin = require('gulp-fontmin');
// gulp-tester
var terser = require('gulp-terser');
// 压缩js
gulp.task('compress', () =>
  gulp.src(['./public/**/*.js', '!./public/**/*.min.js'])
    .pipe(terser())
    .pipe(gulp.dest('./public'))
)
//压缩css
gulp.task('minify-css', () => {
    return gulp.src(['./public/**/*.css'])
        .pipe(cleanCSS({
            compatibility: 'ie11'
        }))
        .pipe(gulp.dest('./public'));
});
//压缩html
gulp.task('minify-html', () => {
    return gulp.src('./public/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true, //清除html注释
            collapseWhitespace: true, //压缩html
            collapseBooleanAttributes: true,
            //省略布尔属性的值，例如：<input checked="true"/> ==> <input />
            removeEmptyAttributes: true,
            //删除所有空格作属性值，例如：<input id="" /> ==> <input />
            removeScriptTypeAttributes: true,
            //删除<script>的type="text/javascript"
            removeStyleLinkTypeAttributes: true,
            //删除<style>和<link>的 type="text/css"
            minifyJS: true, //压缩页面 JS
            minifyCSS: true, //压缩页面 CSS
            minifyURLs: true  //压缩页面URL
        }))
        .pipe(gulp.dest('./public'))
});
//压缩字体
function minifyFont(text, cb) {
  gulp
    .src('./public/fonts/*.ttf') //原字体所在目录
    .pipe(fontmin({
      text: text
    }))
    .pipe(gulp.dest('./public/fontsdest/')) //压缩后的输出目录
    .on('end', cb);
}

gulp.task('mini-font', (cb) => {
  var buffers = [];
  gulp
    .src(['./public/**/*.html']) //HTML文件所在目录请根据自身情况修改
    .on('data', function(file) {
      buffers.push(file.contents);
    })
    .on('end', function() {
      var text = Buffer.concat(buffers).toString('utf-8');
      minifyFont(text, cb);
    });
});
// 运行gulp命令时依次执行以下任务
gulp.task('default', gulp.parallel(
  'compress', 'minify-css', 'minify-html','mini-font'
))
```

