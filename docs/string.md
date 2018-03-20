
## 字符串遍历器接口
>for...of...

```js
//for..of...可以识别码点大于0xFFFF的字符
for (let codePoint of 'string' ) {
	console.log(codePoint)
}
//"s"
//"t"
//"r"
//"i"
//"n"
//"g"
```

## at()

```js
//ES5有对应的chartAt()方法,返回给定位置的字符串,但是该方法不能识别码点大于0xFFFF的字符
//at()可以正确返回码点大于0xFFFF的字符
'abc'.chartAt(0); //"a"
'𠮷'.chartAt(0); //"\uD842"

'abc'.at(0); //"a"
'𠮷'.at(0); //"𠮷"
```
## includes、satrtsWith、endsWith
>includes()

```js
//返回布尔值,表示是否找到了参数字符串
const str = 'Hello World!';
str.includes('H'); // true
```
>startsWith()

```js
//表示参数字符串是否在源字符串的头部
const prefix = 'tab-content';
prefix.startsWith('tab'); //true
```
>endsWith()

```js
//表示参数字符串是否在源字符串的尾部
const suffix = '123456abc.jpg';
suffix.endsWith('.jpg'); //true
```
## repeat
```js
//repeat()方法返回一个新的字符串,表示将原字符串重复n次
'a'.repeat(3); //"aaa"
'hello'.repeat(2); //"hellohello"
//参数是0<= n > -1的数字或者是NaN
'no'.repeat(0); //""
'no'.repeat(NaN); //""
'no'.repeat(-0.3); //""
// 参数是<=-1的负数或者是Infinity
'error'.repeat(-5); //RangeError: Invalid count value
'error'.repeat(Infinity); //RangeError: Invalid count value
//参数是字符串会先转为数字
'm'.repeat('4'); //"mmmm"
'n'.repeat('a'); //"" 字符串转换数字失败是NaN
```
## padStart、padEnd
>padStart()

```js
//ES2017引入了字符串补全长度的功能,如果长度不够,padStart()方法会在头部补全
//第一个参数指定新字符串的长度,第二个参数是用来补全的字符串
'm'.padStart(4, 'ac'); // "acam" length为4,在'm'前面自动补全'ac',按顺序重复
'm'.padStart(5, 'ac'); // "acacm"
//若原字符串的length大于等于指定字符串的length,则返回原字符串
'123456'.padStart(4,'pd'); //"123456"
//若参数为数组且只有一个元素为正数,等同于参数为数组的元素
'n'.padStart([5]) === 'n'.padStart(5) //true
//若没有第二个参数,则自动补全相应的空格
'n'.padStart(5); //'    n'
//若第一个参数为其他,则返回原字符串
'n'.padStart({}); // 'n'
```
>padEnd()

```js
//ES2017引入了字符串补全长度的功能,如果长度不够,padEnd()方法会在尾部补全
//第一个参数指定新字符串的长度,第二个参数是用来补全的字符串
'm'.padStart(4, 'ac'); // "maca" length为4,在'm'后面自动补全'ac',按顺序重复
'm'.padStart(5, 'ac'); // "macac"
//若原字符串的length大于等于指定字符串的length,则返回原字符串
'123456'.padStart(4,'pd'); //"123456"
//若参数为数组且只有一个元素为正数,等同于参数为数组的元素
'n'.padStart([5]) === 'n'.padStart(5) //true
//若没有第二个参数,则自动补全相应的空格
'n'.padStart(5); //'n    '
//若第一个参数为其他,则返回原字符串
'n'.padStart({}); // 'n'
```
>常见用途

```js
//为数字补全指定为数
'6'.padSatrt(2, '0'); // "06";
'5'.padSatrt(4, '0'); // "0005";

//提示字符串格式
'20'.padSatrt(10, 'YYYY-MM-DD'); // "YYYY-MM-20"
'03-20'.padSatrt(10, 'YYYY-MM-DD'); // "YYYY-03-20"
```
## 模板字符串

```js
//模板字符串是增强版的字符串,用反引号(`)标识,它可以当做普通字符串使用,也可以用来定义多行字符串,或者在字符串中嵌入变量或者表达式
//普通字符串
`adv'm'c`; //"adv'm'c"
`adv\nc`;
// "adv
// c"

//多行字符串
`template
 string
`
// "template
//  string
// "

//嵌入变量
const m = 'Hello'
`${m}, Jack`; //"Hello, Jack"

//若要使用反引号
`\`123/\`; //"`123`"

//模板字符串中说有的空格和换行都会被保留输出

//模板字符串允许嵌套写
`template
 string
${`aaaaa`}
`
// "template
//  string
// aaaaa
// "
// 注意: 嵌套必须写在${}内,其实也是js字符串变量
//${}内的变量未声明,将会报错,${}内也可以是js表达式,如果${}内的输出值不是一个字符串会转换成字符串
```