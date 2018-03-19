
## 数组的解构赋值
### 基本用法
>模式匹配

```js
//一般解构
let [a, b, c] = [1, 2, 3];
a //1
b //2
c //3

//嵌套解构
let [m, [n, p]] = [2, [[6], 0]];
m //2
n //[6]
p //0

//...解构
let [head, ...tail] = [1, 2, 3, 4];
head //1
tail // [2, 3, 4]

let [x, y, ...z] = ['first'];
x //first
y //undefined (解构不成功为undefined)
z //[] (...解构不成功为[])
```
>指定默认值

```js
let [foo = true] = [];
foo //true

let [m, n='yes'] = ['hehe', undefined];
m //'hehe'
n //'yes'

//默认值可以引用结构值得其他变量,但被引用变量必须已经声明
let [x = 1, y = x] = [];
x //1
y //1

let [x = y, y = 1] = [];//ReferenceError: y is not defined
```
==**ES6使用严格相等运算符``===``判断一个位置是否有值,数组成员只有严格等于undefined的时候,默认是才会生效**==
## 对象解构赋值
>一般解构

```js
//与数组解构一个重要的不同就是变量要与被解构的对象对应属性同名才能获取正确的赋值
let { m, n, q } = { m: 1, n: 2 };
m //1
n //2
q // undefined

```
>对象属性模式解构
 
```js
//对象的属性可以作为解构赋值的模式
var obj = {
	p: ['hello', {y: 'world'}]
}
let {p: [x, {y}]} = obj;
x //hello
y //word
//其中p是模式,并不是变量

//下面是一个更加复杂的例子
let node = {
	loc: {
		start: {
			line: 1,
			column: 2
		}
	}
}
let {loc, loc:{start}, loc:{start:{line}}} = node;
loc //{start:{...}}
start //{line: 1, column: 2}
line //1

//嵌套赋值
let obj = {};
let arr = [];
({foo: obj.props, bar: arr[0]} = { foo: 123, bar: true });
obj //{props: 123}
arr //[true]
```

>默认值(同样可以使用默认值)

==**ES6使用严格相等运算符``===``判断一个位置是否有值,对象属性只有严格等于undefined的时候,默认是才会生效**==
## 字符串解构赋值
>普通解构  
  
```js
//字符串解构首先是先将字符串转换成一个类数组解构的的对象
let [a, b, c, d, e, f] = 'string';
//与下面例子类似
let [a, b, c, d, e, f] = ['s', 't', 'r', 'i', 'n', 'g'];

```

>字符串模式解构

```js
//类数组结构都有一个length属性
let {length: l} = 'string';
l //6
```
## 数值和布尔值的解构赋值
>如果等号右边是数字或者布尔值则先转化为对象

```js
let {toString: s} = 123;
s === Number.prototype.toString //true
let {toString: s} = true;
s === Boolean.prototype.toString //true
```
**由于undefined和null无法装换为Object,等号右边如果是``undefined``或者``null``会出现报错**
## 函数的参数解构
```js
function add([x, y]){
	return x + y;
}
add([1, 2]) // 2

[[1, 2], [3, 4]].map(([a, b]) => a + b);
//[3, 7]
```
>默认值

```js
function move({x = 0, y = 0} = {}) {
	return [x, y] 
}
move({x: 3, y: 2});//[3, 2]
move({x: 3});//[3, 0]
move({});//[0, 0]
move();//[0, 0]
```

>为参数指定默认值会得到其他结果

```js
function move({x, y} = {x: 0, y: 0}) {
	return [x, y] 
}
move({x: 3, y: 2});//[3, 2]
move({x: 3});//[3, undefined]
move({});//[undefined, undefined]
move();//[0, 0]
```
**`只要有可能,就不要在模式中放置圆括号`**