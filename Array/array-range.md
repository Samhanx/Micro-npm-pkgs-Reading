## array-range

生成递增数字 array 的封装方法

```js
module.exports = function newArray(start, end) {
  // 从调用来看 start 是起始下标，end 是从0开始的 arr.length

  // 过滤 number 类型
  var n0 = typeof start === 'number',
      n1 = typeof end === 'number'

  if (n0 && !n1) {
    end = start
    start = 0
  } else if (!n0 && !n1) {
    start = 0
    end = 0
  }

  // 通过位运算 过滤浮点数和 NaN 这个方法不错
  start = start|0
  end = end|0
  var len = end-start // 首尾相减算出实际的 arr.length （根据 start
  if (len<0)
    throw new Error('array length must be positive')
  
  var a = new Array(len)
  for (var i=0, c=start; i<len; i++, c++)
    a[i] = c
  // 生成了一个从 start 作为开始元素的数组，长度是 end - start，元素从 start 递增
  return a
}
```