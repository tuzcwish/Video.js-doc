# Event Target

## 概述

Video.js 里建立的事件模拟了用于对象上的 DOM API ，也拥有具有相同能力的简写方法。


## `on()` 与 `addEventListener()`

这个方法用于在一个 EventTarget 上添加一个事件侦听器。

```javascript
var foo = new EventTarget();
var handleBar = function() {
  console.log('bar was triggered');
};

foo.on('bar', handleBar);

// This causes any `event listeners` for the `bar` event to get called
// see EventTarget#trigger for more information
foo.trigger('bar');
// logs 'bar was triggered'
```


## `off()` 与 `removeEventListener()`

这个方法用于在一个 EventTarget 上移除一个事件侦听器。

```javascript
var foo = new EventTarget();
var handleBar = function() {
  console.log('bar was triggered');
};

// adds an `event listener` for the `bar` event
// see EventTarget#on for more info
foo.on('bar', handleBar);

// runs all `event listeners` for the `bar` event
// see EventTarget#trigger for more info
foo.trigger('bar');
// logs 'bar was triggered'

foo.off('bar', handleBar);
foo.trigger('bar');
// does nothing
```


## `one()`

这个方法用于添加一个只能被调用一次的事件侦听器。

使用 `on()` 和 `off()` 来模拟 `one()` （不推荐这样做）：

```javascript
var foo = new EventTarget();
var handleBar = function() {
  console.log('bar was triggered');
  // after the first trigger remove this handler
  foo.off('bar', handleBar);
};

foo.on('bar', handleBar);
foo.trigger('bar');
// logs 'bar was triggered'

foo.trigger('bar');
// does nothing
```

使用 `one()`

```javascript
var foo = new EventTarget();
var handleBar = function() {
  console.log('bar was triggered');
};

// removed after the first trigger
foo.one('bar', handleBar);
foo.trigger('bar');
// logs 'bar was triggered'

foo.trigger('bar');
// does nothing
```


## `trigger()` 与 `dispatchEvent()`
这个方法用于触发 EventTarget 上的事件，该事件将运行所有侦听器。

> Note:  如果 `EventTarget.allowedEvents_` 中包含 'click' ，那么触发器将在存在的情况下尝试调用 `onClick` 方法。

```javascript
var foo = new EventTarget();
var handleBar = function() {
  console.log('bar was triggered');
};

foo.on('bar', handleBar);
foo.trigger('bar');
// logs 'bar was triggered'

foo.trigger('bar');
// logs 'bar was triggered'

foo.trigger('foo');
// does nothing
```

