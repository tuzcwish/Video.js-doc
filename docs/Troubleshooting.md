# 排除故障

## 媒体格式问题

### 选择视频格式

#### 我想要单一的源而且不考虑直播/自适应流

目前大多数浏览器使用 [h264 编码 MP4](https://caniuse.com/#feat=mpeg4) 视频。如果你只要一个源而且直播流和自适应不在考虑之中的话，h264 编码的 MP4 是一个不错的选择。

不支持 MP4 的浏览器大多是基于 Linux 的，这需要用户安装额外的解码器，有时候用户不想这么做，你可以提供一些备用的源。[webm](https://caniuse.com/#feat=webm) 和/或者 [ogv](https://caniuse.com/#feat=ogv) 是不错的回退选择，但是这两者在支持 MP4 的浏览器上都不被支持。

#### 我需要自适应流或者直播流

Video.js 7+ 包含了 [http-streaming](https://github.com/videojs/http-streaming) ， 因此能够支持 HLS 和 MPEG-DASH，在现代浏览器中它使用[媒体源扩展](https://caniuse.com/#feat=mediasource)来播放这些格式。如果选择单一格式，HLS 更方便，因为 iOS 和一些不支持 MSE 的 Android 浏览器都原生支持 HLS。

如果添加 [flashls 源处理程序](https://github.com/brightcove/videojs-flashls-source-handler)，不支持MSE 的 Windows 7 IE11上的 Flash 也可以使用 HLS。

对于较旧的 Video.js 版本，[http-streaming](https://github.com/videojs/http-streaming) 或其前身 [videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls) 和 [videojs-contrib-dash](https://github.com/videojs/videojs-contrib-dash) 添加了类似的支持，但为了获得最佳效果，请使用最新的 Video.js。

### 确保你使用的是 Video.js 可以播放的格式

* 你使用的浏览器/操作系统是否支持你想要使用的视频格式？
* 你是否有给媒体格式提供支持的 Video.js 插件，例如 [videojs-youtube](https://github.com/videojs/videojs-youtube) ？
* 确保你给视频设置了正确的 [mime-type/content-type](https://www.iana.org/assignments/media-types/media-types.xhtml#video) 。这用来确定 Video.js 是否能够播放一种格式的媒体文件。

### 确保文件容器中使用的编码是被支持的

* MP4 格式可以包含很多编码类型的视频和音频数据，但是浏览器一般只能支持 h264 编码的 MP4 格式视频和 MP3 或者 AAC 格式的音频。
* 文件扩展名并不总是能反映文件的内容。例如一些低端手机以 3GP 格式保存视频，但提供了 MP4 扩展，这些视频不能播放。

### 如果你使用的是 Flash 视频：

* 考虑使用其他的格式。有些浏览器不支持 Flash ，有些用户也不会安装它。即使安装了，浏览器也会让 Flash 越来越难以使用，比如让用户手动选择在每个站点上启用 Flash 。
* 确保添加了 [Flash 技术](https://github.com/videojs/videojs-flash)，这在 Video.js 5 以及更早的版本是默认包含的。
* Flash 媒体格式包括 RTMP 流和 FLV 格式媒体。
* SWF 不是一种媒体格式。

## 托管媒体时出现的问题

* 你的服务器必须正确支持 Chrome 和 Safari 依赖的 byte-range 请求：
  * 很多服务器是默认支持的；
  * 如果你通过服务器端脚本（PHP）代理媒体文件，则此脚本必须实现 range 。 PHP默认情况下不会这样做。
  * 如果没有这样做，影响范围从查找直到完全不能播放（在 iOS 上）
* 你的服务器必须给发送的媒体返回正确的 [mime-type/content-type](https://www.iana.org/assignments/media-types/media-types.xhtml#video) 头；
* 在以下情况，你的服务器必须实现 [CORS（跨域资源）](https://enable-cors.org/)头：
  * 你使用的是 HLS 或 MPEG-DASH 等格式，并且你的媒体是和网页在不同的域；
  * 你使用了[文字轨道（标题、字幕等）](https://github.com/videojs/video.js/blob/master/docs/guides/text-tracks.md)而且它和网页在不同的域。

## 全屏的问题

* 如果你的播放器在一个 iframe 中，那么该 iframe 和任何父 iframe 必须具有以下属性才能允许全屏：
  * `allowfullscreen`
  * `webkitallowfullscreen`
  * `mozallowfullscreen`

## 播放的问题

* 确保媒体服务器支持 byte-range 请求，这可能会阻断播放。更多信息查看上面“媒体托管时出现的问题”。
* 如果你的视频要等待很久才能开始播放或者播放前需要下载整个文件：
  * 可能是在媒体的初始部分没有包含媒体元数据。在 MP4 术语中，这被称为“ moov atom” ，许多编码器会默认执行此操作，其他编码器可能要求您选择“快速启动”或“优化流式传输”选项。

## Video.js 报错

### vdata123456 错误

如果一个和组件关联的 DOM 元素被移除，但是与它绑定的事件处理程序没有解绑的时候，会抛出这个错误。这几乎总是由于组件调用事件时，没有处理事件侦听。

要修复这个问题，就要确保所有的事件侦听器都已经被清除了。