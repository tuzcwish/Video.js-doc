# Debugging

[TOC]

## Logging

Video.js 包含了 `videojs.log` ，一个围绕 [`console API`](https://developer.mozilla.org/en-US/docs/Web/API/Console) 子集轻量级包装器。可用的方法有 `videojs.log`，`videojs.log.debug` ，`videojs.log.warn` 和 `videojs.log.error` 。

### API Overview

下面的大多数方法的含义都相当不言自明了，完整的细节可以查看 [`API 文档`](https://docs.videojs.com/) 。

| 方法                         | 别名            | 匹配级别                      |
| ---------------------------- | --------------- | ----------------------------- |
| `videojs.log()`              | `console.log`   | all, debug, info              |
| `videojs.log.debug()`        | `console.debug` | all, debug                    |
| `videojs.log.warn()`         | `console.warn`  | all, debug, info, warn        |
| `videojs.log.error()`        | `console.error` | all, debug, info, warn, error |
| `videojs.log.createLogger()` | n/a             | n/a                           |
| `videojs.log.level()`        | n/a             | n/a                           |
| `videojs.log.history()`      | n/a             | n/a                           |
| `videojs.log.clear()`        | n/a             | n/a                           |
| `videojs.log.disable()`      | n/a             | n/a                           |
| `videojs.log.enable()`       | n/a             | n/a                           |
| `videojs.log.filter()`       | n/a             | n/a                           |

关于这些功能的说明，请参阅以下部分。

### 安全地 Log

和 `console` 不同的是，在你的代码中保留 `videojs.log` 的调用是安全的，它们在 `console` 不存在时不会抛出异常。

### Log 有用的对象

和 `console` 类似的是，任意数量的混合类型的值都可以被传入到 `videojs.log` 方法。

```javascript
videojs.log('this is a string', {butThis: 'is an object'});
```

### 创建新的 Logger

有时候，你可能想要构造一个新的模块或者插件，并记录一些带有标签的信息。你可以使用 `createLogger` 方法来实现让所有类型的 logs 都添加 `VIDEOJS:` ，它需要一个名称，然后返回一个类似 `videojs.log` 的 log 对象。这里是一个例子：

```javascript
const mylogger = videojs.log.createLogger('mylogger');

mylogger('hello world!');
// > VIDEOJS: mylogger: hello world!

// We can even chain it further
const anotherlogger = mylogger.createLogger('anotherlogger');

anotherlogger('well, hello there');
// > VIDEOJS: mylogger: anotherlogger: well, hello there
```

### Log 级别

和 `console` 不一样的是，`videojs.log` 包好一个 log 级别的概念。这些级别控制了 log 方法的开关。

级别通过 `videojs.log.level` 暴露出来，这个方法充当了当前 log 级别的 getter 和 setter 。它没有任何参数的时候返回当前的 log 级别。

```javascript
videojs.log.level(); // "info"
```

通过传递一个字符串，可以切换到一个可用的 log 级别。

```javascript
videojs.log.level('error'); // show only error messages and suppress others
videojs.log('foo'); // does nothing
videojs.log.warn('foo'); // does nothing
videojs.log.error('foo'); // logs "foo" as an error
```

### 可用的 Log 级别

* **info**(默认): 只显示 `log` ，`log.wran ` ，`log.error` 消息；
* **all**: 启用所有的 log 方法；
* **error**: 只显示 `log.error` 消息；
* **off**: 禁用所有的 log 方法；
* **warn**: 只显示 `log.warn` 和 `log.error` 消息；
* **debug**: 显示 `log` ，`log.debug` ，`log.warn` 和 `log.error` 消息 

### 调试 Log

尽管 log 级别尝试去匹配 `window.console` 的对应项，但是 `window.console.debug` 并不是在所有平台上都可用。因此，它会去使用最接近的可比方法，从 `window.console.debug` 到 `window.console.info` 到 `window.console.log` 逐渐回退，最终如果这些方法都不可用的话，它会成为空。

### 历史

> NOTE: 在 video.js 5 里，`videojs.log.history` 是一个数组。到了 video.js 6 ，它是一个会返回数组的方法。这个改动是为了提供一个更丰富、更安全的 log 历史 API 。你也可以基于 logger 的名称来对 log 历史进行过滤。

在默认情况下，`videojs.log` 模块会忽视 log 级别追踪被传递进来的所有历史。

```javascript
videojs.log.history(); // an array of everything that's been logged up to now
```

甚至当 log 被设置为 **off** 也会继续生效。

这个很有用，但也可能成为内存泄漏的来源。例如：log 的对象会在历史中保留，即使它的引用已经从其他地方都移除了。

为了避免这个问题，可以通过函数的调用来禁用或者启用历史（分别使用 `disable` 和 `enable` 方法）。禁用历史很简单：

```javascript
videojs.log.history.disable();
```

最后，可以在任何时候清除历史（如果被启用的话）：

```javascript
videojs.log.history.clear();
```

#### 过滤历史

如果你想找到通过一个特定 logger 生成的所有历史，你可以通过 `history.filter()` 实现。指定一个 logger 名称 `foo` ，把它传递给 `history.filter()` 就能得到通过 foo 记录的所有项。下面是一个例子：

```javascript
const mylogger = videojs.log.createLogger('mylogger');
const anotherlogger = mylogger.createLogger('anotherlogger');

videojs.log('hello');
mylogger('how are you');
anotherlogger('today');

videojs.log.history.filter('VIDEOJS');
// > [['VIDEOJS:', 'hello'], ['VIDEOJS: mylogger:', 'how are you'], ['VIDEOJS: mylogger: anotherlogger:', 'today']]
videojs.log.history.filter('mylogger');
// > [['VIDEOJS:    mylogger:', 'how are you'], ['VIDEOJS: mylogger: anotherlogger:', 'today']]
videojs.log.history.filter('anotherlogger');
// > [['VIDEOJS: mylogger: anotherlogger:', 'today']]
```

