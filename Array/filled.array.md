## filled-array

填充数组的方法，源码很漂亮了，没啥好说的

```js
'use strict';
module.exports = function (item, n) {
	var ret = new Array(n);
	var isFn = typeof item === 'function';

	if (!isFn && typeof ret.fill === 'function') {
		return ret.fill(item);
	}

	for (var i = 0; i < n; i++) {
		ret[i] = isFn ? item(i, n, ret) : item;
	}

	return ret;
};
```