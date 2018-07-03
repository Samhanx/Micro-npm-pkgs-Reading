## map-obj

```js
'use strict';

// customized for this use-case
// 对象检测，平时很少会检测正则，错误，日期这三个内置对象
// 突然想到，在浏览器，其实 document 也是一个对象，在浏览器场景下，某些方法严格来说也要考虑
// 比如检测这两个对象构造方法 HTMLDocument HTMLElement
const isObject = x =>
	typeof x === 'object' &&
	x !== null &&
	!(x instanceof RegExp) &&
	!(x instanceof Error) &&
	!(x instanceof Date);

module.exports = function mapObj(obj, fn, opts, seen) {
	opts = Object.assign({
		deep: false,
		target: {}
	}, opts);

	seen = seen || new WeakMap();

	if (seen.has(obj)) {
		return seen.get(obj);
	}
	// 用 weakmap 做一个数据缓存，相比 obj 更不易出现内存泄漏
	// weakmap 只能用对象做 key

	seen.set(obj, opts.target);

	const target = opts.target;
	delete opts.target; // delete opts.target 清空避免下面递归传递时冲突吧

	for (const key of Object.keys(obj)) {
		const val = obj[key];
		const res = fn(key, val, obj);
		let newVal = res[1];

		if (opts.deep && isObject(newVal)) {
			// 数组和对象的 deep copy 处理
			if (Array.isArray(newVal)) {
				newVal = newVal.map(x => isObject(x) ? mapObj(x, fn, opts, seen) : x);
			} else {
				newVal = mapObj(newVal, fn, opts, seen);
			}
		}

		target[res[0]] = newVal;
		// 要求 mapper function 返回一个 [key, val] 的数组
	}

	return target;
};
```

看了这个包的历史 commit，可以看出来也是一步步完善到这个样子的，最开始包括对象的检测，下面的 deep 处理等都没有
