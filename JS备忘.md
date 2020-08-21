### JS备忘

##### 函数

- ```javascript
  var abs = function(x){
  	if(x >= 0){
  		return x;
  	} else {
  		return -x;
  	}
  };		//匿名函数赋值给变量abs，按照完整语法，需要在函数体末尾加一个分号 ；，表示赋值语句结束 
  ```

  #### 路径：
  
  ./ 同一级的文件
  ../上一级的文件
  
  #### rem匹配函数（凯源）
  ```javascript
  function setFontSize() {
    let docEl = document.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            let clientWidth = docEl.clientWidth;
            if (clientWidth >= 640) {
                clientWidth = 640
            }
            if (clientWidth === undefined) return;
            docEl.style.fontSize = 100 * (clientWidth / 640) + 'px';
        };
    if (document.addEventListener === undefined) return;
    window.addEventListener(resizeEvt, recalc, false);
    document.addEventListener('DOMContentLoaded', recalc, false);
    recalc();
}
  ```





let a=b=1; 相当于b=0; let a=b; 此时b=0的前面是没有let的，相当于window.b=0，意味着b始终是全局变量

let a =0,b=0; 相当于let a=0; let b=0;

减小 length 属性的值有删除当前数组元素（索引值在新旧长度值之间）的副作用

```javascript
const length = 4;
const numbers = [];
for (var i = 0; i < length; i++);{
  numbers.push(i + 1);
}

numbers; // => ???
```

null 语句是不执行任何操作的空语句。

前面的代码等效于以下代码：

```javascript
 const length = 4; 
const numbers = []; 
var i; 4for (i = 0; i < length; i++) { 
    // does nothing 
} 
{  
    // a simple block 
    numbers.push(i + 1);
}

numbers; // => [5]
```



```javascript
function arrayFromValue(item) {
  return
   [items];
}

arrayFromValue(10); // => ???
```

换行符会使 JavaScript 自动在 `return` 和 `[items]` 表达式之间插入分号。

这是等效代码，在 `return` 之后插入了分号：

```javascript
function arrayFromValue(item) {
    return;
    [items];
}

arrayFromValue(10); // => undefined
```

`0.1` 和 `0.2` 的和**并非等于** `0.3`，而是略大于 `0.3`。

双向绑定 = 单向绑定 + UI事件监听

```vue
<input v-model="xxx">
等价于
<input :value="xxx" @input="xxx=$event.target.value">
```

从使用时机来说，当条件值大于两个的时候，使用 switch 更好。

#### 调用构造函数

```javascript
new foo(); 
```

如果函数倾向于和 `new` 关键词一块使用，则我们称这个函数是 [构造函数](http://bonsaiden.github.io/JavaScript-Garden/zh/#function.constructors)。 在函数内部，`this` 指向*新创建*的对象。

#### 显式的设置 `this`

```
function foo(a, b, c) {}

var bar = {};
foo.apply(bar, [1, 2, 3]); // 数组将会被扩展，如下所示
foo.call(bar, 1, 2, 3); // 传递到foo的参数是：a = 1, b = 2, c = 3
```

当使用 `Function.prototype` 上的 `call` 或者 `apply` 方法时，函数内的 `this` 将会被 **显式设置**为函数调用的第一个参数。

因此*函数调用*的规则在上例中已经不适用了，在`foo` 函数内 `this` 被设置成了 `bar`。

另外，为增加可读性，如果包含 content 属性，应放在属性的最前面。

push( ) 在数组的最后面追加

pop( )  删除数组中的最后一个元素

shift( )  删除数组中的第一个元素

unshift( )  在数组最前面添加元素

slice( )

splice( )  删除元素、插入元素、替换元素

删除元素：splice(开始删除的位置，删除几个元素（第二个不传，则删除后面的全部元素）)

插入元素：splice(开始插入的位置，0，插入的元素)

替换元素：splice(开始替换的位置，替换（删除）几个元素，替换的元素)

split()：把一个字符串切割成字符串数组，可按某个字符进行切割

sort( ) ：排序

reverse( ) ：反转

trim()：删除字符串的头尾空格

toFixed：四舍五入为指定的小数位数的数字



string强转化为number : "string"+0

number强转换为string : number+" "



#### 高阶函数 reduce

```javascript
const arr =[1,2,3];
function getTotal(){
    let total=arr.reduce((prevalue,n)=>{
    return prevalue+n //第一次 prevalue：0 第二次 n：1
},0)				  //第二次 prevalue：1 第二次 n：2
    return total;	  //第三次 prevalue：3 第二次 n：3
}
getTotal(); //6


```

js把对象分为实例对象、函数对象和原型对象三大类

```javascript
实例对象：通过构造函数所创建的对象都是实例对象，（进行new操作的函数就是构造函数）
var people =new Student();
people是一个实例对象，student是一个构造函数

函数对象：即函数。构造函数和普通函数体现在代码中的区别就是new不new的区别

原型对象：是实例对象和函数对象的父对象。实例对象可以通过constructor属性来访问到构造函数，反过来不可以。
Student.constructor==people;//true 自己的理解是new了一个构造函数student后赋值给people，则people的原型是student
```

如果直接使用函数，this的指向是浏览器全局对象window；new了一个构造函数后，this的指向变成新的实例对象，并且还会将这个新的实例对象返回回来

```javascript
function people(name){
     this.name=name;
     this.say=function(){
             console.log(this.name);
     }
}

var Student=new people("咸鱼");
var man=new people("张三")
Student.say();　　//咸鱼
man.say();    //张三
console.log(Student.say === man.say);  //false
不相等说明两个函数对象不是同一个，因为new的时候会新建一个空白的函数对象。
如果大量重复new相同的函数，会给内存带来极大的浪费，所以可以把这个方法放到原型对象上，需要的时候去调用。
```

```javascript
js中每一个构造函数都有一个prototype属性，这个属性指向原型对象，原型对象里面也有一个constructor属性，这个属性指向构造函数。
js中每一个对象都有一个_ _proto_ _ 属性，实例对象可以通过_ _proto_ _访问到原型对象，构造函数可以通过prototype访问到原型对象，于是
实例对象._ _proto_ _===构造函数.prototype
function people(name){
     this.name=name;
     people.prototype.say=function(){
             console.log(this.name);
     }
}

var Student=new people("咸鱼");
var man=new people("张三")
Student.say();　　//咸鱼
man.say();    //张三
console.log(Student.say === man.say);  //true
```

```javascript
创造一个固定长度的数组：e.g. : Array(5)
创造一个固定长度且相同默认值的数组：Array(5).fill(7) //[7,7,7,7,7]
```



#### 函数

- ```javascript
  var abs = function(x){
  	if(x >= 0){
  		return x;
  	} else {
  		return -x;
  	}
  };		//匿名函数赋值给变量abs，按照完整语法，需要在函数体末尾加一个分号 ；，表示赋值语句结束 
  ```

   **当我们使用箭头函数的时候,函数体内的this指向就是定义时所在的对象,而不是使用时候所在的对象,并不是因为箭头函数内部有绑定this的机制,而是箭头函数没有自己的this,他的this是继承外面的,因此他的内部this就是外部代码层的this** 

  #### 获取昨天日期

```javascript
var time=(new Date).getTime()-24*60*60*1000; //当前距离1970年的毫秒数-一天的毫秒数=昨天距离1970年的毫秒数
var yesterday =new Date(time); //将昨天的毫秒数转换为Mon Aug 17 2020 08:58:36 GMT+0800 (中国标准时间)
var month=yesterday.getMonth(); //获得昨天的月份
var day = yesterday.getDate(); //获得昨天的日期
yesterday= yesterday.getFullYear()+"-"+(yesterday.getMonth())>9?(yesterday.getMonth()+1):"0"+(yesterday.getMonth()+1);//2020-08-17
```

#### 原生时间函数

```javascript
1.获取当前时间
var mydate=new Date();
result：Tue Oct 23 2018 10:02:30 GMT+0800 (中国标准时间)

2.获取时间常用方法
mydate.getFullYear(); //获取完整的年份(4位,1970-???) result：2018
mydate.getMonth(); //获取当前月份(0-11,0代表1月) result：9
mydate.getDate(); //获取当前日(1-31) result：23
mydate.getDay(); //获取当前星期X(0-6,0代表星期天) result：2
mydate.getHours(); //获取当前小时数(0-23) result：10
mydate.getMinutes(); //获取当前分钟数(0-59) result：18
mydate.getSeconds(); //获取当前秒数(0-59) result：45
mydate.getTime(); //获取当前时间(从1970.1.1开始的毫秒数) result：1540261028601
mydate.toLocaleDateString(); //获取当前日期 result：2018/10/23
mydate.toLocaleTimeString(); //获取当前时间 result：上午10:13:26
mydate.toLocaleString( ); //获取日期与时间 result：2018/10/23 上午10:14:21

// 重写方法，自定义格式化日期
Date.prototype.toLocaleString = function() {
    // 补0   例如 2018/7/10 14:7:2  补完后为 2018/07/10 14:07:02
    function addZero(num) {
        if(num<10)
            return "0" + num;
        return num;
    }
    // 按自定义拼接格式返回
    return this.getFullYear() + "/" + addZero(this.getMonth() + 1) + "/" + addZero(this.getDate()) + " " +
        addZero(this.getHours()) + ":" + addZero(this.getMinutes()) + ":" + addZero(this.getSeconds());
};
// 根据毫秒数构建 Date 对象
var date = new Date(1499996760000);
// 按重写的自定义格式，格式化日期
dateTime = date.toLocaleString();
```

