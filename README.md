# postcss-px2vw-include-adapter

English | [中文](README_CN.md)

A plugin for [PostCSS](https://github.com/postcss/postcss) that generates viewport units (vw, vh, vmin, vmax) from pixel units.

## Demo

When postcss-px-to-viewport is used in a project, the include configuration is not supported. The current project only requires the adaptation of certain files, and exclude is redundant. Therefore, postcss-px-to-viewport is adopted to support include.


## Getting Started

### Installation
Add via npm
```
$ npm install postcss-px2vw-include-adapter --save-dev
```
or yarn
```
$ yarn add -D postcss-px2vw-include-adapter
```

### Usage

Default Options:
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
- `unitToConvert` (String) unit to convert, by default, it is px.
- `viewportWidth` (Number) The width of the viewport.
- `unitPrecision` (Number) The decimal numbers to allow the vw units to grow to.
- `propList` (Array) The properties that can change from px to vw.
    - Values need to be exact matches.
    - Use wildcard * to enable all properties. Example: ['*']
    - Use * at the start or end of a word. (['*position*'] will match background-position-y)
    - Use ! to not match a property. Example: ['*', '!letter-spacing']
    - Combine the "not" prefix with the other prefixes. Example: ['*', '!font*']
- `viewportUnit` (String) Expected units.
- `fontViewportUnit` (String) Expected units for font.
- `selectorBlackList` (Array) The selectors to ignore and leave as px.
    - If value is string, it checks to see if selector contains the string.
        - `['body']` will match `.body-class`
    - If value is regexp, it checks to see if the selector matches the regexp.
        - `[/^body$/]` will match `body` but not `.body`
- `minPixelValue` (Number) Set the minimum pixel value to replace.
- `mediaQuery` (Boolean) Allow px to be converted in media queries.
- `replace` (Boolean) replaces rules containing vw instead of adding fallbacks.
- `exclude` (Regexp or Array of Regexp) Ignore some files like 'node_modules'
    - If value is regexp, will ignore the matches files.
    - If value is array, the elements of the array are regexp.
- `include` (Regexp or Array of Regexp) If `include` is set, only matching files will be converted,
  for example (`include: /src\\views\\px-to-vw-page/`)
    - If the value is regexp, the matching file will be included, otherwise it will be excluded.
    - If value is array, the elements of the array are regexp.
- `landscape` (Boolean) Adds `@media (orientation: landscape)` with values converted via `landscapeWidth`.
- `landscapeUnit` (String) Expected unit for `landscape` option
- `landscapeWidth` (Number) Viewport width for landscape orientation.

#### Use with PostCss configuration file

add to your `postcss.config.js`
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

#### Use with gulp-postcss

add to your `gulpfile.js`:
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

## From

* inspired by https://github.com/evrone/postcss-px-to-viewpor with this project
