## map-array

把一个对象的 key, value 组装转换为一个数组

用法一看就明白，跟 Array.prototype.map 不同，是对一个对象 map 然后返回数组

```js
const mapArray = require('map-array');
const obj = {
  giorgio: 'Bianchi',
  gino: 'Rossi'
};
console.log(mapArray(obj, (key, value) => key + ' ' + value)); // ['giorgio Bianchi', 'gino Rossi']
```

代码比较简单，就是对 map-obj 的调用

```js
'use strict';
const map = require('map-obj');

function mapToArray(obj, fn) {
	let idx = 0;
	const result = map(obj, (key, value) =>
		[idx++, fn(key, value)]
	);
	result.length = idx;
  // 调用 map-obj 转成一个 idx 作为 key 的对象，再给一个 length 属性
  // 通过 Array.from 转换为 array
	return Array.from(result);
}

module.exports = mapToArray;
```

目前看来，这个包也是毫无意义， Array.prototype.map 其实就足够了