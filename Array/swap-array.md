## swap-array

交换数组元素

```js
export default (Arr, Caller, Target) => {
  let Instance = Arr.constructor(); //调用数组构造器得到一个新的空数组
  let Stash = Arr; // 复制一份引用看上去没什么用

  let InstanceType = Array.isArray(Instance) ? 'array' : typeof Instance; // 兼容没有 isArray

  // Check types and throw err if no arr is passed
  if(InstanceType !== 'array') throw '[ERR] SwapArray expects a array as first param';

  // Copy the Arr-Content into new Instance - so we don't overwrite the passed array
  Stash.map((s, i) => Instance[i] = s); // 完全用不着用一个空数组来复制吧 map 本身就会返回一个复制后的数组
  // let instance = Stash.map(s => s) 就拿到了一份 stash 的浅拷贝

  // Update indexes
  Instance[Caller] = Instance.splice(Target, 1, Instance[Caller])[0];
  // var arr = ['a', 'b', 'c'];
  // splice 方法实现交换数组元素 arr[0] = arr.splice(2, 1, arr[0])[0]
  // 注意这样调用 splice 返回了一个只有一个元素的数组
  // 另一个方式是解构赋值 [arr[0], arr[2]] = [arr[2], arr[0]]

  return Instance;
}
```