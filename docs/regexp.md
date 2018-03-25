## 构造函数

```js
// 在ES5的正则表达式构造函数中, 第一个参数如果是一个正则表达式,则不允许使用第二个参数(修饰符),ES6改变了这种行为
var reg = new RegExp(/abc/, 'i');
//Uncaught TypeError: Cannot supply flags when constructing one RegExp from another

//在ES6中,构造函数会忽略原表达式中的修饰符,会使用指定的修饰符
var reg = new RegExp(/abc/g, 'i').flag;
// "i"
```

## u修饰符

```js
//u修饰符含义为"Unicode模式",可以正确处理4个字节的UTF-16编码
/^\uD83D/u.test('\uD83D\uDc2A'); //false
/^\uD83D/.test('\uD83D\uDc2A'); //true

//u修饰符会改变下面表达式的行为

```

>点字符

```js
//点(.)字符的含义是除换行以外的任意单个字符,若点字符正确识别大于\uFFFF的字符,必须加上u修饰符
/^.$/.test('𠮷'); //false
/^.$/u.test('𠮷'); //true

```

>Unicode字符表示法

```js
//ES6新增了使用大括号表示Unicode字符的表示法,这种方法用在正则上面必须加上u修饰符才能正确被识别,否则会被当做量词
/\u{61}/.test('a'); //false
/\u{61}/u.test('a'); //true
/\u{61}{3}/.test('aaa'); //true

//大括号前不加\u的仍然是量词
```
>量词  预定义模式

```js
//使用u都会修饰后,所以量词和预定义模式都会正确识别码点大于0xFFFF的Unicode字符
/𠮷{2}/.test('𠮷'); //false
/𠮷{2}/u.test('𠮷𠮷'); //true
/^\S$/.test('𠮷'); //false
/^\S$/u.test('𠮷'); //true

//正确返回字符串长度的函数
function codePointLength(str) {
  var res = str.match(/[\s\S]/gu);
  return res ? res.length : 0;
}
var str = '𠮷𠮷';
str.length; // 4
codePointLength(str) // 2 
```

## y修饰符

```js
//y修饰符叫做"粘连"(sticky)修饰符与g的相同点都是全局匹配,不同的是y确保匹配从剩余的第一个位置开始
var s = 'aaa_aa_a';
var r1 = /a+/g;
var r2 = /a+/y;

r1.exec(s); // ["aaa"]
r2.exec(s); // ["aaa"]

r1.exec(s); // ["aa"]
r2.exec(s); // null
//r2会在第二次匹配的时候,从lastIndex = 3的位置也就是'_'的位置匹配,但是r2在此处没有匹配到'a',返回null
//lastIndex是每次搜索的开始位置,g修饰符从这个位置开始后搜索,知道发现匹配为止
//y修饰符每次也会从lastIndex开始,但是必须要求在lastIndex的位置上发现匹配
//y修饰符隐含了头部匹配的标志(^),y设计的本意是让头部匹配标志(^)在全局匹配中有效
```
## 后行断言

>先行断言 和 先行否定断言(ES5已经支持)

```js
//先行断言x只在y前面才匹配,必须写成/x(?=y)/的形式
/\d+(?=%)/.exec('90% of mine'); //["90"]
//先行否定断言x只有不在y前面才匹配,必须写成/x(?!y)/的形式
/\d+(?!%)/.exec('the % not 1'); //["1"]
```
>后行断言 和 后行否定断言(V8引擎V4.9+支持)

```js
//后行断言x只在y后面才匹配,必须写成/(?<=y)x/的形式
/(?<=\$)\d+/.exec('20 not $90'); //["90"]
//先行否定断言x只有不在y前面才匹配,必须写成/(?<!y)x/的形式
/(?<!\$)\d+/.exec('the 10 not $10'); //["10"]

//后行断言的匹配模式与普通不同,其实现需要先匹配/(?<=y)x/的x,然后再回到左边匹配y的部分,"先右后左"
/(?<=(\d+)(\d+))$/.exec('1053'); //["", "1", "053", index: 4, input: "1053", groups: undefined]
/^(\d+)(\d+)$/.exec('1053'); //["1053", "105", "3", index: 0, input: "1053", groups: undefined]
//没有后行断言时,第一个括号是贪婪模式,第二个括号只能捕获一个字符,所以结果是105和3
//有后行断言时,执行顺序是从右到左,第二个括号是贪婪模式,第一个括号只能捕获一个字符,所以结果是1和053

//若后行断言中有引用,则引用应该放到被引用的前面
/(?<=(o)d\1)r/.exec('hodor'); // null
/(?<=\1d(o))r/.exec('hodor'); // ["r", "o", index: 4, input: "hodor", groups: undefined]
```
## 具名组匹配

>简介

```js
// "具名组匹配"(Named Capture Groups)允许为每一个组匹配指定一个名字,既便于阅读,又便于引用
const RE_DATE = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
const matchObj = RE_DATE.exec('1993-05-27');
const year = matchObj.groups.year; //1993
const month = matchObj.groups.month; //05
const day = matchObj.groups.day; //27

// "具名组匹配"在圆括号内部,在模式前面添加"问好 + 尖括号"(?<具名名称>,例如?<year>),然后就可以使用exec方法返回结果的groups属性上引用该组名,同时数字序号依然有效
```
## 解构赋值和替换

>解构赋值

```js
// 有了具名组匹配以后,可以使用结构赋值直接匹配结果上为变量赋值
let {groups: {one, two}} = /^(?<one>.*):(?<two>.*)$/u.exec('foo:bar');

one; // foo
two; // bar
```

> 替换

```js
// 字符串替换时,使用 $<组名> 引用具名组
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
'2018-03-25'.replace(re, '$<day>/$<month>/$<year>');
// '25/03/2018'

//replace的第二个参数也可以是一个函数,可以传多个参数
'2018-03-25'.replace(re, (
  matched, //整个匹配结果2018-03-25
  capture1, //第一个组匹配 2018
  capture2, //第二个组匹配 03
  capture3, //第三个组匹配 25
  position, //匹配开始的位置 0
  S, //原字符串2018-03-25
  groups //具名组构成的一个对象{year, month, day}, 
) => {
  let {year, month, day} = args[args.length - 1]; // let {year, month, day} = groups;
  return `${day}/${month}/${year}`;
})
```
## 引用

```js
// 如果正则表达式内部引用某个"具名组匹配",可以使用 \k<组名>
const RE_TWICE = /^(?<word>[a-z]+)!\k<word>$/;
//const RE_TWICE = /^(?<word>[a-z]+)!\1$/; // 数字引用\1
RE_TWICE.test('abc!abc'); //true
RE_TWICE.test('abc!ab'); //false
```

