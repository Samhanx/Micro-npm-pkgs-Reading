## array-slice

其实就是原生 Array.prototype.slice 的功能，也是 lodash.slice 的功能

原生方法已经是 Full support 了, 看不出来作者造这个轮子有什么意义

作者提到某些情况下比原生快，但是也没有提供相关的用例和 benchmark，我是不信的

多一层封装怎么可能比直接在语言层面调用 api 更快，除非语言本身的实现是 polyfill 还有可能

```js
module.exports = function slice(arr, start, end) {
  var len = arr.length;
  var range = [];

  start = idx(len, start);
  end = idx(len, end, len);

  while (start < end) {
    range.push(arr[start++]);
  }
  return range;
};

// 计算实际下标
function idx(len, pos, end) {
  // 从调用来看 end 就是 arr.length 或者 undefined
  // pos 是 slice 参数的 start 或者 end
  if (pos == null) {
    // 如果 slice 未传 end， end 对应的 pos 就是 undefined
    // pos 就是 arr.length
    pos = end || 0;
  } else if (pos < 0) {
    // only if pos < 0 => pos + len 就是做减法且可能 < 0
    // 即 slice 参数的 start < 0 从数组右侧往回取
    // 最小取到 0
    pos = Math.max(len + pos, 0);
  } else {
    // pos >= 0 取 pos 最大取到 len
    pos = Math.min(pos, len);
  }

  return pos;
}
```