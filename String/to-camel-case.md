## to-camel-case

将含有特殊 case （驼峰，空格，分隔符）的字符串转为小驼峰字符串

```js
var space = require('to-space-case')

/**
 * Export.
 */

module.exports = toCamelCase

/**
 * Convert a `string` to camel case.
 *
 * @param {String} string
 * @return {String}
 */

function toCamelCase(string) {
  // 在  to-space-case 基础上，匹配空白符和一个字母或数字，然后替换为大写
  return space(string).replace(/\s(\w)/g, function (matches, letter) {
    return letter.toUpperCase()
  })
}
```