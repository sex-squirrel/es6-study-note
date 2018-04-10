## 二进制和八进制表示法
> 新写法

```js
//ES6规定了新的表示法Ob (或 OB) (二进制) 和 Oo (或 OO)（八进制）
OblllllOlll === 503 //true 
Oo767 === 503 // true
```

## Number.isFinite和Number.isNaN

> Number.isFinite

```js
//该方法用来检查数值是否有限
Number.isFinite(1) // true
Number.isFinite(0.8) // true
Number.isFinite(NaN) // false
Number.isFinite(Infinity) // false
Number.isFinite(-Infinity) // false
// 对于非数值类型该方法直接返回false，不会强制转换
```

> Number.isNaN ()

```js
//该方法用来检查是否是一个NaN类型
Number.isNaN(NaN) //true
Number.isNaN(1) // false
Number.isNaN(1/0) // false
Number.isNaN(Infinity) //false
Number.isNaN(-Infinity) //false
Number.isNaN('o') // false
Number.isNaN('1') // false
```

## Number.parseInt、 Number.parseFloat

> Number.parseInt

```js
//ES6将该方法由全局模块一直到了Number模块上面，用法与ES5一致
Number.parseInt === parseInt // true
```

> Number.parseFloat

```js
//ES6将该方法由全局模块一直到了Number模块上面，用法与ES5一致
Number.parseFloat === parseFloat // true
```

## Number.isInteger

```js
//判断一个数值是否为整数，在js内部整数和小数部分为0的浮点型数值任然当做同一个值处理
3 === 3.0 //true
Number.isInteger(25) // true
Number.isInteger(25.0) // true
Number.isInteger(25.1) // false
Number.isInteger ('15' ) // false
Number.isInteger(true) // false
```

## 安全整数 和 Number.isSafeInteger

> Number.MAX_SAFE_INTEGER 和 Number.MIN_SAFE_INTEGER

```js
//js 能够准确表示的整数范围在-2^53到 2^53之间(不含两个端点)，超过这个范围就无法精确表示。
Math.pow(2, 53) === Math.pow(2, 53) + 1 // true
//Number.MAX_SAFE_INTEGER = 9007199254740991； Number.MIN_SAFE_INTEGER = -9007199254740991

//注意: 这并代表不在安全值范围的数值就不能正确显示了，只是可能会出现结果错误的现象比如：Math.pow(2, 53) === Math.pow(2, 53) + 1 // true
Math.pow(2, 54) //18014398509481984
Math.pow(2, 54) + 10 //18014398509481990 正确结果应该是：18014398509481994
//对于超大数值的操作要留心，可能结果并不是你想要的
```

> Number.isSafeInteger

```js
//该方法是用来判断操作数值是否在安全范围内
Number.isSafeInteger ('a') //false
Number.isSafeInteger(null) // false
Number.isSafeInteger(NaN) // false
Number.isSafeInteger(Infinity) //false
Number.isSafeInteger(-Infinity) //false
Number.isSafeInteger(3) // true
Number.isSafeInteger(1.2) //true
Number.isSafeInteger(9007199254740990) //true
Number.isSafeInteger(9007199254740992) // false
Number.isSafeInteger(Number.MIN_SAFE_INTEGER - 1) // false
Number.isSafeInteger(Number.MIN_SAFE_INTEGER) // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER) // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1) // false
```

## Math对象的扩展

> Math.trunc

```js
//该方法去除数值的小数部分，返回整数部分
Math.trunc(5.5); // 5
Math.trunc(-5.5); // -5
Math.trunc(0.5); // 0
Math.trunc(-0.5); // -0
// 对于非数值类型会强转为数值,失败返回NaN
Math.trunc('3.8'); // 3
Math.trunc('-3.8'); // -3
Math.trunc('-3y'); // NaN
Math.trunc(true); //1
Math.trunc(false); // 0
```
> Math.sign

```js
//该方法用来判断-个数到底是正数、负数，还是零 。 对于非数值，会先将其转 换为数值 。
//参数为正数或者true，返回+ 1;
Math.sign(3) // 1
//参数为负数，返回- 1;
Math.sign(-3) //-1
//参数为0，返回0;
Math.sign(0) //0
//参数为-0，返回-0;
Math.sign(-0) //-0
//其他值，能转化则转化为对应数值，不能转化，则返回 NaN
Math.sign(true); //1
Math.sign(false); //0
Math.sign('4'); //1
Math.sign('-4'); //-1
Math.sign('true'); //NaN
```
> Math.cbrt

```js
//该方法用于计算立方根,对于非数值会进行强转,失败返回NaN
Math.cbrt(8); //2
Math.cbrt('64'); //4
Math.cbrt('64y'); //NaN
Math.cbrt(true); //1
Math.cbrt(false); //0
```
## 指数运算符

```js
//ES2016新增了指数运算符(**)
2 ** 2; // 4
let a = 5;
a **= 3; // 125 相当于 a = a * a * a;
//在 V8 引擎中，指数运算符与 Math .pow 的实现不相同，对于特别大的运算结果，两者会有细微的差异 。
```









