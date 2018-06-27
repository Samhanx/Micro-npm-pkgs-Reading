## array-last

获取数组最后一个（或者后n个）元素

同 slice， 原生方法已经是 Full support 了, 看不出来作者造这个轮子有什么意义

作者竟然还说这个库比原生 slice 快， benchmark 跑不起来不说，自己随便测一下明显也是原生快

这几个质量 array 包都是同一个作者，可怕的是每个月还有几百k的下载量

```js
var isNumber = require('is-number');

module.exports = function last(arr, n) {
  if (!Array.isArray(arr)) {
    throw new Error('expected the first argument to be an array');
  }

  var len = arr.length;
  if (len === 0) {
    return null;
  }

  n = isNumber(n) ? +n : 1;
  if (n === 1) {
    return arr[len - 1];
  }

  // 倒数 while 循环赋值数组的方式还是有一点巧妙
  var res = new Array(n);
  while (n--) {
    res[n] = arr[--len];
  }
  return res;
};
```