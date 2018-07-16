## for-in

<https://github.com/jonschlinkert/for-in>

对象的 for-in 循环包装了一层函数。仍然是遍历自有属性和继承的可枚举属性。

```js
'use strict';
// 第三个 this 参数
module.exports = function forIn(obj, fn, thisArg) {
  for (var key in obj) {
    // call 指定 this，没有则为 undefined
    // 非严格模式下，this 值为 null 或 undefined 时会指向全局对象 (window or global)
    // 如果函数返回 false 则循环立刻终止
    if (fn.call(thisArg, obj[key], key, obj) === false) {
      break;
    }
  }
};
```