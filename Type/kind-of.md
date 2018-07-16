## kind-of

目前看到的一个类型检测比较全的包。

```js
var toString = Object.prototype.toString;

module.exports = function kindOf(val) {
  if (val === void 0) return 'undefined'; // void 0 === undefined
  if (val === null) return 'null';

  var type = typeof val;
  if (type === 'boolean') return 'boolean';
  if (type === 'string') return 'string';
  if (type === 'number') return 'number';
  if (type === 'symbol') return 'symbol';
  // 到前面都是比较常规的 typeof 检测
  if (type === 'function') {
    // 检测了普通函数和 generator 函数
    return isGeneratorFn(val) ? 'generatorfunction' : 'function';
  }
  // 以上都没有直接判断 object

  if (isArray(val)) return 'array';
  if (isBuffer(val)) return 'buffer'; // 检测 buffer
  if (isArguments(val)) return 'arguments'; // 好像看不出有什么需要检测一个值是不是 arguments，注意不是单纯的是不是函数参数，见该函数注释
  if (isDate(val)) return 'date';
  if (isError(val)) return 'error';
  if (isRegexp(val)) return 'regexp';
  // 以上检测了各类对象，仍然没有直接判断 object

  // 如果上面都还不能返回，通过构造器名字再来判断一次
  switch (ctorName(val)) {
    case 'Symbol': return 'symbol';
    case 'Promise': return 'promise'; // 判断了 promise

    // map 系列
    // Set, Map, WeakSet, WeakMap
    case 'WeakMap': return 'weakmap';
    case 'WeakSet': return 'weakset';
    case 'Map': return 'map';
    case 'Set': return 'set';

    // 特殊的数组类型，我还没有见过。。。
    // 8-bit typed arrays
    case 'Int8Array': return 'int8array';
    case 'Uint8Array': return 'uint8array';
    case 'Uint8ClampedArray': return 'uint8clampedarray';

    // 16-bit typed arrays
    case 'Int16Array': return 'int16array';
    case 'Uint16Array': return 'uint16array';

    // 32-bit typed arrays
    case 'Int32Array': return 'int32array';
    case 'Uint32Array': return 'uint32array';
    case 'Float32Array': return 'float32array';
    case 'Float64Array': return 'float64array';
  }
  // 以上仍然是对各种 object 的检测

  // 检测 generator 对象
  if (isGeneratorObj(val)) {
    return 'generator';
  }

  // Non-plain objects
  type = toString.call(val); // toString 老办法来检测对象
  switch (type) {
    case '[object Object]': return 'object';
    // 检测迭代器 还有这么多可能
    // iterators
    case '[object Map Iterator]': return 'mapiterator';
    case '[object Set Iterator]': return 'setiterator';
    case '[object String Iterator]': return 'stringiterator';
    case '[object Array Iterator]': return 'arrayiterator';
  }

  // other
  // 从下标8开始到倒数第二个字符截断，转小写，再去掉空格
  // '[object Array Iterator]'.slice(8, -1).toLowerCase().replace(/\s/g, '') // "arrayiterator"
  return type.slice(8, -1).toLowerCase().replace(/\s/g, '');
};

// 返回函数构造器 name
function ctorName(val) {
  return val.constructor ? val.constructor.name : null;
}

function isArray(val) {
  // 优先 isArray 方法，其次 instanceof 来检测实例
  if (Array.isArray) return Array.isArray(val);
  return val instanceof Array;
}

function isError(val) {
  // 检测是不是 Error 的实例，或者是否并存 message 属性，对象构造器以及构造器上的 stackTraceLimit 属性值为数字类型
  return val instanceof Error || (typeof val.message === 'string' && val.constructor && typeof val.constructor.stackTraceLimit === 'number');
}

function isDate(val) {
  // 检测是不是 Date 的实例，或者是否并存下面三个 date 方法
  if (val instanceof Date) return true;
  return typeof val.toDateString === 'function'
    && typeof val.getDate === 'function'
    && typeof val.setDate === 'function';
}

function isRegexp(val) {
  // 检测是不是 RegExp 的实例，或者下面四个属性的值类型是否一致
  if (val instanceof RegExp) return true;
  return typeof val.flags === 'string'
    && typeof val.ignoreCase === 'boolean'
    && typeof val.multiline === 'boolean'
    && typeof val.global === 'boolean';
}

function isGeneratorFn(name, val) {
  // generator 函数的构造器 name 是 GeneratorFunction
  return ctorName(name) === 'GeneratorFunction';
}

function isGeneratorObj(val) {
  // 对象上存在 throw return next 三个方法，视为 generator 对象
  return typeof val.throw === 'function'
    && typeof val.return === 'function'
    && typeof val.next === 'function';
}

// 如果是剩余参数的变量，是没有 callee 的，这个检测方法就不成立了
function isArguments(val) {
  try {
    // 有一个数字类型的 lenght 属性，有一个 callee 的方法
    if (typeof val.length === 'number' && typeof val.callee === 'function') {
      return true;
    }
  } catch (err) {
    // Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
    // 直接访问 arguments.callee 会报如上错误
    if (err.message.indexOf('callee') !== -1) {
      return true;
    }
  }
  return false;
}

/**
 * If you need to support Safari 5-7 (8-10 yr-old browser),
 * take a look at https://github.com/feross/is-buffer
 */

function isBuffer(val) {
  // 检测 Buffer 实例的构造器，构造器上的 isBuffer 方法
  if (val.constructor && typeof val.constructor.isBuffer === 'function') {
    return val.constructor.isBuffer(val);
  }
  return false;
}
```