## rtrim

看作者给出的 example 即可

```js
var rtrim = require( 'rtrim' );

/* Strip whitespace from the end of a string */
rtrim( '    Hello    ' ) + ' World' // →    Hello World

/* Strip multiple special chars from the end of a string */
rtrim( '... Hello World ...', ' .' ); // →... Hello World

/* Strip multiple chars from the end of a string */
rtrim( 'Hello World', 'Hdle' ); // →Hello Wor

/* Strip trailing slash from the end of a string */
rtrim( 'https://goo.gl/', '/' ); // →https://goo.gl
```

right trim，去除字符串右边的空格或者指定的字符

```js
"use strict";


/**
 * Strip whitespace - or other characters - from the end of a string
 *
 * @param  {String} str   Input string
 * @param  {String} chars Character(s) to strip [optional]
 * @return {String} str   Modified string
 */

module.exports = function ( str, chars ) {
  // 作者其实都注释得差不多了
  // Convert to string
  str = str.toString();

  // Empty string?
  if ( ! str ) {
    return '';
  }

  // Remove whitespace if chars arg is empty
  if ( ! chars ) {
    return str.replace( /\s+$/, '' ); // 学习一下用正则替换去除空格, $表示到字符串结束位置，然后匹配一个或多个空格
  }

  // Convert to string
  chars = chars.toString();

  // 把 str 拆成一个个字符的数组
  // Set vars
  var letters = str.split( '' ),
    i = letters.length - 1;

  // 从最后开始遍历
  // Loop letters
  for ( i; i >= 0; i -- ) {
    // str 中没有传入匹配串的任意字符时返回截取的字符串，可以看第三个 example
    if ( chars.indexOf( letters[i] ) === -1 ) {
      // 从 0 截取到 i + 1, 注意 substring 方法第一个参数是起始 index， 第二个参数是截止 index + 1 的位置，即截取的 length
      return str.substring( 0, i + 1 );
    }
  }

  return str;

};
```