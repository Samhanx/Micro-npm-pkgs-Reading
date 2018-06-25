## is-number

判断一个值是不是数字。因为 JS 对于 NaN 认为是 Number 类型，使用 Number 在检测像空字符串之类的边界值的时候也会转成0

这在判断是否是数字 Value 的时候会带来一些困惑

比如：

```js
console.log(Number('')) // 0
console.log(Number([])) // 0
```

```js
module.exports = function(num) {
  // 首先检测 Number 类型
  if (typeof num === 'number') {
    return num - num === 0; // NaN - NaN === NaN -> false
  }
  // 然后检测数字字符串
  if (typeof num === 'string' && num.trim() !== '') { // 非空
    // Number.isFinite 是 es6 提供的方法 如果不支持，就调用原本在 window 上的
    // 排除非空字符串以后，就是三种可能，用 +将字符串转成数字，纯数字字符串（等价数字）、数字和其他字符混合(NaN)、纯其他字符(NaN)
    // 其实用 +转换这一步好像是可以不用的， isFinite本身会转换
    return Number.isFinite ? Number.isFinite(+num) : isFinite(+num);
  }
  // 其他情况直接返回 false
  return false;
};
```