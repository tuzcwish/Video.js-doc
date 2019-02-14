# Audio Tracks

音轨(Audio tracks) 是 HTML5 视频提供给用户选择备用音轨的功能，可以播放主轨道以外轨道的音频。Video.js 提供了一个跨浏览器的音轨的实现。



##注意事项

* 音轨无法像文字轨道那样通过 HTML 添加，必须以编程的方式添加它们；
* Video.js 仅保存音轨的表现，切换用于播放的音轨不由 Video.js 处理，必须在其他的地方处理。比如：[videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls) 处理音轨的切换来支持通过 UI 选择轨道。



##使用 Audio Tracks

### 向播放器添加 Audio Track

```javascript
// Create a player.
var player = videojs('my-player');

// Create a track object.
var track = new videojs.AudioTrack({
  id: 'my-spanish-audio-track',
  kind: 'translation',
  label: 'Spanish',
  language: 'es'
});

// Add the track to the player's audio track list.
player.audioTracks().addTrack(track);
```

### 监听 Audio Track 启用

当一个音轨在 `AudioTrackList` 上被启用或禁用，会触发一个 `change` 的事件，你可以监听它然后处理你自己的逻辑。

> NOTE: 初始的轨道选择（通常是选定的主轨道）不应触发 `change` 事件。

```javascript
// Get the current player's AudioTrackList object.
var audioTrackList = player.audioTracks();

// Listen to the "change" event.
audioTrackList.addEventListener('change', function() {

  // Log the currently enabled AudioTrack label.
  for (var i = 0; i < audioTrackList.length; i++) {
    var track = audioTrackList[i];

    if (track.enabled) {
      videojs.log(track.label);
      return;
    }
  }
});
```

### 从播放器移除 Audio Track

如果你想从播放器删除一个 Audio track，只需要像下面这样：

```javascript
// Get the track we created in an earlier example.
var track = player.audioTracks().getTrackById('my-spanish-audio-track');

// Remove it from the audio track list.
player.audioTracks().removeTrack(track);
```



## API

查看完整信息，可以参考 [Video.js API docs](https://docs.videojs.com/) ，特别是：

* `Player#audioTracks`
* `AudioTrackList`
* `AudioTrack`

### `videojs.AudioTrack`

这是基于 [AudioTrack 标准](https://html.spec.whatwg.org/multipage/embedded-content.html#audiotrack) 实现的类，用来创建一个新的 audio track 对象。

下面的每个属性都是 `AudioTrack` 构造函数的配置参数。

#### `id`

> 查看[标准定义](https://html.spec.whatwg.org/multipage/embedded-content.html#dom-audiotrack-id)

轨道唯一标识，如果没有传入的话，Video.js 会自动生成一个。

#### `kind`

> 查看[标准定义](https://html.spec.whatwg.org/multipage/embedded-content.html#dom-audiotrack-kind)

Video.js 支持 `AudioTracks` 的标准 `kind` 值：

* `"alternative"`：主轨道的可替代方案；
* `"descriptions"`：视频轨道的音频描述；
* `"main"`：视频的主音轨；
* `"main-desc"`：混合音频描述的主音轨；
* `"translation"`：主音轨的翻译版本；
* `"commentary"`：主音轨的评论，例如导演的点评；
* `""`：默认值，没有明确的类型，或者轨道元数据给出的类型无法被浏览器识别。

#### `label`

> 查看[标准定义](https://html.spec.whatwg.org/multipage/embedded-content.html#dom-audiotrack-label)

向用户展示的标签，例如在菜单中列出所有不同语言的备用音轨。

#### `language`

> 查看[标准定义](https://html.spec.whatwg.org/multipage/embedded-content.html#dom-audiotrack-language)

该音轨的语言可用的 [`BCP 47码`](https://tools.ietf.org/html/bcp47)。如英语的 `"en"` 和西班牙语的 `"es"`。

#### `enabled`

> 查看[标准定义](https://html.spec.whatwg.org/multipage/embedded-content.html#dom-audiotrack-enabled)

是否应该播放该音轨。

在 Video.js 中，我们一次只允许启用一个音轨。因此，如果你启用多个音轨，最后启用的音轨将会成为唯一被启用的音轨。虽然规范允许启用多个轨道，但 Safari 和大多数实现仅允许一次启用一个音轨。

