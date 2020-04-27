



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

# 

执行类型转换的规则如下：

- 如果一个运算数是 Boolean 值，在检查相等性之前，把它转换成数字值。false 转换成 0，true 为 1。
- 如果一个运算数是字符串，另一个是数字，在检查相等性之前，要尝试把字符串转换成数字。
- 如果一个运算数是对象，另一个是字符串，在检查相等性之前，要尝试把对象转换成字符串。
- 如果一个运算数是对象，另一个是数字，在检查相等性之前，要尝试把对象转换成数字。

在比较时，该运算符还遵守下列规则：

- 值 null 和 undefined 相等。
- 在检查相等性时，不能把 null 和 undefined 转换成其他值。
- 如果某个运算数是 NaN，等号将返回 false，非等号将返回 true。
- 如果两个运算数都是对象，那么比较的是它们的引用值。如果两个运算数指向同一对象，那么等号返回 true，否则两个运算数不等。



# toString`和`String`的区别

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

**Object.create()**方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。

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

通俗地讲，当声明一个函数时，局部作用域一级一级向上包起来，就是作用域链。

作用域链是一条变量对象的链,它和执行上下文有关,用于在处理标识符的时候进行变量查询.
函数上下文的作用域链在[函数调用](https://www.baidu.com/s?wd=函数调用&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)的时候创建出

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

```html
<div id="content"
        style="height:150px;line-height:150px;text-align:center; color: #fff;background-color:#ccc;font-size:80px;">
</div>

let num = 1;
    let content = document.getElementById('content');

    function count() {
        content.innerHTML = num++;
    };
    content.onmousemove = throttle(count, 1000);
```

**所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间**。

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
 a.insertBefore(d, a.firstElementChild)
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





#  EventLoop事件循环

首先就是执行时会有一个栈 然后有一个事件队列

通常我们把消息队列中的任务称为**宏任务**，每个宏任务中都包含了一个**微任务队列**；在执行宏任务的过程中，如果 DOM 有变化，那么就会将该变化添加到微任务列表中，这样就不会影响到宏任务的继续执行，因此也就解决了执行效率的问题。



完毕后，会看看微任务空间中有没有微任务，有就把微任务空间中的微任务全部执行，然后去队列中取我们的事件执行，执行时若有微任务继续放到微任务队列，当此事件执行完毕，还会把微任务空间中的微任务全部执行完毕，然后再去取队列中的异步任务。。。。反复循环

# 宏任务与微任务

macrotask（又称之为宏任务），可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）

主代码块，**setTimeout，setInterval等**（可以看到，**事件队列中的每一个事件**都是一个macrotask）

- 每一个task会从头到尾将这个任务执行完毕，不会执行其它
- 浏览器为了能够使得JS内部task与DOM任务能够有序的执行，会在一个task执行结束后，在下一个 task 执行开始前，对页面进行重新渲染 （`task(当前的宏任务，微任务队列里面的所有微任务)->渲染->task->...`）

microtask（又称为微任务），可以理解是在当前 task 执行结束后立即执行的任务

**Promise，process.nextTick**等

- 也就是说，在当前task任务后，下一个task之前，在渲染之前
- 所以它的响应速度相比setTimeout（setTimeout是task）会更快，因为无需等渲染
- 也就是说，在某一个macrotask执行完后，就会将在它执行期间产生的所有microtask都执行完毕（在渲染前）



# js设计模式

## 单例模式

单例模式的定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点。实现的方法为先判断实例存在与否，如果存在则直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。

**缺点**：由于单例模式提供的是一种单点访问，所以它有可能导致模块间的强耦合 

**优点**：实例化一次。简化了代码的调试和维护

 适用场景：一个单一对象。比如：弹窗，无论点击多少次，弹窗只应该被创建一次。

```javascript
    class CreateUser {
        constructor(name) {
            this.name = name;
            this.getName();
        }
        getName() {
            return this.name;
        }
    }
    // 代理实现单例模式
    var ProxyMode = (function () {
        var instance = null;
        return function (name) {
            if (!instance) {
                instance = new CreateUser(name);
            }
            if (name) instance.name = name
            return instance
        }
    })();
    // 测试单体模式的实例
    var a = new ProxyMode("aaa");
    var b = new ProxyMode("bbb");
    // 因为单体模式是只实例化一次，所以下面的实例是相等的
    console.log(a === b); //true
```

## 代理模式

代理模式的定义：为一个对象提供一个代用品或占位符，以便控制对它的访问。

#### 优点

- 代理模式能将代理对象与被调用对象分离，降低了系统的耦合度。代理模式在客户端和目标对象之间起到一个中介作用，这样可以起到保护目标对象的作用
- 代理对象可以扩展目标对象的功能；通过修改代理对象就可以了，符合开闭原则；

#### 缺点

处理请求速度可能有差别，非直接访问存在开销

```html
<body>
    <ul id="ul">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
</body>
<script>
    let ul = document.querySelector('#ul');
    ul.addEventListener('click', event => {
        console.log(event.target);
    });
</script>
```

## 发布订阅者模式

定义了一种一对多的关系，让多个观察者对象同时监听某一个主题对象，这个主题对象的状态发生变化时就会通知所有的观察者对象，使它们能够自动更新自己

1.观察者模式维护的是一个单一事件对应多个依赖这个事件的对象之间关系 
观察者是: event->[obj1,obj2obj3,....]
2.订阅发布模式维护的是多个主题(事件) 以及依赖于各个主题(事件)的对象之间的关系
订阅发布是:{
event1->[obj1,obj2....],
event2->[obj1,obj2.....],....}

#### 优点

- 支持简单的广播通信，自动通知所有已经订阅过的对象
- 目标对象与观察者之间的抽象耦合关系能单独扩展以及重用
- 增加了灵活性
- 观察者模式所做的工作就是在解耦，让耦合的双方都依赖于抽象，而不是依赖于具体。从而使得各自的变化都不会影响到另一边的变化。

#### 缺点

过度使用会导致对象与对象之间的联系弱化，会导致程序难以跟踪维护和理解

```javascript
// 发布订阅模式
class EventEmitter {
  constructor() {
    // 事件对象，存放订阅的名字和事件  如:  { click: [ handle1, handle2 ]  }
    this.events = {}
  }
  // 订阅事件的方法
  on(eventName, callback) {
    if (!this.events[eventName]) {
      // 一个名字可以订阅多个事件函数
      this.events[eventName] = [callback]
    } else {
      // 存在则push到指定数组的尾部保存
      this.events[eventName].push(callback)
    }
  }
  // 触发事件的方法
  emit(eventName, ...rest) {
    // 遍历执行所有订阅的事件
    this.events[eventName] &&
      this.events[eventName].forEach(f => f.apply(this, rest))
  }
  // 移除订阅事件
  remove(eventName, callback) {
    if (this.events[eventName]) {
      this.events[eventName] = this.events[eventName].filter(f => f != callback)
    }
  }
  // 只执行一次订阅的事件，然后移除
  once(eventName, callback) {
    // 绑定的时fn, 执行的时候会触发fn函数
    const fn = () => {
      callback() // fn函数中调用原有的callback
      this.remove(eventName, fn) // 删除fn, 再次执行的时候之后执行一次
    }
    this.on(eventName, fn)
  }
}

```



# Location 对象



当然，当我们更改URL时，可能会出现这样一种情况：我们只更改了URL的片段标识符 (跟在＃符号后面的URL部分，包括＃符号)，这种情况下将触发 hashchange 事件，使用方法如下：

```
window.addEventListener("hashchange", funcRef, false);

// 或者

window.onhashchange = funcRef;
```

其中，提到的 hash 属性可以通过 `location.hash` 获得。从这里开始，我们引出 Location 对象。Location 对象包含有关当前 URL 的信息。针对一个示例 `http://b.a.com:88/index.php?name=kang&when=2011#first` 我们整理一下各个属性与其含义的分别所指内容是啥。

| 属性     | 描述                                          | 值                                                      |
| -------- | --------------------------------------------- | ------------------------------------------------------- |
| hash     | 设置或返回从井号 (#) 开始的 URL（锚）。       | "#first"                                                |
| host     | 设置或返回主机名和当前 URL 的端口号。         | "b.a.com:88"                                            |
| hostname | 设置或返回当前 URL 的主机名。                 | "b.a.com"                                               |
| href     | 设置或返回完整的 URL。                        | "http://b.a.com:88/index.php?name=kang&when=2011#first" |
| pathname | 设置或返回当前 URL 的路径部分。               | "/index.php"                                            |
| port     | 设置或返回当前 URL 的端口号。                 | 88                                                      |
| protocol | 设置或返回当前 URL 的协议。                   | "http:"                                                 |
| search   | 设置或返回从问号 (?) 开始的 URL（查询部分）。 | "?name=kang&when=2011"                                  |

window.location和document.location互相等价的，可以交换使用。且 location 的8个属性都是可读写的，但是只有href与hash的写才有意义。例如改变location.href会重新定位到一个URL，而修改location.hash会跳到当前页面中的anchor名字的标记(如果有)，而且页面不会被重新加载。

通过 location 我们可以作如下几种方法的操作：

- **assign()** 方法：加载新的文档；
- **reload()** 方法：重新加载当前文档；
- **replace()** 方法：用新的文档替换当前文档。

导航到一个新页面的方法：

```
window.location.assign("http://www.mozilla.org"); // or
window.location = "http://www.mozilla.org";
```

replace() 方法不会在 History 对象中生成一个新的记录。当使用该方法时，新的 URL 将覆盖 History 对象中的当前记录。用法为：

```
window.location.replace('http://example.com/'); 
```

通过 location 的属性与方法，我们可以做很多事情，详情可以查看 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/location)。

# Location 对象

## **Hash 路由**

url 上的 hash 以 # 开头，原本是为了作为锚点，方便用户在文章导航到相应的位置。因为 hash 值的改变不会引起页面的刷新，聪明的程序员就想到用 hash 值来做单页面应用的路由，并且当 url 的 hash 发生变化的时候，可以触发相应 hashchange 回调函数。

所以我们可以写一个 Router 对象，代码如下：

```javascript
class Router {
  constructor() {
    this.routes = {};
    this.currentUrl = '';
  }
  route(path, callback) {
    this.routes[path] = callback || function() {};
  }
  updateView() {
    this.currentUrl = location.hash.slice(1) || '/';
    this.routes[this.currentUrl] && this.routes[this.currentUrl]();
  }
  init() {
    window.addEventListener('load', this.updateView.bind(this), false);
    window.addEventListener('hashchange', this.updateView.bind(this), false);
  }
}
```

## **History 路由**

History 作为一个全局变量（即 window.history），不继承任何属性，在 HTML4 时代就已经存在，通过这个接口，我们可以操纵浏览器中曾经访问过的会话历史记录，但当时我们能使用的属性与方法只有这么几个：

- **History.length** 属性: 返回一个整数，该整数表示会话历史中元素的数目，包括当前加载的页。例如，在一个新的选项卡加载的一个页面中，这个属性返回1。
- **History.back()** 方法：前往上一页, 用户可点击浏览器左上角的返回按钮模拟此方法。等价于 history.go(-1).
- **History.forward()** 方法：在浏览器历史记录里前往下一页，用户可点击浏览器左上角的前进按钮模拟此方法。等价于 history.go(1).
- **History.go()** 方法：通过当前页面的相对位置从浏览器历史记录( 会话记录 )加载页面。

从 HTML5 开始，增加了两个新的方法：

- History.pushState(state, title [, url])** 方法：往历史堆栈的顶部添加一个状态。
- **History.replaceState(state, title [, url])**：更改当前页面的历史记录值。参数同上。这种更改并不会去访问该URL。
- **popstate**事件：当活动历史记录条目更改时，将触发popstate事件

# HTML attribute与DOM property之间的区别

**attribute** 是我们在 **html** 代码中经常看到的键值对, 例如:

```
<input id="the-input" type="text" value="Name:" />
```

上面代码中的 input 节点有三个 attribute:

- id : the-input
- type : text
- value : Name:

**property**属性属于DOM对象，DOM实质就是javascript中的对象。我们可以跟在js中操作普通对象一样获取、设置DOM对象的属性，并且property属性可以是任意类型。

```
HTMLInputElement.id === 'the-input'
HTMLInputElement.type === 'text'
```

- 非自定义的property（attribute）改变的时候，其对应的attribute（property）在多数情况下也会改变。（自定义的属性 property无法获取）
- 当对应的property改变的时候，attribute特性value的值一直未默认值，并不会随之改变。
- 在javascript中我们推荐使用**property属性**因为这个属性相对**attribute**更快，更简便。尤其是有些类型本该是布尔类型的attribute特性。比如："checked", "disabled", "selected"。浏览器会自动将这些值转变成布尔值传给property属性。



# addEventListener和onClick()的区别

### 1.DOM 0级事件

在了解DOM0级事件之前，我们有必要先了解下**HTML事件处理程序**，也是最早的这一种的事件处理方式，代码如下：

```html
<button type="button" onclick="fn()" id="btn">点我试试</button>

<script>
    function fn() {
        alert('Hello World');
    }
</script>
```

直接在HTML代码当中定义了一个onclick的属性触发fn方法，这样的事件处理程序最大的缺点就是**HTML与JS强耦合**，当我们一旦需要修改函数名就得修改两个地方。当然其优点就是**不需要操作DOM来完成事件的绑定**。

那我们如何实现**HTML与JS低耦合**？这样就有DOM0级处理事件的出现解决这个问题。

**DOM0级事件就是将一个函数赋值给一个事件处理属性**，比如：

```html
<button id="btn" type="button"></button>

<script>
    var btn = document.getElementById('btn');
    
    btn.onclick = function() {
        alert('Hello World');
    }
    
    // btn.onclick = null; 解绑事件 
</script>
复制代码
```

上面的代码我们给button定义了一个id，然后通过JS获取到了这个id的按钮，并将一个函数赋值给了一个事件处理属性onclick，这样的方法便是DOM0级处理事件的体现。我们可以通过给事件处理属性赋值null来**解绑事件**。DOM 0级的事件处理的步骤：**先找到DOM节点，然后把处理函数赋值给该节点对象的事件属性。**

DOM0级事件处理程序的缺点在于**一个处理程序「事件」无法同时绑定多个处理函数**

### 2.DOM2级事件

DOM2级事件在DOM0级事件的基础上弥补了一个处理程序无法同时绑定多个处理函数的缺点，允许给一个处理程序添加多个处理函数。也就是说，**使用DOM2事件可以随意添加多个处理函数**，移除DOM2事件要用removeEventListener。代码如下：

```html
<button type="button" id="btn">点我试试</button>

<script>
    var btn = document.getElementById('btn');

    function fn() {
        alert('Hello World');
    }
    btn.addEventListener('click', fn, false);
    // 解绑事件，代码如下
    // btn.removeEventListener('click', fn, false);  
</script>
```

DOM2级事件定义了addEventListener和removeEventListener两个方法，分别用来绑定和解绑事件

IE8级以下版本不支持addEventListener和removeEventListener，需要用attachEvent和detachEvent来实现


### 3.DOM3级事件

DOM3级事件在DOM2级事件的基础上添加了更多的事件类型，全部类型如下：

1. UI事件，当用户与页面上的元素交互时触发，如：load、scroll
2. 焦点事件，当元素获得或失去焦点时触发，如：blur、focus
3. 鼠标事件，当用户通过鼠标在页面执行操作时触发如：dbclick、mouseup
4. 滚轮事件，当使用鼠标滚轮或类似设备时触发，如：mousewheel
5. 文本事件，当在文档中输入文本时触发，如：textInput
6. 键盘事件，当用户通过键盘在页面上执行操作时触发，如：keydown、keypress
7. 合成事件，当为IME（输入法编辑器）输入字符时触发，如：compositionstart
8. 变动事件，当底层DOM结构发生变化时触发，如：DOMsubtreeModified

同时DOM3级事件也**允许使用者自定义一些事件**。

## 事件代理(事件委托)

由于事件会在冒泡阶段向上传播到父节点，因此可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件。这种方法叫做事件的代理（delegation）。

- 函数是对象，会占用内存，内存中的对象越多，浏览器性能越差
- 注册的事件一般都会指定DOM元素，事件越多，导致DOM元素访问次数越多，会延迟页面交互就绪时间。
- 删除子元素的时候不用考虑删除绑定事件

## Event对象常见的方法和属性

- `event.target`指向**引起触发事件的元素**，而`event.currentTarget`则是**事件绑定的元素**。
- **event.stopPropagation() 方法阻止事件冒泡到父元素，阻止任何父事件处理程序被执行**。
- **stopImmediatePropagation 既能阻止事件向父元素冒泡，也能阻止元素同事件类型的其它监听器被触发。而 stopPropagation 只能实现前者的效果**。
- event. preventDefault()**如果调用这个方法，默认事件行为将不再触发**。



# Promise

### promise 标准

- 三种状态：pending fulfilled reject
- 初始状态：pengding
- pending变为fulfilled 或者 变为 reject
- 状态变化不可逆

- promise必须实现then方法
- then()接受俩函数作为参数
- then()返回的是pormise实例

 

```javascript
实现一个简单的promise
//Promise 的三种状态  (满足要求 -> Promise的状态)
const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';

class AjPromise {
  constructor(fn) {
    //当前状态
    this.state = PENDING;
    //终值
    this.value = null;
    //拒因
    this.reason = null;
    //成功态回调队列
    this.onFulfilledCallbacks = [];
    //拒绝态回调队列
    this.onRejectedCallbacks = [];

    //成功态回调
    const resolve = value => {
      // 使用macro-task机制(setTimeout),确保onFulfilled异步执行,且在 then 方法被调用的那一轮事件循环之后的新执行栈中执行。
      setTimeout(() => {
        if (this.state === PENDING) {
          // pending(等待态)迁移至 fulfilled(执行态),保证调用次数不超过一次。
          this.state = FULFILLED;
          // 终值
          this.value = value;
          this.onFulfilledCallbacks.map(cb => {
            this.value = cb(this.value);
          });
        }
      });
    };
    //拒绝态回调
    const reject = reason => {
      // 使用macro-task机制(setTimeout),确保onRejected异步执行,且在 then 方法被调用的那一轮事件循环之后的新执行栈中执行。 (满足要求 -> 调用时机)
      setTimeout(() => {
        if (this.state === PENDING) {
          // pending(等待态)迁移至 fulfilled(拒绝态),保证调用次数不超过一次。
          this.state = REJECTED;
          //拒因
          this.reason = reason;
          this.onRejectedCallbacks.map(cb => {
            this.reason = cb(this.reason);
          });
        }
      });
    };
    try {
      //执行promise
      fn(resolve, reject);
    } catch (e) {
      reject(e);
    }
  }
  then(onFulfilled, onRejected) {
    typeof onFulfilled === 'function' && this.onFulfilledCallbacks.push(onFulfilled);
    typeof onRejected === 'function' && this.onRejectedCallbacks.push(onRejected);
    // 返回this支持then 方法可以被同一个 promise 调用多次
    return this;
  }
}

```

### promise.all

Promise.all([p1,p2,p3])中，all方法的参数条件

- 参数为一个[可迭代](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol)对象，如 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array) 或 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String)。
- 传入的值必须按顺序输出
- 一旦又一个reject则状态立马变为reject，并将错误原因抛出

**实现思路：**

1. 生成一个新的Promise。
2. 遍历传入的promise数组，依次调用每一个promise的then方法注册回调。
3. 在then 回调中把promise返回值push到一个结果数组中，检测结果数组长度与promise数组长度相等时表示所有promise都已经resolve了，再执行总的resolve。



```

```

### promise.race

1. 参数为一个[可迭代](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol)对象，如 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array) 或 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String)。
2. 一个**待定的** [`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 只要给定的迭代中的一个promise解决或拒绝，就采用第一个promise的值作为它的值，从而**异步**地解析或拒绝

```javascript
Promise.race = function (promises) {
    return new Promise((resolve, reject) => {
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(resolve, reject);
        }
    })
}

```

### catch方法

```javascript
//catch()其实就是简易版的then()方法，它没有成功的回调，只有失败的回调。
then(onFulfilled, onRejected){
    ...
}
catch(onRejected){ //在此处添加原型上的方法catch
    return this.then(null,onRejected);
}
```



# 文件下载

```javascript
 //  -----------------------------------------------------单例模式创造a标签
let download = (function Download() {
            let a = null
            return function (href) {
                if (!a) {
                    a = document.createElement('a')
                }
                if (href) {
                    a.href = href
                }
                return a
            }
        })()

        let myFile = document.getElementById('myFile')
        myFile.addEventListener('change', () => {

            let blob = new Blob([myFile.files[0]])
            console.log(blob)
            let url = window.URL.createObjectURL(blob)
            download(url)
        })
        let btn = document.getElementById('btn')
        btn.addEventListener('click', () => {
            let a = download()
            a.setAttribute('download', 'xxx.docx')
            a.click()
            window.URL.revokeObjectURL(a.href)
        })
```

# react,vue 中key的作用

key的作用就是更新组件时**判断两个节点是否相同**。相同就复用，不相同就删除旧的创建新的。主要是为了提升diff【同级比较】的效率

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将不是移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素

不带key时节点能够复用，省去了销毁/创建组件的开销，同时只需要修改DOM文本内容而不是移除/添加节点，这就是文档中所说的“刻意依赖默认行为以获取性能上的提升”。**这种模式只适用于渲染简单的无状态组件**

带上唯一key虽然会增加开销，但是对于用户来说基本感受不到差距，而且能保证组件状态正确

# 箭头函数

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。

2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

4、不可以使用 new 命令，因为：

- 没有自己的 this，无法调用 call，apply。
- 没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__

## 什么是 Immutable Data

Immutable Data 就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是 Persistent Data Structure（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 Structural Sharing（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。





# 任务队列：补充

1.调用栈中按顺序执行  

- ​    开始 -> 
- ​    取第一个task queue里的任务执行(可以认为同步任务队列是第一个task queue) -> 
- ​    取microtask全部任务依次执行 -> 
- ​    取下一个task queue里的任务执行 -> 
- ​    再次取出microtask全部任务执行 -> … 这样循环往复

2.执行到 async 标记过的函数（设为foo），当进入该函数的时候， JavaScript 引擎会保存当前的调用栈等信息，然后执行 foo 函数，执行到await的时候

**如果await后面就是promise对象则返回原Promise中的resolve**

**如果碰到await 1 这样的它就会默认创建一个 Promise 对象**

```javascript
let promise_ = new Promise((resolve,reject){
  resolve(1)
})
```

- 然后 JavaScript 引擎会暂停当前协程的执行，将主线程的控制权转交给父协程执行，同时会将 promise_ 对象返回给父协程。父协程讲这个promise放入微任务队列
- 随后父协程将执行结束，在结束之前，会进入微任务的检查点，然后执行微任务队列
- 微任务队列中有resolve(1)的任务等待执行，执行到这里的时候，会触发 promise_.then 中的回调函数
- **如果这个回调函数还有.then操作则将这个onResolve放入微任务中 然后将执行权返回父协程**

该回调函数被激活以后，会将主线程的控制权交给 foo 函数的协程

然后 foo 协程继续执行后续语句，执行完成之后，将控制权归还给父协程。



- **Symbol.for(string)，这种方式会把创建的symbol放入一个symbol注册表，如果已经存在了该symbol就会返回相同的值，这样可在不同的页面共享一个Symbol。**

# 垃圾回收机制 标记清除

**这是javascript中最常用的垃圾回收方式**。当变量进入执行环境是，就标记这个变量为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。当变量离开环境时，则将其标记为“离开环境”。

垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。然后，它会去掉环境中的变量以及被环境中的变量引用的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后。垃圾收集器完成内存清发除工作，销毁那些带标记的值，并回收他们所占用的内存空间。


