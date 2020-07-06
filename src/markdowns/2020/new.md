# 如何实现一个 new 关键字


```javascript
function _new(){
  const obj = {}
  const Constructor = Array.prototype.shift.call(arguments)

  obj.__proto__ = Constructor.prototype
  const result = Constructor.apply(obj, arguments)

  return typeof result === 'object' ? result : obj
}
```
