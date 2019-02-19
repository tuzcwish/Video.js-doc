# embeds

## 如何嵌入 Video.js 播放器

Video.js 是为了增强 HTML5 中的 video 元素，多年来它的嵌入代码仅仅是一个 video 标签，然后 Video.js 把这个 video 标签包裹进一个用于我们放置控件和播放器所需要的其他内容的 div 。

在很长的时间里，这样是足够的。在2016年，“ div 摄取”被添加了，它允许开发者为 Video.js 的播放器提供一个 div ，而不是让它自己创建。这部分可以帮助重排内容，也有助于在 iOS 上填充视频元素。我们在创建了播放器 div 之后重新创建 video 元素。然而，用一个 `<div>` 来包裹一个嵌入的 `<video>` 元素，看起来有点奇怪。所以我们创建了一个新的嵌入方式，用一个 `<video-js>` 来嵌入。

下面是三种嵌入方式的细节。

## 嵌入

### `<video>` 嵌入

Video.js 的经典嵌入方法，你可以通过 `data-setup` 或者 `videojs` 方法来初始化它。

```html
<!-- via data-setup -->
<video id="vid1" class="video-js" data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video>

<!-- via code -->
<video id="vid1" class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video>
```

```javascript
const player = videojs('vid1', {});
```

### 播放器 div 摄取

增强的经典嵌入方式，你也可以通过 `data-setup` 或者 `videojs` 方法来初始化它。

```html
<!-- via data-setup -->
<div data-vjs-player>
  <video id="vid1" class="video-js" data-setup='{}'>
    <source src="//vjs.zencdn.net/v/oceans.mp4">
  </video>
</div>

<!-- via code -->
<div data-vjs-player>
  <video id="vid1" class="video-js">
    <source src="//vjs.zencdn.net/v/oceans.mp4">
  </video>
</div>
```

```javascript
const player = videojs('vid1', {});
```

正如你看到的，它和经典的 `<video>` 嵌入方式没有很大的区别，也让 Video.js 和 `React` 配合变得更简单。

### `<video-js>` 嵌入

这是 [自定义标签](https://developers.google.com/web/fundamentals/web-components/customelements) 代码嵌入方法，除了把 `video` 换成 `video-js` 之外，它看起来和 `<video>` 嵌入方式很类似。这对于播放器 div 摄取的所有内容都有用而且它更符合我们的库的名称！

```html
<!-- via data-setup -->
<video-js id="vid1" data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video-js>

<!-- via code -->
<video-js id="vid1">
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video-js>
```

```javascript
const player = videojs('vid1', {});
```

给嵌入代码添加 `class="video-js"` 已经不再必要了，因为它会在缺少 `video-js` 类的时候自动添加。

#### 自定义标签

根据 [Can I Use](https://caniuse.com/#feat=custom-elementsv1) ，原生的自定义标签支持相对较小，因为我们不想包含一个 polyfill ，我们只使用一个名为 `video-js` 的元素而不是使用一个完整的自定义标签。

### data-setup

这是一种让 Video.js 自动设置播放器的便于使用的方法。它是一个 HTML 属性，接收一个代表 `player options` 的值的 JSON 字符串，使用程序化的方法大概是更加可取的。