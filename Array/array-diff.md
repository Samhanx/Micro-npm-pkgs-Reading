## arr-diff

diff array

```js
module.exports = function diff(arr/*, arrays*/) {
  var len = arguments.length;
  var idx = 0;
  // arrays 的处理，依次 diff
  while (++idx < len) {
    // 第一轮循环 idx 从1开始
    arr = diffArray(arr, arguments[idx]);
  }
  return arr;
};

function diffArray(one, two) {
  if (!Array.isArray(two)) {
    // 浅拷贝一份原数组
    return one.slice();
  }

  var tlen = two.length
  var olen = one.length;
  var idx = -1;
  var arr = [];

  // 做了一个 o(n^2) 的比较
  // 对 one 的每一个元素都遍历 two 来比较
  while (++idx < olen) {
    // 第一轮循环 idx 从0开始
    var ele = one[idx];
  
    var hasEle = false;
    for (var i = 0; i < tlen; i++) {
      var val = two[i];

      if (ele === val) {
        hasEle = true;
        break;
      }
    }

    if (hasEle === false) {
      arr.push(ele);
    }
  }
  return arr;
}
```