目的： 保证代码的可读性，可维护性，可扩展性, 可复用性 <br/>
# 1. 目录结构的规范
## 1.1 基本目录结构
```
  stylesheets/
  |
  |-- modules/              # Common modules
  |   |-- _all.scss         # Include to get all modules
  |   |-- _utility.scss     # Module name
  |   |-- _colors.scss      # Etc...
  |   ...
  |
  |-- partials/             # Partials
  |   |-- _base.sass        # imports for all mixins + global project variables
  |   |-- _buttons.scss     # buttons
  |   |-- _figures.scss     # figures
  |   |-- _grids.scss       # grids
  |   |-- _typography.scss  # typography
  |   |-- _reset.scss       # reset
  |   ...
  |
  |-- vendor/               # CSS or Sass from other projects
  |   |-- _colorpicker.scss
  |   |-- _jquery.ui.core.scss
  |   ...
  |
  `-- main.scss            # primary Sass file
```

说明：<br/>

* modules目录： 主要放functions, variables 和 mixin declarations。 类似于公共的库。导入进来的module, 不会影响css的输出。<br/>
* partials目录：主要放 typography, buttons, textboxes, selectboxes等等，组件性css。相比于其他的做法，把css拆分成header content sidebar footer，
这里倾向于采取smacss方式，拆分成更细小的种类。<br/>
* vendor目录： 主要放第三方的css， jquery UI，colorpicker等等。这里面的文件不修改，如果需要修改此类样式，应该在主样式表加载完第三方css后，在加载修改的css，覆盖掉第三方的。<br/>

## 1.2 主样式表

```css
// Modules and Variables
@import "partials/base";

// Partials
@import "partials/reset";
@import "partials/typography";
@import "partials/buttons";
@import "partials/figures";
@import "partials/grids";
// ...

// Third-party
@import "vendor/colorpicker";
@import "vendor/jquery.ui.core";
```

## 1.3 多个项目情况下

```
stylesheets/
|
|-- admin/           # Admin sub-project
|   |-- modules/
|   |-- partials/
|   `-- _base.scss
|
|-- account/         # Account sub-project
|   |-- modules/
|   |-- partials/
|   `-- _base.scss
|
|-- app/            # Site sub-project
|   |-- modules/
|   |-- partials/
|   `-- _base.scss
|
|-- vendor/          # CSS or Sass from other projects
|   |-- _colorpicker-1.1.scss
|   |-- _jquery.ui.core-1.9.1.scss
|   ...
|
|-- admin.scss       # Primary stylesheets for each project
|-- account.scss
`-- app.scss
```

一个项目下面包含好几个子项目时候，vendor可以单独抽离出来，一个项目一个主样式表。

# 2. 命名的规范
* 短，有意义，一看就明白。<br/>
* 使用功能性或通用的名字会减少不必要的文档或模板修改。
* 命名界定符 破折号：名称由多个单词组合，使用破折号分开。使用破折号可以确保html与js的隔离。js命名不支持破折号，可防止与js冲突。[具体案例](http://stackoverflow.com/questions/1696864/naming-class-and-id-html-attributes-dashes-vs-underlines)<br/>
* 前缀： 大型项目中加上应用标志性前缀（命名空间），使用破折号连接。使用命名空间可以防止命名冲突，方便维护。
* 模块等使用名词，状态等使用形容词，用来描述模块或对象的状态
*  class 分类（structural classes，type classes，modifier classes，functional classes， namespace classes）

## 2.1 modifier/state classes
```css
.component.is-active
.component.js-selcted
.component.has-children
```

# 3. 编码风格指南
# 3.1 Style Rule
* css 有效性

有效定义：每一行的css都是有用的，作用于页面的。不用的及时删掉

* 类型选择器

Avoid qualifying ID and class names with type selectors.<br/>
除非有必要（helper class这类辅助的），否则不要像这样使用，元素名后面连接id或者class <br/>
Avoiding unnecessary ancestor selectors is useful for [performance reasons](http://www.stevesouders.com/blog/2009/06/18/simplifying-css-selectors/)<br/>

selector的优化是css性能提升的关键点（selector engine works from right to left）


```css
/* 不推荐 */
ul#example {}
div.error {}
/* 推荐 */
#example {}
.error {}
```


* 属性缩写

使用缩写可以提高代码的效率和方便理解。

```css
/* 不推荐 */
border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;
/* 推荐 */
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;
```

* URI外的引号

省略URI外的引号。不要在 url() 里用 ( "" , '' ) 。

```css
@import url(//www.google.com/css/go.css);
```

* 关于数
  
  * 0和单位。省略0后面的单位。非必要的情况下 0 后面不用加单位。<br>
  ```css
  margin: 0;
  padding: 0;
  ```

  * 0开头的小数。省略0开头小数点前面的0。值或长度在-1与1之间的小数，小数前的 0 可以忽略不写。<br>
  ```css
  font-size: .8em;
  ```

  * 十六进制。 十六进制尽可能使用3个字符。加颜色值时候会用到它，使用3个字符的十六进制更短与简洁。<br>
  ```css
  /* 不推荐 */
  color: #eebbcc;
  /* 推荐 */
  color: #ebc;
  ```

* 避免使用css hack方法。尽量尝试用其他方法解决问题。有损项目效率和代码管理。

* 参考 oocss 的两个原则： 1. 结构与外观分离 2. 容器与内容分离
* 参考 smacss 的两个核心目标：1. 提升语义。 2. 降低对特定html结构的依赖



# 3.2 Formatting Rule(格式要求)

* 规则分行

每个规则独立一行。两个规则之间隔行。

```css
html {
  background: #fff;
}

body {
  margin: auto;
  width: 50%;
}
```

* 选择器和声明分行

将选择器和声明隔行。每个选择器和声明都要独立新行。

```css
/* 不推荐 */
a:focus, a:active {
  position: relative; top: 1px;
}
/* 推荐 */
h1,
h2,
h3 {
  font-weight: normal;
  line-height: 1.2;
}
```

* 代码块内容缩进

缩进所有代码块（“{}”之间）内容。缩进所有代码块的内容，它能够提高层次结构的清晰度。

```css
@media screen, projection {

  html {
    background: #fff;
    color: #444;
  }

}
```

* 声明顺序
参考申明顺序表<br>
1. Positioning
2. Box model
3. Typographic
4. Visual
声明顺序按照 位置、盒子模型等一层一层写

```css
position
top
right
bottom
left
z-index
display
float
width
height
max-width
max-height
min-width
min-height
padding
padding-top
padding-right
padding-bottom
padding-left
margin
margin-top
margin-right
margin-bottom
margin-left
margin-collapse
margin-top-collapse
margin-right-collapse
margin-bottom-collapse
margin-left-collapse
overflow
overflow-x
overflow-y
clip
clear
font
font-family
font-size
font-smoothing
osx-font-smoothing
font-style
font-weight
hyphens
src
line-height
letter-spacing
word-spacing
color
text-align
text-decoration
text-indent
text-overflow
text-rendering
text-size-adjust
text-shadow
text-transform
word-break
word-wrap
white-space
vertical-align
list-style
list-style-type
list-style-position
list-style-image
pointer-events
cursor
background
background-attachment
background-color
background-image
background-position
background-repeat
background-size
border
border-collapse
border-top
border-right
border-bottom
border-left
border-color
border-image
border-top-color
border-right-color
border-bottom-color
border-left-color
border-spacing
border-style
border-top-style
border-right-style
border-bottom-style
border-left-style
border-width
border-top-width
border-right-width
border-bottom-width
border-left-width
border-radius
border-top-right-radius
border-bottom-right-radius
border-bottom-left-radius
border-top-left-radius
border-radius-topright
border-radius-bottomright
border-radius-bottomleft
border-radius-topleft
content
quotes
outline
outline-offset
opacity
filter
visibility
size
zoom
transform
box-align
box-flex
box-orient
box-pack
box-shadow
box-sizing
table-layout
animation
animation-delay
animation-duration
animation-iteration-count
animation-name
animation-play-state
animation-timing-function
animation-fill-mode
transition
transition-delay
transition-duration
transition-property
transition-timing-function
background-clip
backface-visibility
resize
appearance
user-select
interpolation-mode
direction
marks
page
set-link-source
unicode-bidi
speak

* 属性名完结

在属性名冒号结束后加一个空字符。
```css
/* 不推荐 */
h3 {
  font-weight:bold;
}
/* 推荐 */
h3 {
  font-weight: bold;
}
```

* 声明完结

所有声明都要用“;”结尾。考虑到一致性和拓展性，请在每个声明尾部都加上分号。

```css
/* 不推荐 */
.test {
  display: block;
  height: 100px
}
/* 推荐 */
.test {
  display: block;
  height: 100px;
}
```

参考资料：<br/>
[google css style guide](https://google.github.io/styleguide/htmlcssguide.xml)<br/>
[smacss](https://smacss.com/)<br/>
[oocss](http://oocss.org/)</br>
[bem]()<br>