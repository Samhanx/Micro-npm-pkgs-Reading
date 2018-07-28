## to-space-case

将含有特殊 case （驼峰，空格，分隔符）的字符串转为小写空格分隔的字符串

```js
toSpaceCase('-RAnDom -jUNk$__loL!')  // "random junk lol"
```

这个示例比较能够说明情况

```js
// 使用了 to-no-case
var clean = require('to-no-case')

/**
 * Export.
 */

module.exports = toSpaceCase

/**
 * Convert a `string` to space case.
 *
 * @param {String} string
 * @return {String}
 */

function toSpaceCase(string) {
  // 这个正则在 to-no-case 中作为分隔符单词部分匹配正则已经分析过，相当于在 to-no-case 的基础上再过滤一遍特殊的分隔符，因为 to-no-case 首先以空格符作为判断
  return clean(string).replace(/[\W_]+(.|$)/g, function (matches, match) {
    return match ? ' ' + match : ''
  }).trim() // 最后去除两头空格
}
```