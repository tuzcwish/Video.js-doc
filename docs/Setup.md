# Video.js 设置

## 获取 Video.js

Video.js 官方通过 CDN 和 npm 发布。

Video.js 是开箱即用的，不仅是 HTML `<script>` 和 `<link>` 标签，在所有主要的模块化/包管理/构建工具上也是如此，例如：Browserify，Node，WebPack 等。

详细信息可以查看 [起步](https://videojs.com/getting-started/) 文档。

## 创建播放器

> NOTE: Video.js 适用于 `<video>` 和 `<audio>` 元素，但为简单起见，我们仅提及 <video> 元素。

一旦 Video.js [在你的页面上完成加载](https://videojs.com/getting-started/)，你就可以准备创建一个播放器了。

Video.js的核心优势在于它修饰了[标准的 `<video>` 元素](https://www.w3.org/TR/html5/embedded-content-0.html#the-video-element)并模拟了其相关的[事件和API](https://www.w3.org/2010/05/video/mediaevents.html)，同时提供了一个可自定义的基于DOM的UI。

Video.js 支持 `<video>` 元素的所有属性（例如 `controls` ，`preload` 等），也支持它的[专属配置](./Setup.md#配置项)。有两种方式可以创建播放器并传递配置项，它们都从一个带有 `class="video-js"` 属性的标准 `<video>` 元素开始。

```html
<video class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

各种嵌入选项的高级概述，可以查看[嵌入页面](./Embeds.md)，然后按照本页的剩余部分操作。

### 自动设置

默认情况下，当网页结束加载时，Video.js 会扫描带有 `data-setup` 属性的媒体元素，`data-setup` 是用来给 Video.js 传递配置项的，下面是一个最小化的例子：

```html
<video class="video-js" data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

> NOTE: 你应该在 `data-setup` 里使用单括号，因为它应该包含的是 JSON 数据。

### 手动设置

在现在网页中，页面在刚结束加载时常常不存在一个 `<video>` 元素，在这些情况下，无法使用自动设置，但是可以通过 [videojs 函数](https://docs.videojs.com/module-videojs.html)实现的手动设置。

调用这个函数的一种方法是传递一个匹配 `<video>` 元素 ID 的字符串：

```html
<video class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

```JavaScript
videojs('my-player');
```

然而，使用 `id` 属性并不总是实用，因此 `videojs` 函数也接收一个 DOM 元素：

```html
<video class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4" type="video/mp4">
  <source src="//vjs.zencdn.net/v/oceans.webm" type="video/webm">
</video>
```

```JavaScript
videojs(document.querySelector('.video-js'));
```

### 获取播放器的引用

一旦创建了一个播放器，Video.js 就会在内部进行跟踪。有几种方式可以获取已存在的播放器的引用。

#### 使用 `videojs`

使用已存在的播放器的 ID 来调用 `videojs()` 将返回该播放器，而不是创建一个新的播放器。

如果没有匹配的播放器，Video.js 会尝试创建一个。

#### 使用 `videojs.getPlayer()`

有时候，你想在没有调用 `videojs()` 的潜在副作用的情况下获得一个播放器的引用，可以通过用一个匹配元素 ID 或者元素自身的字符串来调用 `videojs.getPlayer()` 来实现。

#### 使用 `videojs.getPlayer()` 还是 `videojs.players`

`videojs.players` 属性会暴露所有已知的播放器，而 `videojs.getPlayer()` 方法只会简单地返回一个相同的对象。

播放器被保存在一个使用键值匹配 ID 的对象中。

> NOTE: 如果一个播放器通过一个没有 ID 的元素被创建，那么它将会被分配一个自动生成的 ID。

## 配置项

> NOTE: 这份指南仅包含如何在播放器设置阶段传递配置项，要查看所有可用配置项的完整参考，请查看 [配置指南](https://github.com/videojs/video.js/blob/master/docs/guides/options.md)。

有三种方式可以给 Video.js 传递配置项。由于 Video.js 修饰了原生 HTML5 `<video>` 元素，因此它的很多配置项也可用作[标准 `<video>` 标签的属性](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#Attributes)：

```HTML
<video controls autoplay preload="auto" ...>
```

另外，你可以使用 `data-setup` 属性以 [JSON](https://json.org/example.html) 格式传递配置项。这也是设置 `<video>` 元素非标准配置项的方式。

> NOTE: 你 *必须* 使用单括号包裹 `data-setup` 的值，因为它是一个 JSON 字符串，里面的内容必须使用双括号。 

最后，如果你没有使用 `data-setup` 的方式来设置播放器，你可以将播放器配置项对象作为第二个参数传递给 `videojs` 函数。

```JavaScript
videojs('my-player', {
  controls: true,
  autoplay: false,
  preload: 'auto'
});
```

> NOTE: 不要同时使用 data-setup 和配置项对象。

### 全局默认配置

针对所有播放器的全局默认设置可以在 `videojs.options` 中找到并直接修改。例如，要给后面所有的播放器设置 `autoplay: true` 只需要：

```JavaScript
videojs.options.autoplay = true;
```

### 针对 `<video>` 标签属性的注释

许多属性都被称为所谓的 [布尔属性](https://www.w3.org/TR/2011/WD-html5-20110525/common-microsyntaxes.html#boolean-attributes)，这意味着它们要么是处于激活状态要么是关闭状态。在这些情况下，属性不应该有值(或者使用它们的名字作为值)——它的存在代表着一个 true 值，如果不存在就代表一个 false 值。

这些是错误的：

```HTML
<video controls="true" ...>
<video loop="true" ...>
<video controls="false" ...>
```

> NOTE: 带有 `controls="false"` 的例子可能会让初学者感到困惑，它实际上会将 controls 设置为开。

这些是正确的：

```HTML
<video controls ...>
<video loop="loop" ...>
<video ...>
```

## 播放器就绪

因为 Video.js 是有可能会异步加载的，所以在设置上立即与播放器进行交互并不总是安全的。因此，Video.js 有一个“就绪”的概念，之前使用过 jQuery 的人应该都会很熟悉。

实际上，一个 Video.js 播放器可以定义任意数量的 ready 回调函数。有三种方式可以传递这些回调函数。在每个例子中，我们都将给播放器添加一个相同的 class：

将回调函数作为第三个参数传递给 `videojs()` 方法：

```JavaScript
// Passing `null` for the options argument.
videojs('my-player', null, function() {
  this.addClass('my-example');
});
```

给播放器的 `ready()` 方法传递一个回调函数：

```JavaScript
var player = videojs('my-player');

player.ready(function() {
  this.addClass('my-example');
});
```

监听播放器的 `ready` 事件：

```JavaScript
var player = videojs('my-player');

player.on('ready', function() {
  this.addClass('my-example');
});
```

每一个例子里，回调函数都是异步调用的。

上面所述的方法之间，一个重要的区别就是：使用 `on()` 方法给播放器添加 `ready` 监听器必须在播放器就绪之前设置完成；使用 `player.ready()` 的话，这个函数会在播放器就绪的情况下立即调用。

## 高级的播放器工作流程

关于更高级的播放器工作流程的讨论，请查看[播放器工作流程指南](https://github.com/videojs/video.js/blob/master/docs/guides/player-workflows.md)。
