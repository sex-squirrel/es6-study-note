## 函数参数的默认值
> 基本用法

```js
//ES6允许函数参数设置默认值
function log (x, y='world') {
  console.log(x + ',' + y)
}
log('Hello'); // 'Hello,world'
log('Hello', 'China'); // 'Hello,China'
log('Hello', ''); // 'Hello,'
```