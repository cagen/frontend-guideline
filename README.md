# Kaikeba前端代码规范

> - 版本：v0.1
> - 开始时间：2014.11.24
> - 完稿时间：暂无
> - 参考：
- [Google HTML/CSS Style Guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)
- [Code Style](https://github.com/mdo/code-guide)
- [Douban Code Guideline](https://github.com/kejun/CSS-Code-Guideline)


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

## HTML样式规范

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

## HTML格式规范

### 通用格式

### 引号使用
