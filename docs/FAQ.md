# FAQ



## Video.js 是什么？

Video.js 是一个围绕原生 video 元素的扩展框架/库，它做了以下事情：

* 提供插件 API ，让原生 video 元素可以处理不同类型的视频（比如：[HLS](https://github.com/videojs/videojs-contrib-hls) ，[flash](https://github.com/videojs/videojs-flash) ，HTML5 video 等 ）；
* 统一跨浏览器的原生视频 API （给必要的功能提供 polyfill）；
* 提供可扩展的主题化 UI ；
* 确保键盘和屏幕阅读器用户的无障碍访问；
* 有一组核心插件，给其他格式的视频提供支持：
  * [videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls)
  * [videojs-contrib-dash](https://github.com/videojs/videojs-contrib-dash)
* 通过一个核心插件支持了 DRM 视频：
  * [videojs-contrib-eme](https://github.com/videojs/videojs-contrib-eme)
* 可扩展的大量插件和支持的所有功能，请查看 [Video.js 插件列表](https://videojs.com/plugins)。

## 如何安装 Video.js ？

现在 Video.js 可以使用 npm 进行安装，可以使用 GitHub 标签提供的发布文件，甚至可以使用 CDN 托管版本。更多信息可以查看[起步指南](https://videojs.com/getting-started/)。

## Video.js 是否支持使用 bower 安装？

Video.js 6 之前的版本是支持 bower 的，然而，从 Video.js 6 开始不再对 bower 提供官方支持。更多信息请查看 [https://github.com/videojs/video.js/issues/4012](https://github.com/videojs/video.js/issues/4012) 。

## Video.js 版本里的数字是什么意思？

Video.js 遵循 [semver](https://semver.org/lang/zh-CN) 规范，意味着除非主版本号增加， API 都应向下兼容。

## 如何解决播放问题

查看[排除故障指南](./Troubleshooting.md)，如果没有解决你的问题，请在 [Slack](https://videojs.slack.com/) 上提问或者提交 [issue](https://docs.videojs.com/tutorial-faq.html#q-i-think-i-found-a-bug-with-videojs-or-i-want-to-add-a-feature-what-should-i-do) 。

当你在寻求播放问题的帮助时，由于问题常常是特定的视频文件、托管视频的方式或者浏览器，因此请确保能提供关于这些的所有信息以及简化的测试用例。

## 一个视频在特定的浏览器无法播放，这是为什么？

查看[排除故障指南](./Troubleshooting.md)，如果没有解决你的问题，请在 [Slack](https://videojs.slack.com/) 上提问或者提交 [issue](https://docs.videojs.com/tutorial-faq.html#q-i-think-i-found-a-bug-with-videojs-or-i-want-to-add-a-feature-what-should-i-do) 。

## 为什么要在播放器下载整个视频？为什么视频加载时间很长？

查看[排除故障指南](./Troubleshooting.md)，如果没有解决你的问题，请在 [Slack](https://videojs.slack.com/) 上提问或者提交 [issue](https://docs.videojs.com/tutorial-faq.html#q-i-think-i-found-a-bug-with-videojs-or-i-want-to-add-a-feature-what-should-i-do) 。

## 抛出的异常提到了 `vdata12345` ，这是什么意思？

查看[排除故障指南](./Troubleshooting.md)，如果没有解决你的问题，请在 [Slack](https://videojs.slack.com/) 上提问或者提交 [issue](https://docs.videojs.com/tutorial-faq.html#q-i-think-i-found-a-bug-with-videojs-or-i-want-to-add-a-feature-what-should-i-do) 。

## 我觉得我发现了 Video.js 的一个 bug ，或者我想增加一个功能，我应该怎么做？

### 如果你能够自己修复这个问题或者增加功能

非常欢迎在 [Video.js repo](https://github.com/videojs/video.js/pulls) 提交 request ，请遵循[贡献指南](https://github.com/videojs/video.js/blob/master/CONTRIBUTING.md#contributing-code)以及[提交 request 模板](https://github.com/videojs/video.js/blob/master/.github/PULL_REQUEST_TEMPLATE.md) 。

### 如果你无法自己修复问题或者增加这个功能

[在 Video.js repo 上开一个 issue](https://github.com/videojs/video.js/issues) ，请遵循[issue 模板](https://github.com/videojs/video.js/blob/master/.github/ISSUE_TEMPLATE.md)和[贡献指南](https://github.com/videojs/video.js/blob/master/CONTRIBUTING.md#contributing-code)以便我们更好地帮助你解决问题。

## 什么是简化测试用例？

简化测试用例是将你遇到的问题单独隔离出来的一个示例，可以将其视为用尽可能最少的代码重现问题的示例页面。

附加简化测试用例是很重要的，即使这个问题对你来说显而易见，但对其他人来说可能不是的。有一个参考的例子也可以让其他人一眼就能看出问题所在，而不是需要花时间去重现你所描述的内容。

## Video.js 支持哪些媒体格式？

这取决于浏览器的HTML5 video 元素支持哪些格式以及 Video.js 使用的播放技术/插件，关于媒体格式的更多信息请查看[排除故障指南](./Troubleshooting.md)。

## Video.js 如何决定使用哪个源？

当提供了一个播放源的数组时，Video.js 会按照顺序来测试这些源。对于每个源，将检查 [`techOrder`](https://docs.videojs.com/tutorial-options.html#techorder) 中的每个技术，来看它是否可以直接或通过源处理程序（例如 videojs-contrib-hls）来播放，第一个匹配到的源将会被选中。

## 如何让视频自动播放？

由于近期自动播放表现上的改动，我们将不再推荐使用 `video` 元素的 `autoplay` 属性。它仍然被 Video.js 支持，但是很多浏览器，包括 Chrome ，都在改动 `autoplay` 属性的表现。

我们推荐使用 Video.js 的 `autoplay` 配置而不是 `autoplay` 属性，关于如何使用的更多信息，请查看 Video.js 的配置指南中 [autoplay option] [autoplay-option] 。

关于 autoplay 的改动的更多信息，可以查看我们的博客：[https://blog.videojs.com/autoplay-best-practices-with-video-js/](https://blog.videojs.com/autoplay-best-practices-with-video-js/) 。

## 如何在移动设备上自动播放视频？

直到现在，大多数移动设备都禁止了视频的自动播放功能。Video.js 在不支持自动播放的移动设备上也不支持自动播放，对那些支持自动播放的设备，例如 iOS 10 和 Chrome for Android 53+ ，你必须将视频静音或者没有音轨的视频才能自动播放。

我们不推荐在 `video` 元素上手动设置属性，相反，你应该传递值为 `'any'` 或者 `'muted'` 的 [autoplay option] [autoplay-option] 配置，相关的更多信息请查看上面那个链接。

> Note: 这时，autoplay 的配置或者属性并不能保证你的视频能够自动播放。

## Video.js 可以播放 RTMP 视频吗？

RTMP 格式的播放需要 Flash ，你需要一个支持 Flash 的浏览器和 Flash 技术。

Video.js 6 版本默认不包含 Flash ，而是提供一个单独的 [videojs-flash 包](https://github.com/videojs/videojs-flash) 。之前的版本，它是内置在 Video.js 中的。

RTMP 源应该设置一个适当的格式—— `rtmp/mp4` 或者 `rtmp/flv` 。请注意，Video.js 会将连接 URL 和流的名称以 `&` 符号分割开，例如：`rtmp://example.com/live&foo` 或者 `rtmp://example.com/fms&mp4:path/to/file.mp4` 。

如果服务器需要用于身份验证的查询参数，应当把这些参数添加到连接部分 URL ，例如：`rtmp://example.com/live?token=1234&foo` 。

请记住移动端浏览器不支持 Flash ，而现代浏览器也会增加使用 Flash 的难度或者默认替终端用户禁止 Flash 。考虑使用更加现在的格式，例如 HLS 或者 DASH 。

## 怎样将我的视频/字幕/音频/轨道的地址隐藏起来？

要隐藏浏览器发出的网络请求是不可能的，要充分混淆资源中的 URL 也很困难。token 校验技术可能有所帮助，但是这已经不是 Video.js 的范畴了。

对于必须高等安全的内容，[videojs-contrib-eme](https://github.com/videojs/videojs-contrib-eme) 会添加 DRM 支持。

## 我可以关闭 Video.js 的 log 吗？

是的！可以在引入 Video.js 之后，创建任何播放器之前，添加以下代码实现：

```javascript
videojs.log.level('off');
```

更多信息，包括哪些日志级别可用，请查看 [调试指南](./Debugging.md) 。

## 什么是插件？

插件是一组可供其他人重用的功能。例如一个插件可以给 Video.js 添加一个按钮，让视频重复播放10次然后再停止播放。如果存在这样的一个插件，并且已经发布的时候，其他人可以在他们的页面里引入这个插件，然后共享这个功能。

## 如何编写 Video.js 的插件？

编写 Video.js 插件的信息请查看 [插件指南](https://docs.videojs.com/tutorial-plugins.html)。

## 怎样给 Video.js 增加一个按钮？

给 Video.js 增加按钮的示例请查看 [组件指南](./Components.md)。

## 哪里可以找到 Video.js 的插件列表？

[videojs.com](https://videojs.com/plugins) 上维护了在 npm 已发布的带有 `videojs-plugin` 关键词的插件列表。

## 怎样把我的插件列在网站上？

在 [package.json 的 keyword 数组](https://docs.npmjs.com/files/package.json#keywords) 中添加 'videojs-plugin' 关键词，然后把你的包发布到 npm 。如果你使用了 [plugin generator](https://github.com/videojs/generator-videojs-plugin) ，它会帮你自动设置好。更多信息请查看 [插件指南](https://docs.videojs.com/tutorial-plugins.html)。

## 哪里可以找到 Video.js 的主题列表？

查看 [Video.js GitHub wiki](https://github.com/videojs/video.js/wiki/Skins) 。

## Video.js 可以只作为音频播放器来工作吗？

可以！它能够只播放 `<video>` 或 `<audio>` 标签中的音频文件。注意，只有音频的源不能适用于 Flash 播放技术。

## Video.js 支持音轨吗？

支持！如何使用音轨请查看[音轨指南](./AudioTrack.md)。

## Video.js 支持视频轨道吗？

备用视频轨道的支持正在开发中，如何使用视频轨道请查看[视频轨道指南](https://docs.videojs.com/tutorial-video-tracks.html)。

## Video.js 支持文字轨道（标题、字幕等）吗？

支持！如何使用文字轨道请查看[文字轨道指南](https://docs.videojs.com/tutorial-text-tracks.html)。

## Video.js 支持 HLS （HTTP 直播流）视频吗？

如果原生的 HTML5 支持 HLS 的话（例如在 Safari ，Edge ，Chrome for Android 以及 iOS 上），Video.js 就支持 HLS 视频，对于在不支持 HLS 的浏览器上，[videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls) 可以提供支持。

请注意，在没有原生支持 HLS 的情况下，托管视频的服务器必须设置 [CORS 头](https://enable-cors.org/)。

## Video.js 支持 MPEG DASH 视频吗？

MPEG DASH 视频的支持由 [videojs-contrib-dash 包](https://github.com/videojs/videojs-contrib-dash) 提供支持。

和 HLS 一样，DASH 流也需要设置 [CORS 头](https://enable-cors.org/)。

## Video.js 支持直播吗？

支持！直播的通用格式为 HLS 或者早期的 RTMP ，HLS 又 [videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls) 提供支持，而 RTMP 由 [videojs-flash](https://github.com/videojs/videojs-flash) 提供支持。

## Video.js 支持 YouTube 视频吗？

有一个官方插件提供支持，[videojs-youtube](https://github.com/videojs/videojs-youtube) 。

## Video.js 支持 Vimeo 视频吗？

有一个官方插件提供支持，[videojs-vimeo](https://github.com/videojs/videojs-vimeo) 。

## Video.js 支持 DRM 视频吗？

有一个官方插件提供支持，[videojs-contrib-eme](https://github.com/videojs/videojs-contrib-eme) 。

## Video.js 对广告集成有提供支持吗？

有一个官方插件提供了广告的核心支持，[videojs-contrib-ads](https://github.com/videojs/videojs-contrib-ads) ，在此基础上负责广告服务器通讯和广告展示的更进一步的插件，例如 [Google's IMA plugin](https://github.com/googleads/videojs-ima) 。

## Video.js 可以被导入到 node.js 里吗？

可以！Video.js 已经[在 npm 发布](https://www.npmjs.com/package/video.js)。

## Video.js 可以和 webpack 协同工作吗？

可以！查看 [webpack 和 Video.js 配置指南](https://docs.videojs.com/tutorial-webpack.html)。

## Video.js 可以在 react 里使用吗？

可以！查看 [ReactJS 集成示例](https://docs.videojs.com/tutorial-react.html)。