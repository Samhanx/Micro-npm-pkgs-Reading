## decamelize

将驼峰形式的字符串转换为全小写和分隔符连接的字符串，默认分隔符是下划线

比如：`userInfo` => `user_info`

用到了 xregexp 这个正则库

[xregexp 文档](http://xregexp.com/)
[xregexp github](https://github.com/slevithan/xregexp)

简单看了下，这个正则库非常强大，在兼容 js 正则对象的同时，扩展了很多匹配语法和 api，比如下面这个正则的匹配，在 js 里就是不支持的

```js
'use strict';
const xRegExp = require('xregexp');

module.exports = (text, separator) => {
  // 参数校验
	if (typeof text !== 'string') {
		throw new TypeError('Expected a string');
	}
  // 默认分隔符
	separator = typeof separator === 'undefined' ? '_' : separator;

  // 这个正则匹配 userInfo 这类驼峰形式，分两个部分
  // \p{Ll}\d 匹配一个小写字母或者一个数字
  // \p{Lu}) 匹配一个大写字母
  // userInfo 为例，匹配结果就是 rI, $1 => r, $2 => I
	const regex1 = xRegExp('([\\p{Ll}\\d])(\\p{Lu})', 'g');
  // 这个正则匹配连续大写字母的情况，比如 xxxxUSAUserInfoXX
  // \p{Lu}+ 匹配一个或者多个大写字母
  // \p{Lu}[\p{Ll}\d]+ 匹配一个大写字母然后跟随任意小写字母或者数字
  // xxxxUSAUserInfoXX 为例，匹配的结果就是 USAUser, $1 => USA, $2 => User
	const regex2 = xRegExp('(\\p{Lu}+)(\\p{Lu}[\\p{Ll}\\d]+)', 'g');
  // xRegExp 方法第一个参数是正则字符串或者正则表达式对象，第二个参数是标记字符串，在正则对象的标记符基础上又扩充了一些，g 还是代表 global
  // 返回了一个 js 可用的正则对象

	return text
		// TODO: Use this instead of `xregexp` when targeting Node.js 10:
		// .replace(/([\p{Lowercase_Letter}\d])(\p{Uppercase_Letter})/gu, `$1${separator}$2`)
		// .replace(/(\p{Lowercase_Letter}+)(\p{Uppercase_Letter}[\p{Lowercase_Letter}\d]+)/gu, `$1${separator}$2`)
    // 个人猜测作者还是想在 node10 之后用更简便的方法去掉对外部包的依赖
    // replace 方法传入正则，$1 - $9 表示每个括号内匹配的内容，$1-9 这种使用方式尽管几乎所有浏览器都支持，但并不是标准包含的内容
    // 第一次替换先在前小写后大写的字母之间添加分隔符，第二次替换处理多个连续大写字母后跟随小写字母或数字的情况，在连续大写字母的最后一个字母前添加分隔符
    // xxxxUSAUserInfoXX 为例
		.replace(regex1, `$1${separator}$2`)
    // xxxx_USAUser_Info_XX
		.replace(regex2, `$1${separator}$2`)
    // xxxx_USA_User_Info_XX
		.toLowerCase();
    // xxxx_usa_user_info_xx
};
```