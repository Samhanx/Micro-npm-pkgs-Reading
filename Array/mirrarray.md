## mirrarray

```js
// 共享 Error.prototype
function MirrarrayError() {}
MirrarrayError.prototype = Object.create(Error.prototype);
// (new MirrarrayError()) instanceof Error
// 子类继承父类的话，构造函数里还需要对 parentFn.call(this)
// 要抛错直接用 new Error 就好了，完全用不着这样浪费空间，并且实测并不会显示错误信息

// 看到这个包的第一个想法就是 key 的类型转换
const isValidKey = element => {
  const isNull = element === null;
  // typeof null === 'object'
  return ['string', 'number', 'boolean', 'undefined'].includes(typeof element) || isNull;
}

const nonOverlappingKey = element => {
  /**
   * Ensure we don't have distinct elements that coerce to the same key, leading to unexpected results.
   * For example, input of [true, 'true'] would return a keymirror of {true: 'true'} despite containing two distinct elements
   * if we didn't make this check.
   */
  const isNull = element === null;
  const typeSeenBefore = keysSeen['' + element]; // 空字符串拼接转换类型，keysSeen 这个变量来得莫名巧妙，虽然实际调用的时候已经是一个空对象了
  const thisType = isNull ? 'null' : typeof element;

  // 这里对类型进行检测， '' + element 的值为 string number boolean undefined null 之一
  // typeSeenBefore 在对象里检测看是否有值
  if (typeSeenBefore) {
    // 有值且前后两次传入元素类型不同时返回 false，随后报错，即作者注释提到的 [true, 'true'] 情况
    return typeSeenBefore === thisType;
  } else {
    // 没有就添加上标记，然后返回 true
    keysSeen['' + element] = thisType;
    return true;
  }
  // 整个这一块逻辑其实有点绕且重复，可以修改成下面这样，'' + element 可以存成 key
  // !typeSeenBefore && (keysSeen['' + element] = thisType)
  // return keysSeen['' + element] === thisType
}

let keysSeen;

const arrayToKeyMirror = arr => {
  keysSeen = {};
  if (!Array.isArray(arr)) {
    throw new MirrarrayError('Input to mirrarray must be an array!');
  }
  // 用了 reduce 方法，传空对象，然后不断迭代写入
  return arr.reduce((acc, key) => {
    if (!isValidKey(key)) {
      throw new MirrarrayError('Invalid element contained within input array; each element must be either a string or number!');
    }
    if (!nonOverlappingKey(key)) {
      throw new MirrarrayError('Distinct elements in the input array are coercing to the same string!');
    }
    acc[key] = key;
    return acc;
  }, {});
};

export default arrayToKeyMirror;
```