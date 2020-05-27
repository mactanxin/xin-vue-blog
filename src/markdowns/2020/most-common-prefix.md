# leetcode14：最长公共前缀

## 前情提要之: 最近一直在面试别人, 好久没有自己动手刷算法了

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

```
示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
```

```
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

1. 最笨办法:

```
var a = 'flow';
var b = 'fleet';

function getCommon(a, b) {
  let result = '';
  let counter = 0;
  for (let i = 0; i < a.length; i++) {
      if (a[i] === b[i]) {
          result += a[i];
      }
  }
  return result
}
```
