---
created: 20211125 16:51
tags: [全栈]
---

# JavaScript 侦测手机浏览器的五种方法

### 1. navigator.userAgent
JS 通过`navigator.userAgent`属性拿到这个字符串，只要里面包含`mobi`、`android`、`iphone`等关键字，就可以认定是移动设备。
```javascript
if (/Mobi|Android|iPhone/i.test(navigator.userAgent)) {
  // 当前设备是移动设备
}

// 另一种写法
if (
  navigator.userAgent.match(/Mobi/i) ||
  navigator.userAgent.match(/Android/i) ||
  navigator.userAgent.match(/iPhone/i)
) {
  // 当前设备是移动设备
}
```
Chromium 系的浏览器，还有一个`navigator.userAgentData`属性，但是，苹果的 Safari 浏览器和 Firefox 浏览器都不支持这个属性。
```javascript
const isMobile = navigator.userAgentData.mobile; 
```
#####  缺点：
不可靠，因为用户可以修改这个字符串，让手机浏览器伪装成桌面浏览器。

### 2. window.screen，window.innerWidth
```javascript
if (window.screen.width < 500) {
  // 当前设备是移动设备 
}
```
##### 缺点：
如果手机横屏使用，就识别不了。
```javascript
const getBrowserWidth = function() {
  if (window.innerWidth < 768) {
    return "xs";
  } else if (window.innerWidth < 991) {
    return "sm";
  } else if (window.innerWidth < 1199) {
    return "md";
  } else {
    return "lg";
  }
};
```

### 3. window.orientation
`window.orientation`属性用于获取屏幕的当前方向，只有移动设备才有这个属性，桌面设备会返回`undefined`。
```javascript
if (typeof window.orientation !== 'undefined') {
  // 当前设备是移动设备 
}
```
##### 缺点：
iPhone 的 Safari 浏览器不支持该属性。

### 4. touch 事件
手机浏览器的 DOM 元素可以通过`ontouchstart`属性，为`touch`事件指定监听函数。桌面设备没有这个属性。
```javascript
function isMobile() { 
  return ('ontouchstart' in document.documentElement); 
}

// 另一种写法
function isMobile() {
  try {
    document.createEvent("TouchEvent"); return true;
  } catch(e) {
    return false; 
  }
}
```

### 5. window.matchMedia()
`window.matchMedia()`方法接受一个 CSS 的 media query 语句作为参数，判断这个语句是否生效。该对象的`matches`属性是一个布尔值。如果是`true`，就表示查询生效，当前设备是手机。
```javascript
let isMobile = window.matchMedia("only screen and (max-width: 760px)").matches;

// or CSS 语句`pointer:coarse`表示当前设备的指针是不精确的。由于手机不支持鼠标，只支持触摸，所以符合这个条件。
let isMobile = window.matchMedia("(pointer:coarse)").matches;

// or 有些设备支持多种指针，比如同时支持鼠标和触摸。`pointer:coarse`只用来判断主指针，此外还有一个`any-pointer`命令判断所有指针。`any-pointer:coarse`表示所有指针里面，只要有一个指针是不精确的，就符合查询条件。
let isMobile = window.matchMedia("(any-pointer:coarse)").matches;
```

### 6. 工具包
除了上面这些方法，也可以使用别人写好的工具包。这里推荐 [react-device-detect](https://www.npmjs.com/package/react-device-detect)，它支持多种粒度的设备侦测。
```javascript
import {isMobile} from 'react-device-detect';

if (isMobile) {
  // 当前设备是移动设备
}
```

## 总结
第 4、6 中方法最好，因为没有兼容性问题，第 5 种方法需要利用 CSS Media Query，对此不熟悉的人慎用。