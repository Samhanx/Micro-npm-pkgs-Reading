## is sorted

检测一个数组是否排序完成，默认为是顺序排列，针对 Number 类型

```js
function defaultComparator (a, b) {
  return a - b
}

// 将 comparator 抽出来达到自定义扩展的目的，也就不用局限在数组元素的类型和排列规则上了
module.exports = function checksort (array, comparator) {
  if (!Array.isArray(array)) throw new TypeError('Expected Array, got ' + (typeof array))
  comparator = comparator || defaultComparator

  // length 赋值有点多此一举
  for (var i = 1, length = array.length; i < length; ++i) {
    if (comparator(array[i - 1], array[i]) > 0) return false
  }

  return true
}
```