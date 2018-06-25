## array-first

获取数组第一个（或者前n个）元素，其实 Array.prototype.slice 已经可以实现且兼容性很好了

```js
// 引入了作者自己的另外两个包。。。对应包源码的评注
var isNumber = require('is-number');
var slice = require('array-slice');

module.exports = function arrayFirst(arr, num) {
  if (!Array.isArray(arr)) {
    throw new Error('array-first expects an array as the first argument.');
  }

  if (arr.length === 0) {
    return null;
  }

  // 同 slice，原生方法已经是 Full support 了, 看不出来作者造这个轮子有什么意义
  var first = slice(arr, 0, isNumber(num) ? +num : 1);
  // 目前看来，下面这个条件应该提到 first 上面来 直接返回 arr[0] 比较好
  if (+num === 1 || num == null) {
    return first[0];
  }
  return first;
};
```