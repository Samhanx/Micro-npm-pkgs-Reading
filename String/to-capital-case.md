## to-capital-case

将含有特殊 case （驼峰，空格，分隔符）的字符串转为首字母大写空格分隔的字符串

```js
var space = require('to-space-case')

/**
 * Export.
 */

module.exports = toCapitalCase

/**
 * Convert a `string` to capital case.
 *
 * @param {String} string
 * @return {String}
 */

function toCapitalCase(string) {
  // 在  to-space-case 基础上，匹配空起始位置或者一个空白符，然后匹配一个字母或数字
  return space(string).replace(/(^|\s)(\w)/g, function (matches, previous, letter) {
    // 替换为字母大写的匹配串
    return previous + letter.toUpperCase()
  })
}
```