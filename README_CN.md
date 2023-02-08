# postcss-px2vw-include-adapter

[English](README.md) | 中文

将px单位转换为视口单位的 (vw, vh, vmin, vmax) 的 [PostCSS](https://github.com/postcss/postcss) 插件.

## 简介

项目中使用postcss-px-to-viewport时，发现不支持include配置，当前项目只需要某几个文件需要适配，exclude写着很冗余，因此对postcss-px-to-viewport进行了适配，以支持include。

## 上手

### 安装
使用npm安装
```
$ npm install postcss-px2vw-include-adapter --save-dev
```
或者使用yarn进行安装
```
$ yarn add -D postcss-px2vw-include-adapter
```

### 配置参数

默认参数:
```js
{
  unitToConvert: 'px',
  viewportWidth: 320,
  unitPrecision: 5,
  propList: ['*'],
  viewportUnit: 'vw',
  fontViewportUnit: 'vw',
  selectorBlackList: [],
  minPixelValue: 1,
  mediaQuery: false,
  replace: true,
  exclude: undefined,
  include: undefined,
  landscape: false,
  landscapeUnit: 'vw',
  landscapeWidth: 568
}
```
- `unitToConvert` (String) 需要转换的单位，默认为"px"
- `viewportWidth` (Number) 设计稿的视口宽度
- `unitPrecision` (Number) 单位转换后保留的精度
- `propList` (Array) 能转化为vw的属性列表
    - 传入特定的CSS属性；
    - 可以传入通配符"*"去匹配所有属性，例如：['*']；
    - 在属性的前或后添加"*",可以匹配特定的属性. (例如['*position*'] 会匹配 background-position-y)
    - 在特定属性前加 "!"，将不转换该属性的单位 . 例如: ['*', '!letter-spacing']，将不转换letter-spacing
    - "!" 和 "*"可以组合使用， 例如: ['*', '!font*']，将不转换font-size以及font-weight等属性
- `viewportUnit` (String) 希望使用的视口单位
- `fontViewportUnit` (String) 字体使用的视口单位
- `selectorBlackList` (Array) 需要忽略的CSS选择器，不会转为视口单位，使用原有的px等单位。
    - 如果传入的值为字符串的话，只要选择器中含有传入值就会被匹配
        - 例如 `selectorBlackList` 为 `['body']` 的话， 那么 `.body-class` 就会被忽略
    - 如果传入的值为正则表达式的话，那么就会依据CSS选择器是否匹配该正则
        - 例如 `selectorBlackList` 为 `[/^body$/]` , 那么 `body` 会被忽略，而 `.body` 不会
- `minPixelValue` (Number) 设置最小的转换数值，如果为1的话，只有大于1的值会被转换
- `mediaQuery` (Boolean) 媒体查询里的单位是否需要转换单位
- `replace` (Boolean) 是否直接更换属性值，而不添加备用属性
- `exclude` (Array or Regexp) 忽略某些文件夹下的文件或特定文件，例如 'node_modules' 下的文件
    - 如果值是一个正则表达式，那么匹配这个正则的文件会被忽略
    - 如果传入的值是一个数组，那么数组里的值必须为正则
- `include` (Array or Regexp) 如果设置了`include`，那将只有匹配到的文件才会被转换，例如
  (`include: /src\\views\\px-to-vw-page/`)
    - 如果值是一个正则表达式，将包含匹配的文件，否则将排除该文件
    - 如果传入的值是一个数组，那么数组里的值必须为正则
- `landscape` (Boolean) 是否添加根据 `landscapeWidth` 生成的媒体查询条件 `@media (orientation: landscape)`
- `landscapeUnit` (String) 横屏时使用的单位
- `landscapeWidth` (Number) 横屏时使用的视口宽度

#### Ignoring (需要翻译帮助。)

You can use special comments for ignore conversion of single lines:
- `/* px-to-viewport-ignore-next */` — on a separate line, prevents conversion on the next line.
- `/* px-to-viewport-ignore */` — after the property on the right, prevents conversion on the same line.


#### 使用PostCss配置文件时

在`postcss.config.js`添加如下配置
```js
module.exports = {
  plugins: {
    // ...
    'postcss-px-to-viewport': {
      // options
    }
  }
}
```

#### 直接在gulp中使用，添加gulp-postcss

在 `gulpfile.js` 添加如下配置:
```js
var gulp = require('gulp');
var postcss = require('gulp-postcss');
var pxtoviewport = require('postcss-px-to-viewport');
gulp.task('css', function () {
    var processors = [
        pxtoviewport({
            viewportWidth: 320,
            viewportUnit: 'vmin'
        })
    ];
    return gulp.src(['build/css/**/*.css'])
        .pipe(postcss(processors))
        .pipe(gulp.dest('build/css'));
});
```

## 借鉴自

* 受 https://github.com/evrone/postcss-px-to-viewpor 启发有了这个项目