# 每个人都应该知道的jQuery开发技巧 [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

简单的技巧的集合，以帮助您的jQuery玩得更溜。

> 对于其他大名单退房 [@sindresorhus](https://github.com/sindresorhus/)的策展的名单 [真棒名单](https://github.com/sindresorhus/awesome/).


<div id="table-of-contents"></div>
## 目录

* [温馨提示](＃提示)
* [支持](＃support)
* [贡献指引](../../CONTRIBUTING.md)


<div id="tips"></div>
## 提示

1. [检查是否加载的jQuery](＃checking-if-jquery-loaded)
1. [使用`.on()`绑定代替`.click()`](#use-on-binding-instead-of-click)
1. [返回顶部按钮](#back-to-top-button)
1. [预加载图片](#preload-images)
1. [检测图片是否已加载](#checking-if-images-are-loaded)
1. [自动修复破碎的形象](#fix-broken-images-automatically)
1. [悬停切换类](#toggle-classes-on-hover)
1. [禁用输入字段](#disabling-input-fields)
1. [停止正在加载的链接](#stop-the-loading-of-links)
1. [缓存jquery选择器](#cache-jquery-selectors)
1. [淡入淡出切换/幻灯片](#toggle-fadeslide)
1. [简单折叠动画](#simple-accordion)
1. [让两个div相同的高度](#make-two-divs-the-same-height)
1. [在新标签/窗口打开外部链接](#open-external-links-in-new-tabwindow)
1. [通过文本查找元素](#find-element-by-text)
1. [可见性改变触发器](#trigger-on-visibility-change)
1. [Ajax调用错误处理](#ajax-call-error-handling)
1. [连锁插件调用](#chain-plugin-calls)
1. [排序列表项按字母顺序](#sort-list-items-alphabetically)


<div id="checking-if-jquery-loaded"></div>

### 检查如果加载的jQuery

之前，你可以使用jQuery做任何事情，你首先需要确保它加载：

```javascript
if (typeof jQuery == 'undefined') {
  console.log('jQuery hasn\'t loaded');
} else {
  console.log('jQuery has loaded');
}
```

现在你是关闭...

<sup>[回目录](#table-of-contents)</sup>


<div id="use-on-binding-instead-of-click"></div>

### 使用`.on()`绑定代替`.click()`

使用`.on()`给你几个优点比使用`.click()`，如添加多个事件的能力...

```javascript
.on('click tap hover')
```

...绑定适用于动态创建的元素，以及（有没有必要动态添加到DOM元素每一个元素手动绑定）...

...和可能性来设置一个命名空间:

```javascript
.on('click.menuOpening')
```

命名空间给你解除对特定事件（例如，`.off（'click.menuOpening'）`）的权力。

<sup>[回目录](#table-of-contents)</sup>


<div id="back-to-top-button"></div>

### 返回顶部按钮

通过使用jQuery中的`animate`和`scrollTop`方法，你并不需要一个插件来创建一个简单的滚动到顶部动画：

```javascript
// Back to top
$('.container').on('click', '.back-to-top', function (e) {
  e.preventDefault();
  $('html, body').animate({scrollTop: 0}, 800);
});
```

```html
<!-- Create an anchor tag -->
<a class="top" href="#">Back to top</a>
```

改变，你想要滚动降落`scrollTop`价值的变化。所有你真的做的是动画文件的全身各处的800毫秒的过程中，直到它滚动到文档的顶部。

**注：** 观看一些 [错误行为](https://github.com/jquery/api.jquery.com/issues/417) 用 `scrollTop`。

<sup>[回目录](#table-of-contents)</sup>


<div id="preload-images"></div>

### 预加载图片

如果你的网页使用了大量不可见的开始（例如，悬停）图片是有意义的预加载它们：

```javascript
$.preloadImages = function () {
  for (var i = 0; i < arguments.length; i++) {
    $('<img>').attr('src', arguments[i]);
  }
};

$.preloadImages('img/hover-on.png', 'img/hover-off.png');
```

<sup>[回目录](#table-of-contents)</sup>


<div id="checking-if-images-are-loaded"></div>

### 检测图片是否已加载

有时你可能需要检查你的图片是否已完全加载完毕，以便让脚本执行：

```javascript
$('img').on('load', function () {
  console.log('image load successful');
});
```

您也可以检查，如果一个特定的形象已被用一个ID或类替换`<img>`标签加载。

<sup>[回目录](#table-of-contents)</sup>


<div id="fix-broken-images-automatically"></div>

### 修复加载失败的图片

如果你碰巧发现网站上的残缺的图像链接一个取代他们可以是一个痛苦事情。这一段简单的代码可以节省很多时间：

```javascript
$('img').on('error', function () {
  if(!$(this).hasClass('broken-image')) {
    $(this).prop('src', 'img/broken.png').addClass('broken-image');
  }
});
```

即使你没有任何断开的链接，这将不会做任何伤害。

<sup>[回目录](#table-of-contents)</sup>


<div id="toggle-classes-on-hover"></div>

### 切换悬停类

比方说，你想改变视觉可点击元素的页面上，当用户将鼠标悬停在它。您可以将类添加到你的元素，当用户徘徊;用户停止徘徊时删除类：

```javascript
$('.btn').on('hover', function () {
  $(this).addClass('hover');
}, function () {
  $(this).removeClass('hover');
});
```

你只需要添加必要的CSS。如果你想要一个更_simpler_方式使用`toggleClass`方法：

```javascript
$('.btn').hover(function () {
  $(this).toggleClass('hover');
});
```

**注：** CSS可能在这种情况下是更快的解决方案，但它仍然是值得的知道这一点。

<sup>[回目录](#table-of-contents)</sup>


<div id="disabling-input-fields"></div>

### 禁用输入字段

有时你可能要直到用户执行特定操作（例如，勾选“我已阅读条款”复选框）提交被禁用的形式或它的文本输入中的一个按钮。在`disabled`属性添加到您的输入，所以你可以在需要时启用它：

```javascript
$('input[type="submit"]').prop('disabled', true);
```

所有你需要做的是在输入再次运行`prop`方法，但`disabled`的值设置为`false`：

```javascript
$('input[type="submit"]').prop('disabled', false);
```

<sup>[回目录](#table-of-contents)</sup>


<div id="stop-the-loading-of-links"></div>

### 添加加载链接

有时候，你不想链接到特定网页，也没有重新加载页面;你可能希望他们做别的事情一样触发一些其他的脚本。这将做防止默认操作的技巧：

```javascript
$('a.no-link').on('click', function (e) {
  e.preventDefault();
});
```

<sup>[回目录](#table-of-contents)</sup>


<div id="cache-jquery-selectors"></div>

### 缓存jquery选择器

想想有多少次你一遍又一遍地在任何项目中再次写入相同的选择。每个`$('.element')`选择有每次搜索整个DOM，如果不管选择以前有运行。相反，运行一次选择器和存储在变量的结果：

```javascript
var blocks = $('#blocks').find('li');
```

现在你可以使用任何你想要的`blocks`变量，而不必每次搜索DOM：

```javascript
$('#hideBlocks').on('click', function () {
  blocks.fadeOut();
});

$('#showBlocks').on('click', function () {
  blocks.fadeIn();
});
```

缓存jQuery选择器是一种最简单的提升性能的方式。

<sup>[回目录](#table-of-contents)</sup>


<div id="toggle-fadeslide"></div>

### 切换淡入/幻灯片

滑动和渐变都是我们经常使用jQuery的动画。你可能只是想表明一个元素，当用户点击的东西，这使得`fadeIn`和`slideDown`方法完善。但是，如果你想要的元素出现在第一次点击，然后在第二次点击中消失

```javascript
// Fade
$('.btn').on('click', function () {
  $('.element').fadeToggle('slow');
});

// Toggle
$('.btn').on('click', function () {
  $('.element').slideToggle('slow');
});
```

<sup>[回目录](#table-of-contents)</sup>


<div id="simple-accordion"></div>

### 简单手风琴动画

这是一个快速手风琴动画一个简单的方法：

```javascript
// Close all panels
$('#accordion').find('.content').hide();

// Accordion
$('#accordion').find('.accordion-header').on('click', function () {
  var next = $(this).next();
  next.slideToggle('fast');
  $('.content').not(next).slideUp('fast');
  return false;
});
```

通过添加这个脚本你真正需要在网页上做的是必要的HTML去得到这个工作。

<sup>[回目录](#table-of-contents)</sup>


<div id="make-two-divs-the-same-height"></div>

### 让两个div相同的高度

有时候，你会想两个div具有相同的高度，无论他们有他们什么内容：

```javascript
$('.div').css('min-height', $('.main-div').height());
```

本示例设置了`min-height`这意味着它可以比主分区越大，但从来没有变小。然而，更灵活的方法是循环在一组的元素，并设置高度最高的元件的高度：

```javascript
var $columns = $('.column');
var height = 0;
$columns.each(function () {
  if ($(this).height() > height) {
    height = $(this).height();
  }
});
$columns.height(height);
```

如果希望_all_列具有相同的高度：

```javascript
var $rows = $('.same-height-columns');
$rows.each(function () {
  $(this).find('.column').height($(this).height());
});
```

**注：**这是可以做到几个方面[在CSS](http://codepen.io/AllThingsSmitty/pen/KMPqoO)，但根据您的需要是什么，知道如何在jQuery中做到这一点还是值得的。

<sup>[回目录](#table-of-contents)</sup>


<div id="open-external-links-in-new-tabwindow"></div>

### 在新标签中打开外部链接/窗

打开外部链接在新的浏览器标签或窗口，并确保在同一起源于同一个标签或窗口打开链接：

```javascript
$('a[href^="http"]').attr('target', '_blank');
$('a[href^="//"]').attr('target', '_blank');
$('a[href^="' + window.location.origin + '"]').attr('target', '_self');
```

**注：** `window.location.origin`不IE10工作。 [此修复程序](http://tosbourn.com/a-fix-for-window-location-origin-in-internet-explorer/) 主罚问题的照顾。

<sup>[回目录](#table-of-contents)</sup>


<div id="find-element-by-text"></div>

### 通过文字查找元素

通过jQuery中使用`contains()`选择，你可以找到在元素内容的文字。如果文本不存在，该元素将被隐藏：

```javascript
var search = $('#search').val();
$('div:not(:contains("' + search + '"))').hide();
```

<sup>[回目录](#table-of-contents)</sup>


<div id="trigger-on-visibility-change"></div>

### 捕获可见性的改变

当用户不再在一个浏览器窗口中获取到焦点，或者重新把焦点放在另外一个窗口中时，处理如下

```javascript
$(document).on('visibilitychange', function (e) {
  if (e.target.visibilityState === 'visible') {
    console.log('Tab is now in view!');
  } else if (e.target.visibilityState === 'hidden') {
    console.log('Tab is now hidden!');
  }
});
```

<sup>[回目录](#table-of-contents)</sup>


<div id="ajax-call-error-handling"></div>

### AJAX调用错误处理

当Ajax调用返回404或500错误的错误处理程序将被执行。如果没有定义的处理程序，其他的jQuery代码可能就失效了。定义一个全局的Ajax错误处理程序：

```javascript
$(document).on('ajaxError', function (e, xhr, settings, error) {
  console.log(error);
});
```

<sup>[回目录](#table-of-contents)</sup>


<div id="chain-plugin-calls"></div>

### 链式插件调用

jQuery的允许“链式”插件的方法调用，以减轻反复查询DOM并创建多个jQuery的对象的过程。比方说，下面的代码片段代表了你的插件方法调用：

```javascript
$('#elem').show();
$('#elem').html('bla');
$('#elem').otherStuff();
```

这可以通过链式调用可以大大提高效率：

```javascript
$('#elem')
  .show()
  .html('bla')
  .otherStuff();
```

另一种方法是缓存元素的变量（用 `$` 前缀）：


```javascript
var $elem = $('#elem');
$elem.hide();
$elem.html('bla');
$elem.otherStuff();
```

无论链接和jQuery的[缓存](#缓存的jQuery选择器)方法是导致更短，更快的代码的最佳实践。

<sup>[回目录](#table-of-contents)</sup>


<div id="sort-list-items-alphabetically"></div>

### 按字母顺序排序列表

比方说，你结束了在列表中的项目太多。也许内容是由CMS生成并希望他们字母顺序：

```javascript
var ul = $('#list'),
lis = $('li', ul).get();

lis.sort(function (a, b) {
  return ($(a).text().toUpperCase() < $(b).text().toUpperCase()) ? -1 : 1;
});

ul.append(lis);
```

你去那里！

<sup>[回目录](#table-of-contents)</sup>


## 支持

当前版本的Chrome，火狐，Safari，Opera，Edge和IE11。