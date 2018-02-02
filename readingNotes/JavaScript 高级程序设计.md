# JavaScript 高级程序设计

## 变量、作用域和内存问题

在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数，或者用ECMAScript 的概念来说，就是arguments对象中的一个元素）。
在向参数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。

typeof 操作符是确定一个变量是字符串、数值、布尔值，还是undefined 的最佳工具。
如果变量的值是一个对象或null，typeof 返回都为"object"。
由typeof可知基本数据类型不是对象。

instanceof 判断是否为原型链上的类型，返回true或false。

变量对象(variable object)保存所有执行环境中所有的变量和函数。

每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。ECMAScript 程序中的执行流正是由这个方便的机制控制着。
当代码在一个环境中执行时，会创建变量对象的一个作用域链（scope chain）。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象（activation object）作为变量对象。活动对象在最开始时只包含一个变量，即arguments 对象（这个对象在全局环境中是不存在的）。作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。

函数内使用arguments对象遍历所有函数参数值。

下面两个语句可延长作用域链：
try-catch 语句的catch 块；
with 语句。
这两个语句都会在作用域链的前端添加一个变量对象。对with 语句来说，会将指定的对象添加到作用域链中。对catch 语句来说，会创建一个新的变量对象，其中包含的是被抛出的错误对象的声明。

变量提升
变量声明会被提升到函数的顶部。执行环境会初始化所有变量对象，只有执行流执行到相应的代码时变量才会被赋值

```
function fn () {
　　console.log(a); // undefined
　　var a = 'aaa';
　　console.log(a); // aaa
}
fn();

等价于
 
function fn () {
　　var a;        // 变量提升，函数作用域范围内
　　console.log(a);
　　a = 'aaa';
　　console.log(a);
}
fn();
```

如果局部环境中存在着同名变量标识符，就不会使用位于父环境中的变量标识符

```
var color = "blue";
function getColor(){
var color = "red";
return color;
}
alert(getColor()); //"red"
```

## 引用类型

### Object

在使用对象字面量语法时，属性名也可以使用字符串

```
var person = {
    "my name" : "Nicholas",
    "age" : 29,
    5 : true    //这里的数值属性名会自动转换为字符串
};
```

### Array

数组的length属性可读写，通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项
向数组中添加新项可使用Array.push()或Array[length] = XXX

```
var colors = ["red", "blue", "green"]; // 创建一个包含3 个字符串的数组
colors[colors.length] = "black";       //（在位置3）添加一种颜色
colors[colors.length] = "brown";       //（在位置4）再添加一种颜色
```

#### 检测数组

判断某个对象是不是数组的2种方法

```
if (value instanceof Array){
//对数组执行某些操作
}

if (Array.isArray(value)){
//对数组执行某些操作,推荐,但低版本浏览器不支持
}

```

#### 转换方法

数组的toLocaleString()、toString()和valueOf(), join(separator)用于按指定分隔符(separator)返回字符串
toLocaleString()、toString()返回逗号拼接的字符串，valueOf()返回数组本身

#### 栈方法

数组push()方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。
数组pop()方法从数组末尾移除最后一项，减少数组的length 值，然后返回移除的项。

```
var colors = new Array(); // 创建一个数组
var count = colors.push("red", "green"); // 推入两项
alert(count); //2
count = colors.push("black"); // 推入另一项
alert(count); //3
var item = colors.pop(); // 取得最后一项
alert(item); //"black"
alert(colors.length); //2
```

#### 队列方法

shift() 移除数组中的第一个项并返回该项，同时将数组长度减1。
结合使用shift()和push()方法，可以像使用队列一样使用数组。

unshift() 在数组前端添加任意个项并返回新数组的长度。
同时使用unshift()和pop()方法，可以从相反的方向来模拟队列，即在数组的前端添加项，从数组末端移除项。

#### 重排序方法

sort() 将数组每一项转换成字符串比较大小排序，哪怕都是数字也会转换为字符串
可以使用sort(compare)传入一个比较函数参数达到想要的排序目的并解决sort()排序机制带来的问题。

对于数值类型或者其valueOf()方法会返回数值类型的对象类型，可以使用一个更简单的比较函数。这个函数只要用第二个值减第一个值即可。

```
function compare(value1, value2){
    return value2 - value1;
}
```

#### 操作方法

concat() 连接数组并返回一个新的数组，原数组不改变
slice() 截取数组并返回一个新的数组，原数组不改变
splice()
删除：可以删除任意数量的项，只需指定2 个参数：要删除的第一项的位置和要删除的项数。
例如，splice(0,2)会删除数组中的前两项。
插入：可以向指定位置插入任意数量的项，只需提供3 个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。
例如，splice(2,0,"red","green")会从当前数组的位置2 开始插入字符串"red"和"green"。
替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。
例如，splice (2,1,"red","green")会删除当前数组位置2 的项，然后再从位置2 开始插入字符串"red"和"green"。

#### 归并方法

reduce()，reduceRight()这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。reduce()方法从数组的第一项开始，逐个遍历到最后。而reduceRight()则从数组的最后一项开始，向前遍历到第一项。

```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
});
alert(sum); //15
```

### Function

函数声明提升
JavaScript引擎在第一遍会声明函数并将它们放到源代码树的顶部

```
alert(sum(10,10));
function sum(num1, num2){
    return num1 + num2;
}
```

callee
该属性是一个指针，指向拥有这个arguments 对象的函数。

```
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * arguments.callee(num-1)
    }
}

等价于

function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * factorial(num-1)
    }
}
```

caller
这个属性中保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为null。

```
function outer(){
    inner();
}
function inner(){
    alert(arguments.callee.caller);
}
outer();

等价于

function outer(){
    inner();
}
function inner(){
    alert(inner.caller);
}
outer();
```

Function.call(thisArg, arg1, arg2, ...)、Function.apply(thisArg, [argsArray])在thisArg对象的作用域中调用Function函数

## 对象

### 理解对象

数据属性
[[Configurable]]：表示能否通过delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。默认值为true。
[[Enumerable]]：表示能否通过for-in 循环返回属性。默认值为true。
[[Writable]]：表示能否修改属性的值。默认值为true。
[[Value]]：包含这个属性的数据值。默认值为undefined。

访问器属性
[[Configurable]]
[[Enumerable]]
[[Get]]：在读取属性时调用的函数。默认值为undefined。
[[Set]]：在写入属性时调用的函数。默认值为undefined。
访问器属性不能直接定义，必须使用Object.defineProperty()来定义。

Object.getOwnPropertyDescriptor()方法，可以取得给定属性的描述符。这个方法接收两个参数：属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有configurable、enumerable、get 和set；如果是数据属性，这个对象的属性有configurable、enumerable、writable 和value。



















