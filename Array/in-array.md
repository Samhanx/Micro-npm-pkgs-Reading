## in-array

检测一个值是否在数组里，简单测试了一下，遍历检查居然比 indexOf 快

代码比较简单，也做了边界判断

```js
'use strict';

module.exports = function inArray (arr, val) {
  arr = arr || [];
  var len = arr.length;
  var i;

  for (i = 0; i < len; i++) {
    if (arr[i] === val) {
      return true;
    }
  }
  return false;
};
```