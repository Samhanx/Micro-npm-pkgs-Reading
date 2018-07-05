## unordered-array-remove

乱序删除数组指定元素，看 usage，传入数组和需要删除元素的 index

```js
var remove = require('unordered-array-remove')
var list = ['a', 'b', 'c', 'd', 'e']
remove(list, 2) // remove 'c'
console.log(list) // returns ['a', 'b', 'e', 'd'] (no 'c')
```

这个方法有点奇怪，删除数组一个非末尾元素，然后将末尾元素放在删除的位置，大概就是 unordered 的意思

```js
module.exports = remove

function remove (arr, i) {
  // index 范围检查
  if (i >= arr.length || i < 0) return
  // 删除并取最后一个元素，注意 pop 之后，数组长度已经变成了 arr.length - 1
  var last = arr.pop()
  // 如果刚好 i 是最后一个就直接返回，否则就把 i 和最后一个元素的值替换一下
  // 看 pr 有个地方可以完善，多一个检查 arr[i] !== last
  // 另一个 pr 提出改为 i !== arr.length，由于已经做了范围检测，所以没有必要
  if (i < arr.length) {
    var tmp = arr[i]
    arr[i] = last
    return tmp
  }
  return last
}
```