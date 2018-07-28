## to-no-case

移除字符串的所有特殊形式（驼峰大写和各种分隔符）

```js
/**
 * Export.
 */

module.exports = toNoCase

/**
 * Test whether a string is camel-case.
 */
// 驼峰，空格，分隔符校验正则
var hasSpace = /\s/
var hasSeparator = /(_|-|\.|:)/
var hasCamel = /([a-z][A-Z]|[A-Z][a-z])/

/**
 * Remove any starting case from a `string`, like camel or snake, but keep
 * spaces and punctuation that may be important otherwise.
 *
 * @param {String} string
 * @return {String}
 */

function toNoCase(string) {
  // 检验到有空格分割直接转成全小写返回
  if (hasSpace.test(string)) return string.toLowerCase()
  // 没有空格分割的基础上再检查分隔符，去分隔符后再转全小写返回
  if (hasSeparator.test(string)) return (unseparate(string) || string).toLowerCase()
  // 没有分隔符的基础上再检查驼峰，去驼峰后再转全小写返回
  if (hasCamel.test(string)) return uncamelize(string).toLowerCase()
  return string.toLowerCase()
}

/**
 * Separator splitter.
 */
// 分隔单词部分的匹配正则
var separatorSplitter = /[\W_]+(.|$)/g
// \W 相当于 [^a-zA-Z0-9_]，即除了字母数字和下划线之外的字符
// [\W_]+: 匹配一个以上除了字母和数字之外的字符
// (.|$): 匹配一个除换行符之外的任何单个字符或者单词结束
// 综合起来，这个正则匹配到分隔符连同下一个起始字符，或者分隔符连同结束位置

/**
 * Un-separate a `string`.
 *
 * @param {String} string
 * @return {String}
 */
// 去分隔符方法
// replace 传 callback 方法，replace 返回 callback 返回的内容
// callback 第一个参数是匹配到的字符串，如果用正则匹配
// 第 2 个开始随后的参数依次对应 $1 - $9（取决于正则有多少分组）
// 倒数第二个参数是匹配字符串在原字符串的偏移量，最后一个参数是原字符串
function unseparate(string) {
  return string.replace(separatorSplitter, function (m, next) {
    // 这里用 $1 取到 (.|$) 匹配的内容，相当于将匹配到的分隔符替换成了空格
    return next ? ' ' + next : ''
  })
}

/**
 * Camelcase splitter.
 */
// 驼峰单词部分的匹配正则
var camelSplitter = /(.)([A-Z]+)/g
// 一个换行符之外的字符，和一个以上的大写字母

/**
 * Un-camelcase a `string`.
 *
 * @param {String} string
 * @return {String}
 */

function uncamelize(string) {
  return string.replace(camelSplitter, function (m, previous, uppers) {
    // 将大写字母部分以空格分隔(多个连续大写字母的 case)，然后前面再加一个空格
    return previous + ' ' + uppers.toLowerCase().split('').join(' ')
  })
}
```