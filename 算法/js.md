



# 原始值和引用值类型及区别

null,undefind,object,string,number，boolean,symbol

基本数据类型：number,string,boolean,null,undefined

- #### 1. 基本数据类型的值是不可变的

- #### 2. 基本数据类型不可以添加属性和方法

- #### 3. 基本数据类型的赋值是简单赋值

- #### 5. 基本数据类型是存放在栈区的

js引用类型： **Array、Function、Date、RegExp、Error、Arguments**

- #### 1. 引用类型的值是可以改变的

- #### 2. 引用类型可以添加属性和方法

- #### 3. 引用类型的赋值是对象引用

- #### 4. 引用类型的是存在堆中的



# 类型检测

```javascript
var und=undefined;
var nul=null;
var boo=true;
var num=1;
var str='xys'
var obj=new Object();
var arr=[1,2,3];
var fun=function(){}
var date=new Date();
var reg = /a/g;
var err=new Error()
var arg;
(function getArg(){
    arg=arguments;
})();

console.log(typeof und);  // undefined
console.log(typeof nul);  // object
console.log(typeof boo);  // boolean
console.log(typeof num);  // number
console.log(typeof str);  // string
console.log(typeof obj);  // object
console.log(typeof arr);  // object
console.log(typeof fun);  // function
console.log(typeof date);  // object
console.log(typeof reg);  // object
console.log(typeof err);  // object
console.log(typeof arg);  // object
typeof Symbol()    // 'symbol'

//通过 instanceof 操作符也可以对对象类型进行判定，其原理就是测试构造函数的prototype 是否出现在被检测对象的原型链上。
let arr = []
let obj = {}
arr instanceof Array    // true
arr instanceof Object   // true
obj instanceof Object   // true

//Object.prototype.toString()
先获取Object原型上的toString方法，让方法执行，并且改变方法中的this关键字的指向；

Object.prototype.toString 它的作用是返回当前方法的执行主体（方法中this）所属类的详细信息；

Object.prototype.toString.call({})              // '[object Object]'
Object.prototype.toString.call([])              // '[object Array]'
Object.prototype.toString.call(() => {})        // '[object Function]'
Object.prototype.toString.call('seymoe')        // '[object String]'
Object.prototype.toString.call(1)               // '[object Number]'
Object.prototype.toString.call(true)            // '[object Boolean]'
Object.prototype.toString.call(Symbol())        // '[object Symbol]'
Object.prototype.toString.call(null)            // '[object Null]'
Object.prototype.toString.call(undefined)       // '[object Undefined]'

Object.prototype.toString.call(new Date())      // '[object Date]'
Object.prototype.toString.call(Math)            // '[object Math]'
Object.prototype.toString.call(new Set())       // '[object Set]'
Object.prototype.toString.call(new WeakSet())   // '[object WeakSet]'
Object.prototype.toString.call(new Map())       // '[object Map]'
Object.prototype.toString.call(new WeakMap())   // '[object WeakMap]'

//constructor即构造函数，作用和instanceof非常的相似。
constructor的局限性：我们可以把类的原型进行重写，在重写的过程中，很有可能把之前的constructor给覆盖了，这样检测出来的结果就是不准确的。
function Fn() {
}
Fn.prototype = new Array;
var f = new Fn;
console.log(f.constructor); // Array
```

# `toString`和`String`的区别

`toString()`可以将数据都转为字符串，但是`null`和`undefined`不可以转换。

`toString()`括号中可以写数字，代表进制

`String()`可以将`null`和`undefined`转换为字符串，但是没法转进制字符串



# 类数组与数组的区别与转换

类数组

1. 拥有`length`属性，其它属性（索引）为非负整数（对象中的索引会被当做字符串来处理）;
2. 不具有数组所具有的方法；
3. 类数组是一个普通对象，而真实的数组是`Array`类型。

`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（`array-like object`）和可遍历（`iterable`）的对象。

类数组有: **函数的参数arugments（有迭代器接口）, DOM对象列表(比如通过 document.querySelectorAll 得到的列表),jQuery 对象 (比如 $("div")).**

#  bind、call、apply

fn.call(obj, arg1, arg2, ...)

call 接收多个参数，第一个为函数上下文也就是this，后边参数为函数本身的参数。

```javascript
Function.prototype.mycall = function (context, ...argus) {
   if (typeof this !== 'function' ) {
            new TypeError('type error')
        }
    let fn = this
    context = context || window
     //----------------------防止属性冲突
        let caller = Symbol('caller')
        context[caller] = fn
        context[caller](...rest)
        delete context[caller]
}
```



fn.apply(obj, [ arg1 ,  arg2 ] )

apply接收两个参数，第一个参数为函数上下文this，第二个参数为函数参数只不过是通过一个数组的形式传入的。

```javascript
Function.prototype.myApply = function (context, rest) {
       
        if (typeof this !== 'function' ) {
            new TypeError('type error')
        }
        context = context || window
        let fn = this
        //----------------------防止属性冲突
        let caller = Symbol('caller')
        context[caller] = fn
        context[caller](...rest)
        delete context[caller]
    }
```



bind 接收多个参数，第一个是bind返回值*返回值是一个函数*上下文的this，不会立即执行。返回一个新函数，第一个参数将作为它运行时的 this

```javascript
Function.prototype.mybind = function (context, ...argus) {
        if (typeof this !== 'function') {
            throw new TypeError('not funciton')
        }
        const fn = this
        const fBound = function (...argus2) {
            // const obj = new bindFoo  这个时候 this应该指向obj所以得处理一下
            return fn.apply(this instanceof fBound ? this : context, [...argus, ...argus2])
        }
        fBound.prototype = Object.create(this.prototype)
        return fBound
    }
```



# new的原理

除了箭头函数之外的任何函数，都可以使用 `new` 进行调用，

![](https://img-blog.csdnimg.cn/20190425004810731.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMxMDcxMQ==,size_16,color_FFFFFF,t_70)

```javascript
function myNew(func){
        let o  = Object.create(func.prototype)
        let res =  func.call(o)
        if(typeof res =='object'){
            return res
        }
        return o
    }
```



#  如何正确判断this？

1. 无论是否在严格模式下，在全局执行环境中（在任何函数体外部） **this** 都指向全局对象。
2. 简单函数调用， **this** 在一般模式下指向全局对象；严格模式下 **this** 默认为 **ndefined** 。
3. call , bind, apply在非箭头函数下修改 **this** 值；箭头函数下无法修改（由于 箭头函数没有自己的this指针，通过 call() 或 apply() 方法调用一个函数时，只能传递参数），不管call , bind, apply多少次，函数的 **this** 永远由第一次的决定。
4. 当函数作为对象里的方法被调用时，它们的 this 是调用该函数的对象。
5. 如果该方法存在于一个对象的原型链上，那么this指向的是调用这个方法的对象，就像该方法在对象上一样。
6. 当一个函数用作构造函数时（使用new关键字），它的this被绑定到正在构造的新对象。



# 作用域和作用域链、执行上下文



**作用域**

作用域是指在程序中定义变量的区域，该位置决定了变量的生命周期。通俗地理解，作用域就是变量与函数的可访问范围，即作用域控制着变量和函数的可见性和生命周期。

- **全局作用域**中的对象在代码中的任何地方都能访问，其生命周期伴随着页面的生命周期。

- **函数作用域**就是在函数内部定义的变量或者函数，并且定义的变量或者函数只能在函数内部被访问。函数执行结束之后，函数内部定义的变量会被销毁。

- **块级作用域**就是使用一对大括号包裹的一段代码，比如函数、判断语句、循环语句，甚至单独的一个{}都可以

  被看作是一个块级作用域

通过 `var` 创建的变量只有函数作用域，而通过 `let` 和 `const` 创建的变量既有函数作用域，也有块作用域。



**执行上下文**

1. 当 JavaScript 执行全局代码的时候，会编译全局代码并创建全局执行上下文，而且在整个页面的生存周期内，全局执行上下文只有一份。
2. 当调用一个函数的时候，函数体内的代码会被编译，并创建函数执行上下文，一般情况下，函数执行结束之后，创建的函数执行上下文会被销毁。
3. 当使用 eval 函数的时候，eval 的代码也会被编译，并创建执行上下文。

**执行上下文创建过程中，需要做以下几件事**:

1. 创建变量对象：首先初始化函数的参数arguments，提升函数声明和变量声明。
2. 创建作用域链（Scope Chain）：在执行期上下文的创建阶段，作用域链是在变量对象之后创建的。
3. 确定this的值，即 ResolveThisBinding



在执行上下文创建好后，JavaScript 引擎会将执行上下文压入栈中，通常把这种用来管理执行上下文的栈称为执行上下文栈，又称调用栈。



**作用域链**

其实在每个执行上下文的变量环境中，都包含了一个外部引用，用来指向外部的执行上下文，我们把这个外部引用称为 outer。

当一段代码使用了一个变量时，JavaScript 引擎首先会在“当前的执行上下文”中查找该变量，如果在当前的变量环境中没有查找到，那么 JavaScript 引擎会继续在 outer 所指向的执行上下文中查找。

我们把这个查找的链条就称为作用域链。



# 闭包及其作用

当通过调用一个外部函数返回一个内部函数后，即使该外部函数已经执行结束了，但是内部函数引用外部函数的变量依然保存在内存中，我们就把这些变量的集合称为闭包

1. 闭包的用途之一是实现对象的私有数据。
2. 在函数式编程中，闭包经常用于柯里化。
3. (阻止其被回收)



# 原型和原型链

![](https://img-blog.csdnimg.cn/20190425002456830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjMxMDcxMQ==,size_16,color_FFFFFF,t_70)

# 继承的实现方式及比较

**利用call ,apply(部分继承)**

```javascript
function Parent1 () {
           this.name = 'parent1'
       }
       function Child1 () {
           this.type = 'child'
           Parent1.call(this) //**改变this指向**
       }

```

这个方法没法继承 对象的原型（prototype）方法

**原型链继承****

```javascript
function Parent1 () {
           this.name = 'parent1'
           this.arr = [1,2,3,4]
       }
       function Child1 () {
           this.type = 'child'
           
       }
       Child1.prototype = new Parent1()
       var a = new Child1()
       var b = new Child1()
       a.arr.push('a') //** a,b的arr都会多个 'a'

```

该原型的引用类型属性会被所有的实例共享。



**组合方式**

```javascript
function Parent1 () {
           this.name = 'parent1'
           this.arr = [1,2,3,4]
       }
       function Child1 () {
           this.type = 'child'
           Parent1.call(this)//***********************多执行了一次父级 不推荐！！！！
       }
       Child1.prototype = new Parent1()
       var a = new Child1()
       var b = new Child1()
       a.arr.push('a') 
```

无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。

 

```javascript
function Parent1 () {
           this.name = 'parent1'
           this.arr = [1,2,3,4]
       }
       function Child1 () {
           this.type = 'child'
           Parent1.call(this)
       }
       Child1.prototype = Parent1.prototype
       var a = new Child1()  //**custructor指向父类（上面方法都有这个问题）
       var b = new Child1()
       a.arr.push('a')

```

**寄生组合式继承**

```javascript
function Parent1 () {
           this.name = 'parent1'
           this.arr = [1,2,3,4]
       }
       function Child1 () {
           this.type = 'child'
           Parent1.call(this)
       }
       Child1.prototype = Object.create(Parent1.prototype)
       //创建空对象  指定空对象{}的原型为Object.create()的参数。
       Child1.prototype.constructor = Child1

```

# 深拷贝与浅拷贝

**浅拷贝**

1. **Ob**ject.assign 

   语法：Object.assign(target, ...sources)

2. 扩展运算符  

   语法：let cloneObj = { ...obj };

3. Array.prototype.slice  

   语法: arr.slice(begin, end);

4. Array.prototype.concat 

 

**深拷贝**

将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且修改新对象不会影响原对象

JSON.stringify

JSON.stringify()是目前前端开发过程中最常用的深拷贝方式，原理是把一个对象序列化成为一个JSON字符串，将对象的内容转换成字符串的形式再保存在磁盘上，再用JSON.parse()反序列化将JSON字符串变成一个新的对象

1. 拷贝的对象的值中如果有函数,undefined,symbol则经过JSON.stringify()序列化后的JSON字符串中这个键值对会消失
2. 无法拷贝不可枚举的属性，无法拷贝对象的原型链
3. 拷贝Date引用类型会变成字符串
4. 拷贝RegExp引用类型会变成空对象
5. 对象中含有NaN、Infinity和-Infinity，则序列化的结果会变成null
6. 无法拷贝对象的循环应用(即obj[key] = obj)



自己实现深拷贝

```javascript
  function deepClone(origin, target) {
        target = target || {}
        let toStr = Object.prototype.toString
        let arr = '[object Array]'
        for (let prop in origin) {
            if (origin.hasOwnProperty(prop)) {
               if (typeof (origin[prop]) == 'object' && typeof(origin[prop] !=='null')) {
                    target[prop] = toStr.call(origin[prop]) == arr ? [] : {}
                    deepClone(origin[prop], target[prop])
                } else {
                    target[prop] = origin[prop]
                }
            }
        }
        return target
    }
```



# 防抖debounce，节流 throttle

**所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。**

```javascript
   function debounce(fn, time) {
        let timer = null
        return () => {
            if (timer) clearTimeout(timer)
            timer = setTimeout(() => {
                fn()
            }, time)
        }
    }
```



节流（throttle）

**所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。** 节流会稀释函数的执行频率。 

```javascript

    function throttle(fn, time) {
        let timer = null
        return () => {
            if (!timer) {
                timer = setTimeout(() => {
                    fn()
                    timer = null
                }, time)
            }
        }
    }
```

# DOM操作方式

### 创建节点

```
新的标签(元素节点) = document.createElement("标签名");
```



### 插入节点

```
 父节点.appendChild(新的子节点);
```

解释：父节点的最后插入一个新的子节点。

```
父节点.insertBefore(新的子节点,作为参考的子节点);
```

解释：

- 在参考节点前插入一个新的节点。
- 如果参考节点为null，那么他将在父节点最后插入一个子节点。

### 删除节点

```
  父节点.removeChild(子节点);
```

解释：**用父节点删除子节点**。必须要指定是删除哪个子节点。

如果我想删除自己这个节点，可以这么做：

```
node1.parentNode.removeChild(node1);
```

### 获取节点的属性值

方式1：

```
    元素节点.属性;
    元素节点[属性];
```

方式2：（推荐）

```
素节点.getAttribute("属性名称");
```

### 设置节点的属性值

方式1举例：（设置节点的属性值）

```
    myNode.src = "images/2.jpg"   //修改src的属性值
    myNode.className = "image2-box";  //修改class的name
```

方式2：（推荐）

```
 元素节点.setAttribute(属性名, 新的属性值);
```

### 删除节点的属性

格式：

```
    元素节点.removeAttribute(属性名);
```