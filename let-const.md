
## let声明
>let所声明的变量只在let命令所在的代码块内有效  

```js
{
	let a = 10;
	var b = 5;		
}
a //undefined
b //5
```
>for循环中的let十分方便

```js
//在ES5中泄露
//1.可能会引起变量
for(var i = 0; i < 3; i++) {
	console.log(i)
}

0
1
2
i //3
//如果后续的逻辑代码中有i的话可能会引起其他混乱

//2.其他函数操作获取对应的i值得时候
var a = [];
for(var i = 0; i < 3; i++){
	a[i] = function() {
		console.log(i)
	}
}
a[0]() //3

//在ES6中,这些问题都可以用let很方便的解决
for(let i = 0; i < 3; i++) {
	console.log(i)
}

0
1
2
i // ReferenceError: i is not defined

var a = [];
for(let i = 0; i < 3; i++){
	a[i] = function() {
		console.log(i)
	}
}
a[0]() //0

```
>没有变量提升的问题  

```js
//使用var会出现大家熟知的"变量提升"的怪问题
console.log(a) //undefined
var a = 'foo';

//在let中不会有变量提升的问题,使得逻辑更加严谨
console.log(a) //ReferenceError: a is not defined
let a = 'foo';

```
>暂时性死区
 
```js
//在代码块内使用let声明变量之前,该变量都是不可用的,语法上成为"暂时性死区"(TDZ)
{
	//TDZ开始
	foo = 123; //ReferenceError: foo is not defined
	consol.log(foo);
	let foo; //TDZ结束
	console.log(foo);//undefined
}
//在TDZ内,即使使用typeof,依然会抛出错误,let作用域内,在变量声明之前,该变量不可获取
```
>同一作用域内不可重复声明同一变量

```js
{
	let a = 1;
	var a = 1; //Uncaught SyntaxError: Identifier 'a' has already been declared
}
```

## const声明
>const拥有let块级作用的所有特性

<!--2111-->

>const声明一个只读的常量,一旦声明,不允许改变,如果是对象,可以对对象进行属性扩展,是数组可以使用数组操作的方法 

```js
const PI = 3.1415926;
PI //3.1415926
PI = 1; //TypeError: Assignment to constant variable.
```