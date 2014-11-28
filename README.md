# 前端代码规范

版本：v0.1  
开始时间：2014.11.24  
版本v0.1：2014.11.28  
参考：  
> [Google HTML/CSS Style Guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)  
> [Code Style](https://github.com/mdo/code-guide)  
> [Principles of writing consistent, idiomatic CSS](https://github.com/necolas/idiomatic-css)
> [Douban Code Guideline](https://github.com/kejun/CSS-Code-Guideline)  

----------

## 通用样式规则

### 链接协议

嵌入资源忽略协议类型  
对于图片、媒体文件、样式文件、脚本的URL地址中忽略协议部分（https:,http:），除非该资源只能支持某一种协议类型。  

原因：
- 使URL地址可以根据当前页面的协议形式进行请求，避免出现https和http混合的内容。
- 减少文件大小

```html
<!-- 不推荐 -->
<script src="http://www.google.com/js/gweb/analytics/autotrack.js"></script>
<!-- 推荐 -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```

```css
/* 推荐 */
.example {
  background: url(http://www.google.com/images/example);
}
/* 不推荐 */
.example {
  background: url(//www.google.com/images/example);
}
```

----------

## 通用格式规则

### 缩进
使用空格缩进，一次缩进两个空格  
不要使用tab或者混合使用tab和space进行缩进

### 大小写
只是用小写  
所有的代码小写，包括：
- HTML元素名称、属性、属性值（text/CDATA除外）
- CSS的选择器，属性，和属性值（字符串除外）

```html
<!-- 不推荐 -->
<A HREF="/">Home</A>

<!-- 推荐 -->
<img src="google.png" alt="Google">
```

```css
/* 不推荐 */
color: #E5E5E5;

/* 推荐 */
color: #e5e5e5;
```

### 末尾空格
去除行尾无用空格  
行尾空格容易造成文件差异比较时的混乱


----------

## 通用Meta规则

### 编码
使用UTF-8编码格式（不要使用BOM）
- 确保编辑器使用UTF-8编码格式，不要使用[BOM](https://zh.wikipedia.org/wiki/%E4%BD%8D%E5%85%83%E7%B5%84%E9%A0%86%E5%BA%8F%E8%A8%98%E8%99%9F)
- 在HTML模板和文档中，通过`<meta charset="utf-8">`声明编码格式
- 不要特定样式表的编码格式，因为它默认为UTF-8

更多关于编码格式及如何声明的问题，参见[Handling character encodings in HTML and CSS](http://www.w3.org/International/tutorials/tutorial-char-enc/)

### 注释 （可选）
如果可能，对所有需要解释的代码写注释，写清代码包含内容、使用目的、为何使用此解决方案  
（根据项目复杂度的不同，这一工作的会花费巨大的工作量）


### TODO（待议）
使用`TODO`标识标明需要完成的工作  
只使用`TODO`来高亮标识需要完成的工作，不要使用其他的符号（例如`@@`）  
在TODO标识后的括号内标明用户名或者邮件列表，格式为`TODO(contact)`
```html
{# TODO(john.doe): revisit centering #}
<center>Test</center>
```
在`TODO:`后面加上需要完成的工作内容
```html
<!-- TODO: remove optional tags -->
<ul>
  <li>Apples</li>
  <li>Oranges</li>
</ul>
```


----------

## HTML代码规范

### 文档类型
使用HTML5的文档类型`<!DOCTYPE html>`  
（推荐使用HTML，对应的MIME类型为`text/html`。不推荐使用XHTML，对应的MIME类型为`application/xhtml+xml`，因为该类型缺少浏览器的支持，并且相较于HTML，会少了很多优化的空间）  
并且对于HTML来说，不要关闭空元素，例如：使用`<br>`，而无需使用`<br />`

### HTML有效性
书写有效的HTML代码  
使用[W3C HTML validator](http://validator.w3.org/nu/)测试代码有效性  
编写有效的HTML代码，是一种可衡量的代码质量基准。同时也可以帮助理解HTML技术的要求和限制，这保证了HTML的正确使用。
```html
<!-- 不推荐 -->
<title>Test</title>
<article>This is only a test.
<!-- 推荐 -->
<!DOCTYPE html>
<meta charset="utf-8">
<title>Test</title>
<article>This is only a test.</article>
```

### 语义化
根据目的使用对应的HTML元素（常被错误的称呼为HTML标签）  
例如：标题使用标题元素，段落使用`p`元素，链接使用`a`元素等等  
这么做的目的在于提高HTML代码的可访问性，可重用性以及代码效率
```html
<!-- 不推荐 -->
<div onclick="goToRecommendations();">All recommendations</div>
<!-- 推荐 -->
<a href="recommendations/">All recommendations</a>
```

### 多媒体向下兼容
为多媒体提供备选的内容  
对于图片、视频、`canvas`创建的动画对象等，确保提供替代的内容。  
例如：对图片使用`alt`属性提供有意义的替代文字，对于视频音频，提供对应的字幕和标题。  
提供备选内容对于提高页面的可访问性有很大帮助。例如：一个盲人如果没有`alt`属性里的内容，会很难了解一张图片是关于什么的。一个聋人没有字幕或者标题也会很难了解视频或者音频的内容。  
以下特殊情况可以使用空的属性值`alt=""`：
- 如果图片在页面中已经有了其他的文字解释，引入`alt`的内容会带来冗余
- 该图片只是为了装饰性而使用，并且还无法使用CSS背景图片来替代

```html
<!-- 不推荐 -->
<img src="spreadsheet.png">
<!-- 推荐 -->
<img src="spreadsheet.png" alt="Spreadsheet screenshot.">
```

### 关注点分离
展示和行为分离  
将结构（标记）、展示（样式）、行为（脚本）分离，并且尽量使这三者的交互减少到最小  
保证文档和模板只包含HTML，并且HTML只作为页面结构的目的而使用，将所有的样式放入样式表，将所有的行为放入脚本中去  
并且，在HTML模板中尽力减少外联样式和脚本的数量以保证三者的连接尽量简单  
将样式和行为从结构中分离出来可提高代码的可维护性。因为修改HTML模板的代价远高于修改样式表和脚本。
```html
<!-- 不推荐 -->
<!DOCTYPE html>
<title>HTML sucks</title>
<link rel="stylesheet" href="base.css" media="screen">
<link rel="stylesheet" href="grid.css" media="screen">
<link rel="stylesheet" href="print.css" media="print">
<h1 style="font-size: 1em;">HTML sucks</h1>
<p>I’ve read about this on a few sites but now I’m sure:
  <u>HTML is stupid!!1</u>
<center>I can’t believe there’s no way to control the styling of
  my website without doing everything all over again!</center>
  
<!-- 推荐 -->
<!DOCTYPE html>
<title>My first CSS-only redesign</title>
<link rel="stylesheet" href="default.css">
<h1>My first CSS-only redesign</h1>
<p>I’ve read about this on a few sites but today I’m actually
  doing it: separating concerns and avoiding anything in the HTML of
  my website that is presentational.
<p>It’s awesome!
```

### HTML字符实体引用
不要使用HTML字符实体引用  
只要团队中使用的文件和编辑器都设置为UTF-8，则没有必要使用`&mdash;`、`&rdquo;`、`&#x263a;`这样的实体引用  
需要使用实体引用的情况有两种：
- 在HTML中有特殊意义的字符，比如`<`、`&`
- 控制符，或者空格

```html
<!-- 不推荐 -->
The currency symbol for the Euro is &ldquo;&eur;&rdquo;.

<!-- 推荐 -->
The currency symbol for the Euro is “€”.
```

### `type`属性
忽略样式表和脚本的`type`属性  
HTML5规范中样式表和脚本的`type`属性默认分别为`text/css`和`text/javascript`。  
对于老的浏览器，这么做也是可以的。  
当然，如果样式表使用的不是CSS或者脚本使用的不是Javascript，则需要显示声明`type`属性

```html
<!-- 不推荐 -->
<link rel="stylesheet" href="//www.google.com/css/maia.css" type="text/css">
<script src="//www.google.com/js/gweb/analytics/autotrack.js" type="text/javascript"></script>

<!-- 推荐 -->
<link rel="stylesheet" href="//www.google.com/css/maia.css">
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```


----------

## HTML代码格式

### 通用格式
对于所有的`block`、`list`、`table`元素，必须新起一行，并且缩进其子元素  
> 元素样式的独立（因为CSS允许元素假定每种display属性对应不同角色），`block`、`list`、`table`元素全部需要新起一行。  
> Independent of the styling of an element (as CSS allows elements to assume a different role per display property), put every block, list, or table element on a new line.  

对于`block`、`list`、`table`的子节点，都需要缩进。  
如果使用时碰到list的项之间出现空格的问题，可以折中地将所有`li`元素写在一行。代码检查会出现警告，而非错误。

```html
<!-- 推荐 -->
<blockquote>
  <p><em>Space</em>, the final frontier.</p>
</blockquote>

<!-- 推荐 -->
<ul>
  <li>Moe
  <li>Larry
  <li>Curly
</ul>

<!-- 推荐 -->
<table>
  <thead>
    <tr>
      <th scope="col">Income
      <th scope="col">Taxes
  <tbody>
    <tr>
      <td>$ 5.00
      <td>$ 4.50
</table>
```

### 引号使用
属性值使用双引号  
在HTML中的属性值使用双引号而非单引号

```html
<!-- 不推荐 -->
<a class='maia-button maia-button-secondary'>Sign in</a>

<!-- 推荐 -->
<a class="maia-button maia-button-secondary">Sign in</a>
```


----------

## CSS代码规范

### CSS有效性
使用有效的CSS代码。  
除非遇到特定的兼容性语法，如前缀、IE兼容语法等。  
使用[W3C CSS validator](http://jigsaw.w3.org/css-validator/)测试有效性  
验证CSS有效性可以发现无用的CSS代码，并及时移除，保证正确的CSS使用。

### ID和Class命名
使用有意义或者通用的ID和Class命名。  
ID和Class命名时最好能够表达出这一样式的目的，而不是使用表象的或者晦涩难懂的命名方式。  
特定的或者能够表达出目的的命名容易理解而且不会轻易改变  
通用的命名是为了那些没有特殊目的元素或者那些跟其相同的元素没有意义上差别的元素，他们就类似于Helper方法一样  
使用通用的命名可以减少HTML代码改动的几率。  
```css
/* 不推荐: 无意义 */
#yee-1901 {}

/* 不推荐: 太过于表象 */
.button-green {}
.clear {}

/* 推荐: 目的特定 */
#gallery {}
#login {}
.video {}

/* 推荐: 通用 */
.aux {}
.alt {}
```

### ID和Class命名规范
命名在保证易懂的前提下尽量简短  
在能够描述清楚这一ID或Class时，尽力保持简短  
这样命名ID和Class能够让代码更加易懂并提升代码效率
```css
/* 不推荐 */
#navigation {}
.atr {}

/* 推荐 */
#nav {}
.author {}
```

### 类型选择器
避免使用类型选择器去限定ID和Class  
除非有必要（例如使用Helper类时），不要将类型选择器与ID或Class同时使用  
减少不必要的祖先选择器对于性能来说[有所帮助](http://www.stevesouders.com/blog/2009/06/18/simplifying-css-selectors/)。
```css
/* 不推荐 */
ul#example {}
div.error {}

/* 推荐 */
#example {}
.error {}
```

### 属性简写 （待议）
尽可能使用属性简写  
CSS提供了的某些简写形式（如font），就算只是显式设定其中一个值，也要使用缩写形式。  
使用简写形式可以提高代码效率和可读性。
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

### 0和单位
在属性值为0时，忽略单位，除非该单位是必须的。
```css
/* 推荐 */
margin: 0;
padding: 0;
```

### 0.前缀
当属性值在-1与1之间时，忽略前置的`0.`
```css
/* 推荐 */
font-size: .8em;
```

### 16进制
尽量使用3位16进制数更加短小简洁
```css
/* 不推荐 */
color: #eebbcc;

/* 推荐 */
color: #ebc;
```

### 前缀 （可选）
带应用特定的前缀的选择器  
在大型的项目中嵌入了其他项目的代码，或者引用了其他站点的样式时，使用简短而独特的标识符当作前缀（类似命名空间），后用`-`连接  
使用这种方式可以避免命名冲突，并可以使维护更加方便，比如在查找和替换样式的时候


### ID和Class分隔符
在ID和Class中使用`-`作为词语的分隔符  
不要使用下划线或者驼峰等形式作为ID和Class的分隔形式，需要保持这种习惯，以提高代码的可读性和一致性。
```css
/* 不推荐：未分割词语 */
.demoimage {}

/* 不推荐：下划线形式 */
.error_status {}

/* 推荐 */
#video-id {}
.ads-sample {}
```

### CSS Hacks
避免使用UA检测和CSS Hack，优先尝试其他的解决方案  
虽然使用UA检测或者特殊的CSS滤镜、Hack可以很方便地解决一些样式上的差异问题。但是为了编写和维护高效和可控的代码基础，这些方法只能作为最后的手段。从项目的长远来看，如果轻易地给这些方法开绿灯，就会使得项目更加倾向于使用更多的这些方法，最终会导致项目代码完全无法维护。


----------

## CSS代码格式规范

### 属性声明顺序
按类型声明  
相关的属性声明应当归为一组，并按照下面的顺序排列：  
1. Positioning
2. Box model
3. Typographic
4. Visual
由于定位（positioning）可以从正常的文档流中移除元素，并且还能覆盖盒模型（box model）相关的样式，因此排在首位。盒模型排在第二位，因为它决定了组件的尺寸和位置。  
其他属性只是影响组件的内部（inside）或者是不影响前两组属性，因此排在后面。  
完整的属性列表及其排列顺序请参考[Recess](https://github.com/twitter/recess/blob/master/lib/compile/strict-property-order.js)。

```css
/* 推荐 */
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
```

### 块级元素缩进
所有块级元素要进行缩进，增加层级能提高代码的可读性。
```css
/* 推荐 */
@media screen, projection {

  html {
    background: #fff;
    color: #444;
  }

}
```

### 声明结尾
每个属性声明都要加分号，即使最后一个声明也不要省略
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

### 属性名结尾
属性名的冒号后需要加空格
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

### 声明块格式
在最后一个选择器和声明块的左侧花括号之间要有一个空格，且声明块左侧花括号必须和最后一个选择器同一行。
```css
/* 不推荐：没空格 */
#video{
  margin-top: 1em;
}

/* 不推荐：不在同一行 */
#video
{
  margin-top: 1em;
}

/* 推荐 */
#video {
  margin-top: 1em;
}
```

### 选择器和声明分隔
每个选择器和声明都要新起一行
```css
/* 不推荐：没空格 */
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

### 规则分隔
在不同规则之间空一行
```css
/* 推荐 */
html {
  background: #fff;
}

body {
  margin: auto;
  width: 50%;
}
```

### CSS冒号使用
属性选择器和属性值都使用单引号，不要给URI值（`url()`）加上任何引号
例外：如果你必须要使用`@charset`，可以使用双引号
```css
/* 不推荐 */
@import url("//www.google.com/css/maia.css");

html {
  font-family: "open sans", arial, sans-serif;
}


/* 推荐 */
@import url(//www.google.com/css/maia.css);

html {
  font-family: 'open sans', arial, sans-serif;
}
```

----------

## CSS Meta规范

### 注释分块 (可选)
如果可以，可以将一组样式使用注释分割开来。
```css
/* 推荐 */

/* Header */

#adw-header {}

/* Footer */

#adw-footer {}

/* Gallery */

.adw-gallery {}
```

## 写在最后

**保持一致性！**  
在你修改一段代码之前，花几分钟看看这些代码的基本格式。如果他们在所有的运算符前后都加了空格，那么你也应该加上。如果代码的评论是用一圈`#`号围起来的，那么你的评论也该这么做。   
制定并施行代码规范的目的在于，能建立一种通用的代码“语法”，让人们能够专注于你的代码要表达什么，而不是怎么表达的。我们在这里制订了全局的样式规范，让人们能够了解这一“语法”，但如果你添加的代码与原有的代码差别很大，则要尽量注意保持与原有代码风格一致。
