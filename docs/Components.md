# Components

Video.js 播放器是以组件为中心来构建的，`Player` 类以及其他所有播放器控制和 UI 元素的类都是从 `Component` 类继承而来。这种架构使得以镜像 DOM 的树状结构来构建 Video.js 播放器的用户界面变得容易。

[TOC]

## 什么是组件

组件是包含下列特性的 JavaScript 对象：

* 几乎所有情况下是一个 DOM 相关元素；
* 与 `Player` 对象相关；
* 能够管理任意数量的子组件；
* 能够监听和触发事件；
* 初始化和处置的生命周期。

有关组件的编程接口和更多细节，可以参阅[组件 API 文档](https://docs.videojs.com/Component.html)。



## 创建组件

Video.js 组件可以被继承和在 Video.js 中注册，以向播放器添加新的功能和 UI 。

这里有一个[工作示例](https://jsbin.com/vobacas/edit?html,css,js,output)，演示了创建一个用于在播放器顶部显示标题的组件。

此外，有几个方法是值得认识的：

* `videojs.getComponent(string name)`：从 Video.js 中检索组件构造函数；
* `videojs.registerComponent(string name, Function Comp)`：用 Video.js 注册组件构造函数；
* `videojs.extend(Function component, Object properties)`：提供原型继承。可用作扩展组件的构造函数，返回带有给定属性的新构造函数。

创建：

```javascript
// 给播放器添加按钮
var player = videojs('some-video-id');
var Component = videojs.getComponent('Component');
var button = new Component(player);

console.log(button.el());
```

上面的代码将会输出：

```html
<div class="video-js">
  <div class="vjs-button">Button</div>
</div>
```

给播放器添加一个新的按钮：

```javascript
// 给播放器添加一个按钮
var player = videojs('some-video-id');
var button = player.addChild('button');

console.log(button.el());
// 将会得到和上一个例子相同的 HTML 结果
```



## 子组件

同样地，管理组件构造的可用方法的完整细节请查看[组件 API 文档](https://docs.videojs.com/Component.html)。

### 基础例子

当子组件被添加到父组件时，Video.js 会将子组件插入到父组件的元素中。例如，像这样添加一个组件：

```javascript
// 向播放器添加"BigPlayButton"组件，它的元素会被插入到播放器的元素中
player.addChild('BigPlayButton');
```

在 DOM 中的结果看起来会像这样：

```html
<!-- Player Element -->
<div class="video-js">
  <!-- BigPlayButton Element -->
  <div class="vjs-big-play-button"></div>
</div>
```

相反，移除一个子组件也会从 DOM 中移除它的元素：

```javascript
player.removeChild('BigPlayButton');
```

在 DOM 中的结果看起来会像这样：

```html
<!-- Player Element -->
<div class="video-js">
</div>
```



### 使用 Options

给子组件构造函数传递 options 。

```javascript
var player = videojs('some-vid-id');
var Component = videojs.getComponent('Component');
var myComponent = new Component(player);
var myButton = myComponent.addChild('MyButton', {
  text: 'Press Me',
  buttonChildExample: {
    buttonChildOption: true
  }
});
```

初始化组件时，也可以通过 options 添加子组件。

> NOTE：如果两个相同类型的子组件拥有不同的 options 的话，将使用到 'name' 键。

```javascript
// MyComponent 来自上面的例子
var myComp = new MyComponent(player, {
  children: ['button', {
    name: 'button',
    someOtherOption: true
  }, {
    name: 'button',
    someOtherOption: false
  }]
});
```



## 事件监听

### 使用 `on`

```javascript
var player = videojs('some-player-id');
var Component = videojs.getComponent('Component');
var myComponent = new Component(player);
var myFunc = function() {
  var myComponent = this;
  console.log('myFunc called');
};

myComponent.on('eventType', myFunc);
myComponent.trigger('eventType');
// logs 'myFunc called'
```

`myFunc` 的上下文将会是 `myComponent` ，除非它被绑定。你可以将监听器添加到另外一个元素或者组件上。

```javascript
var otherComponent = new Component(player);

// myComponent/myFunc is from the above example
myComponent.on(otherComponent.el(), 'eventName', myFunc);
myComponent.on(otherComponent, 'eventName', myFunc);

otherComponent.trigger('eventName');
// logs 'myFunc called' twice
```

### 使用 `off`

```javascript
var player = videojs('some-player-id');
var Component = videojs.getComponent('Component');
var myComponent = new Component(player);
var myFunc = function() {
  var myComponent = this;
  console.log('myFunc called');
};
myComponent.on('eventType', myFunc);
myComponent.trigger('eventType');
// logs 'myFunc called'

myComponent.off('eventType', myFunc);
myComponent.trigger('eventType');
// does nothing
```

如果 myFunc 被排除，该事件类型的 *所有监听器* 都会被移除；如果 eventType 被排除，该组件的 *所有监听器* 都会被移除。你可以使用 `off` 来移除通过 `myComponent.on(otherComponents…` 添加在其他元素或组件上的监听器。

在这种情况下，事件类型和侦听器函数都是**必需**的。

```javascript
var otherComponent = new Component(player);

// myComponent/myFunc is from the above example
myComponent.on(otherComponent.el(), 'eventName', myFunc);
myComponent.on(otherComponent, 'eventName', myFunc);

otherComponent.trigger('eventName');
// logs 'myFunc called' twice
myComponent.off(ootherComponent.el(), 'eventName', myFunc);
myComponent.off(otherComponent, 'eventName', myFunc);
otherComponent.trigger('eventName');
// does nothing
```

### 使用 `one`

```javascript
var player = videojs('some-player-id');
var Component = videojs.getComponent('Component');
var myComponent = new Component(player);
var myFunc = function() {
  var myComponent = this;
  console.log('myFunc called');
};
myComponent.one('eventName', myFunc);
myComponent.trigger('eventName');
// logs 'myFunc called'

myComponent.trigger('eventName');
// does nothing
```

你也可以向其他元素或组件添加只触发一次的监听器

```javascript
var otherComponent = new Component(player);

// myComponent/myFunc is from the above example
myComponent.one(otherComponent.el(), 'eventName', myFunc);
myComponent.one(otherComponent, 'eventName', myFunc);

otherComponent.trigger('eventName');
// logs 'myFunc called' twice

otherComponent.trigger('eventName');
// does nothing
```

### 使用 `trigger`

```javascript
var player = videojs('some-player-id');
var Component = videojs.getComponent('Component');
var myComponent = new Component(player);
var myFunc = function(data) {
  var myComponent = this;
  console.log('myFunc called');
  console.log(data);
};
myComponent.one('eventName', myFunc);
myComponent.trigger('eventName');
// logs 'myFunc called' and 'undefined'

myComponent.trigger({'type':'eventName'});
// logs 'myFunc called' and 'undefined'

myComponent.trigger('eventName', {data: 'some data'});
// logs 'myFunc called' and "{data: 'some data'}"

myComponent.trigger({'type':'eventName'}, {data: 'some data'});
// logs 'myFunc called' and "{data: 'some data'}"
```



## 默认组件树

Video.js 的默认组件结构看起来类型下面这样：

```
Player
├── MediaLoader (has no DOM element)
├── PosterImage
├── TextTrackDisplay
├── LoadingSpinner
├── BigPlayButton
├─┬ ControlBar
│ ├── PlayToggle
│ ├── VolumePanel
│ ├── CurrentTimeDisplay (hidden by default)
│ ├── TimeDivider (hidden by default)
│ ├── DurationDisplay (hidden by default)
│ ├─┬ ProgressControl (hidden during live playback)
│ │ └─┬ SeekBar
│ │   ├── LoadProgressBar
│ │   ├── MouseTimeDisplay
│ │   └── PlayProgressBar
│ ├── LiveDisplay (hidden during VOD playback)
│ ├── RemainingTimeDisplay
│ ├── CustomControlSpacer (has no UI)
│ ├── PlaybackRateMenuButton (hidden, unless playback tech supports rate changes)
│ ├── ChaptersButton (hidden, unless there are relevant tracks)
│ ├── DescriptionsButton (hidden, unless there are relevant tracks)
│ ├── SubtitlesButton (hidden, unless there are relevant tracks)
│ ├── CaptionsButton (hidden, unless there are relevant tracks)
│ ├── AudioTrackButton (hidden, unless there are relevant tracks)
│ └── FullscreenToggle
├── ErrorDisplay (hidden, until there is an error)
├── TextTrackSettings
└── ResizeManager (hidden)
```



## 特定组件详情

### Play Toggle

`PlayToggle` 拥有一个 `replay` 配置项来显示或隐藏重播图标，可以通过传递 `{replay: false}` 来进行设置。重播图标在视频播放完成后会默认显示。

关闭重播图标的例子：

```javascript
let player = videojs('myplayer', {
  controlBar: {
    playToggle: {
      replay: false
    }
  }
});
```

### Volume Panel

`VolumePanel` 包含了 `MuteToggle` 和 `VolumeControl` 组件，在不支持改变音量的情况下会被隐藏。`VolumePanel` 有一个很重要的配置项能够让 `VolumeControl` 在 `MuteToggle` 上垂直显示。可以通过 `VolumePanel` `{inline: false}` 来设置，因为默认情况下 `VolumeControl` 是通过 `{inline: true}` 来横向排列的。

一个纵向的 `VolumeControl` 的例子：

```javascript
let player = videojs('myplayer', {
  controlBar: {
    volumePanel: {
      inline: false
    }
  }
});
```

### Text Track Settings

文本轨道设置组件仅在使用模拟文本轨道时可用。

### Resize Manager

这个新组件是负责在播放器尺寸变化时触发 `playerresize` 事件的，它在可用或提供了 polyfill 的情况下使用 ResizeObserver 。如果使用了 ResizeObserver ，它不包含任何元素。如果 ResizeObserver 不可用，它会回退使用 ifream 元素并通过去抖处理程序来监听它的 resize 事件。

一个 ResizeObserver polyfill 可以像下面这样被传入：

```javascript
var player = videojs('myplayer', {
  resizeManager: {
    ResizeObserver: ResizeObserverPoylfill
  }
});
```

如果要强制使用 ifream ，可以给 `ResizeObserver` 传递  `null` ：

```javascript
var player = videojs('myplayer', {
  resizeManager: {
    ResizeObserver: null
  }
});
```

Resize Manager 也可以像这样被禁用：

```javascript
var player = videojs('myplayer', {
  resizeManager: false
});
```

