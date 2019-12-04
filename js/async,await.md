 ES7 引入了 async/await，这是 JavaScript 异步编程的一个重大改进，提供了在不阻塞主线程的情况下使用同步代码实现异步访问资源的能力，并且使得代码逻辑更加清晰。 

### 生成器（Generator） 

生成器函数是一个带星号函数，而且是可以暂停执行和恢复执行的 

1.  在生成器函数内部执行一段代码，如果遇到 yield 关键字，那么 JavaScript 引擎将返回关键字后面的内容给外部，并暂停该函数的执行。
2. 外部函数可以通过 next 方法恢复函数的执行。 

### 协程（Coroutine） 

 协程是一种比线程更加轻量级的存在。你可以把协程看成是跑在线程上的任务，一个线程上可以存在多个协程，但是在线程上同时只能执行一个协程，比如当前执行的是 A 协程，要启动 B 协程，那么 A 协程就需要将主线程的控制权交给 B 协程，这就体现在 A 协程暂停执行，B 协程恢复执行；同样，也可以从 B 协程中启动 A 协程。通常，如果从 A 协程启动 B 协程，我们就把 A 协程称为 B 协程的父协程。 

 正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程。最重要的是，协程不是被操作系统内核所管理，而完全是由程序所控制（也就是在用户态执行）。这样带来的好处就是性能得到了很大的提升，不会像线程切换那样消耗资源。 

```javascript

function* genDemo() {
    console.log("开始执行第一段")
    yield 'generator 2'

    console.log("开始执行第二段")
    yield 'generator 2'
    
    console.log("执行结束")
    return 'generator 2'
}

console.log('main 0')
let gen = genDemo()
console.log(gen.next().value)
console.log('main 1')
console.log(gen.next().value)
console.log('main 2')
console.log(gen.next().value)
console.log('main 3')

------------main 0
------------开始执行第一段
------------generator 2
------------main 1
------------开始执行第二段
------------generator 2
------------main 2
------------执行结束
------------generator 2
------------main 3

```

 父协程有自己的调用栈，gen 协程时也有自己的调用栈 ，那么栈的使用情况是怎样的



![](https://static001.geekbang.org/resource/image/92/40/925f4a9a1c85374352ee93c5e3c41440.png)

### async 

async 是一个通过异步执行并隐式返回 Promise 作为结果的函数。

对 async 函数的理解，这里需要重点关注两个词：**异步执行和隐式返回 Promise**。 

```javascript

async function foo() {
    return 2
}
console.log(foo())  // Promise {<resolved>: 2}
```

###  await 

```javascript
  async function foo() {
        console.log(1)
        let a = await 100
        console.log(a)
        console.log(2)
    }
    console.log(0)
    foo()
    console.log(3)
----------------------0
----------------------1
----------------------3
----------------------100
----------------------2
```

 当执行到await 100时，会默认创建一个 Promise 对象 

```javascript
let promise_ = new Promise((resolve,reject){
  resolve(100)
})
```

 JavaScript 引擎会将该任务提交给微任务队列 

 然后 JavaScript 引擎会暂停当前协程的执行，将主线程的控制权转交给父协程执行，同时会将 promise_ 对象返回给父协程。 

 主线程的控制权已经交给父协程了，这时候父协程要做的一件事是调用 promise_.then 来监控 promise 状态的改变。 

 接下来继续执行父协程的流程，这里我们执行console.log(3)，并打印出来 3。随后父协程将执行结束，在结束之前，会进入微任务的检查点，然后执行微任务队列，微任务队列中有resolve(100)的任务等待执行，执行到这里的时候，会触发 promise_.then 中的回调函数，如下所示： 

```javascript

promise_.then((value)=>{
   //回调函数被激活后
  //将主线程控制权交给foo协程，并将vaule值传给协程
})
```

 该回调函数被激活以后，会将主线程的控制权交给 foo 函数的协程，并同时将 value 值传给该协程。 

 foo 协程激活之后，会把刚才的 value 值赋给了变量 a，然后 foo 协程继续执行后续语句，执行完成之后，将控制权归还给父协程。 

 以上就是 await/async 的执行流程。 