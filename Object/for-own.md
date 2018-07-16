## for-own

<https://github.com/jonschlinkert/for-own>

在 for-in 基础上包装一层，只遍历对象自有属性。

```js
'use strict';

var forIn = require('for-in');
var hasOwn = Object.prototype.hasOwnProperty;
// 在 Object 原型链上调用 hasOwnProperty 可以避免对象原型上同名方法劫持
module.exports = function forOwn(obj, fn, thisArg) {
  forIn(obj, function(val, key) {
    if (hasOwn.call(obj, key)) {
      return fn.call(thisArg, obj[key], key, obj);
    }
  });
};

// 感觉作者是为了自己 for-in 包的下载，强行增加了两次函数调用（forIn，然后 forIn 的 fn，然后才是 hasOwn），很恶心
// 不用 for-in 包
function forOwn(obj, fn, thisArg) {
  for (var key in obj) {
    if (!obj.hasOwnProperty(key)) continue
    if (fn.call(thisArg, obj[key], key, obj) === false) break
  }
}
```