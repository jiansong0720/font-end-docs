# JavaScript Tips



[TOC]



## 1. 类型强制转换

### 1.1  string强制转换为数字

- 可以用`*1`来转化为数字(实际上是调用`.valueOf`方法) 然后使用`Number.isNaN`来判断是否为`NaN`，或者使用 `a !== a` 来判断是否为`NaN`，因为 `NaN !== NaN`

```
'32' * 1            // 32
'ds' * 1            // NaN
null * 1            // 0
undefined * 1    // NaN
1  * { valueOf: ()=>'3' }        // 3
```
- 也可以使用`+`来转化字符串为数字

```
+ '123'            // 123
+ 'ds'               // NaN
+ ''                    // 0
+ null              // 0
+ undefined    // NaN
+ { valueOf: ()=>'3' }    // 3
```

### 1.2 object强制转化为string



### 1.3 使用Boolean过滤数组中的所有假值

```
const compact = arr => arr.filter(Boolean)
compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34])             // [ 1, 2, 3, 'a', 's', 34 ]
```

### 1.4 双位运算符 ~~

```
~~4.5            // 4
Math.floor(4.5)        // 4
~~-4.5        // -4
Math.floor(-4.5)        // -5
```

### 1.5 短路运算符做简单判断

```
let variable = param && param.prop
如果param如果为真值则返回param.prop属性，否则返回param这个假值，这样在某些地方防止param为undefined的时候还取其属性造成报错。
```

### 1.6 取整 | 0

"|”就是转换为2进制之后相加得到的结果

```
1.3 | 0         // 1
-1.9 | 0        // -1
console.log(0.6|0)//0
console.log(1.1|0)//1
console.log(3.65555|0)//3
console.log(5.99999|0)//5
console.log(-7.777|0)//-7
```

### 1.7 判断奇偶数 `& 1`

```
const num=3;
!!(num & 1)					// true
!!(num % 2)					// true
```

### 1.8 ++a 和 a++

```
a++   
++a 
b=a++ ;    相当于 b=a ; 然后 a 自己加1
b=++a ;    相当于 a 自己加 1, 然后 b=a; (此时 a 已经先加1了)

let a = 1
console.log(a++) // 1
console.log(++a) // 2
```



## 2. 函数

### 2.1 函数默认值

```
func = (l, m = 3, n = 4 ) => (l * m * n);
func(2)             //output: 24
传入参数为undefined或者不传入的时候会使用默认参数，但是传入null还是会覆盖默认参数。
```

### 2.2 强制参数

```
默认情况下，如果不向函数参数传值，那么JS 会将函数参数设置为undefined。其它一些语言则会发出警告或错误。要执行参数分配，可以使用if语句抛出未定义的错误，或者可以利用强制参数。\

mandatory = ( ) => {
  throw new Error('Missing parameter!');
}
foo = (bar = mandatory( )) => {            // 这里如果不传入参数，就会执行manadatory函数报出错误
  return bar;
}
```

### 2.3 隐式返回值

```
返回值是我们通常用来返回函数最终结果的关键字。只有一个语句的箭头函数，可以隐式返回结果（函数必须省略大括号{ }，以便省略返回关键字）。

要返回多行语句（例如对象文本），需要使用( )而不是{ }来包裹函数体。这样可以确保代码以单个语句的形式进行求值。

function calcCircumference(diameter) {
  return Math.PI * diameter
}
// 简写为：
calcCircumference = diameter => (
  Math.PI * diameter;
)
```

### 2.4 惰性载入函数

```
function foo(){
    if(a !== b){
        console.log('aaa')
    }else{
        console.log('bbb')
    }
}

// 优化后
function foo(){
    if(a != b){
        foo = function(){
            console.log('aaa')
        }
    }else{
        foo = function(){
            console.log('bbb')
        }
    }
    return foo();
}
```

### 2.5 一次性函数

```
var sca = function() {
    console.log('msg')
    sca = function() {
        console.log('foo')
    }
}
sca()        // msg
sca()        // foo
sca()        // foo
```

## 3. 代码复用

### 3.1 Object [key]

```
// object validation rules
const schema = {
  first: {
    required:true
  },
  last: {
    required:true
  }
}

// universal validation function
const validate = (schema, values) => {
  for(field in schema) {
    if(schema[field].required) {
      if(!values[field]) {
        return false;
      }
    }
  }
  return true;
}
console.log(validate(schema, {first:'Bruce'})); // false
console.log(validate(schema, {first:'Bruce',last:'Wayne'})); // true
```

### 4.2 精确到指定位数的小数

```
const round = (n, decimals = 0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`)
round(1.345, 2) 				// 1.35
round(1.345, 1) 				// 1.3
```

## 5. 数组

### 5.1 reduce方法同时实现map和filter

```
// 将数组全部*2后找出大于50的数字组成数组
const numbers = [10, 20, 30, 40];
const doubledOver50 = numbers.reduce((finalList, num) => {
  num = num * 2;
  if (num > 50) {
    finalList.push(num);
  }
  return finalList;
}, []);
doubledOver50;            // [60, 80]
```

### 5.2 统计数组中相同项的个数

```
var cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
var carsObj = cars.reduce(function (obj, name) {
  obj[name] = obj[name] ? ++obj[name] : 1;
  return obj;
}, {});
carsObj; // => { BMW: 2, Benz: 2, Tesla: 1, Toyota: 1 }
```

### 5.3 使用解构来交换参数数值

```
let param1 = 1;
let param2 = 2;
[param1, param2] = [param2, param1];
console.log(param1) // 2
console.log(param2) // 1
```

### 5.4 接收函数返回的多个结果

```
async function getFullPost(){
  return await Promise.all([
     fetch('/post'),
     fetch('/comments')
  ]);
}
const [post, comments] = getFullPost();
```

### 5.5 将数组平铺到指定深度

```
const flatten = (arr, depth = 1) =>
  depth != 1
    ? arr.reduce((a, v) => a.concat(Array.isArray(v) ? flatten(v, depth - 1) : v), [])
    : arr.reduce((a, v) => a.concat(v), []);
flatten([1, [2], 3, 4]);                    		 // [1, 2, 3, 4]
flatten([1, [2, [3, [4, 5], 6], 7], 8], 2);           // [1, 2, 3, [4, 5], 6, 7, 8]
```

