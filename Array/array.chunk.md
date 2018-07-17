## array.chunk

<https://github.com/zhiyelee/array.chunk>

数组按传入的 size 值进行切分的方法。

```js
/**
 * get type string
 * @param val
 * @returns {string}
 */
function getType(val) {
  // `[object Array]`.slice(8, -1) ==> Array
  return Object.prototype.toString.call(val).slice(8, -1);
}

/**
 * check whether arr is an Array of TypedArray
 * @param arr
 * @return bool
 */
function isArray(arr) {
  var type = getType(arr);
  var arrTypes = [
    'Array',
    'Int8Array',
    'Uint8Array',
    'Uint8ClampedArray',
    'Int16Array',
    'Uint16Array',
    'Int32Array',
    'Uint32Array',
    'Float32Array',
    'Float64Array'
  ];

  return arrTypes.indexOf(type) !== -1;
}
// 以上检测类型

module.exports = function chunks(arr, size) {
  if (!isArray(arr)) {
    throw new TypeError('Input should be Array or TypedArray');
  }

  if (typeof size !== 'number') {
    throw new TypeError('Size should be a Number');
  }
  // 调用 TypeError 构造函数抛出提示更明确的错误

  var result = [];
  // 通过步进为 size 的循环
  for (var i = 0; i < arr.length; i += size) {
    // 每次从 i 开始，截取到 end 索引为 size + i 的元素
    // size = 3  0 => 3, 3 => 6, 6 => 9
    // [0, 3) [3, 6) [6, 9)
    if (typeof arr.slice === 'function') {
      result.push(arr.slice(i, size + i));
    } else {
      // subarray 是 TypedArray.prototype 上的方法，用法和 slice 一样
      result.push(arr.subarray(i, size + i));
    }
  }

  return result;
};
```