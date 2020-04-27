# 先说说最近用的hooks吧

### 为什么要用HOOKS

#### 复用一个有状态的组件太麻烦

 react都核心思想是，将一个页面拆成一堆独立的，可复用的组件，并且用自上而下的单向数据流的形式将这些组件串联起来。但假如你在大型的工作项目中用react，你会发现你的项目中实际上很多react组件冗长且难以复用。尤其是那些写成class的组件，它们本身包含了状态（state），所以复用这类组件就变得很麻烦。

那之前，官方推荐怎么解决这个问题呢？答案是：[渲染属性（Render Props）](https://reactjs.org/docs/render-props.html)和[高阶组件（Higher-Order Components）](https://reactjs.org/docs/higher-order-components.html)。我们可以稍微跑下题简单看一下这两种模式。

渲染属性指的是使用一个值为函数的prop来传递需要动态渲染的nodes或组件。如下面的代码可以看到我们的`DataProvider`组件包含了所有跟状态相关的代码，而`Cat`组件则可以是一个单纯的展示型组件，这样一来`DataProvider`就可以单独复用了。

```javascript
import Cat from 'components/cat'
class DataProvider extends React.Component {
  constructor(props) {
    super(props);
    this.state = { target: 'Zac' };
  }

  render() {
    return (
      <div>
        {this.props.render(this.state)}
      </div>
    )
  }
}

<DataProvider render={data => (
  <Cat target={data.target} />
)}/>

复制代码
```

虽然这个模式叫Render Props，但不是说非用一个叫render的props不可，习惯上大家更常写成下面这种：

```
...
<DataProvider>
  {data => (
    <Cat target={data.target} />
  )}
</DataProvider>
复制代码
```

高阶组件这个概念就更好理解了，说白了就是一个函数接受一个组件作为参数，经过一系列加工后，最后返回一个新的组件。看下面的代码示例，`withUser`函数就是一个高阶组件，它返回了一个新的组件，这个组件具有了它提供的获取用户信息的功能。

```javascript
const withUser = WrappedComponent => {
  const user = sessionStorage.getItem("user");
  return props => <WrappedComponent user={user} {...props} />;
};

const UserPage = props => (
  <div class="user-container">
    <p>My name is {props.user}!</p>
  </div>
);

export default withUser(UserPage);
```

 以上这两种模式看上去都挺不错的，很多库也运用了这种模式，比如我们常用的React Router。但我们仔细看这两种模式，会发现它们会增加我们代码的层级关系。 



#### 生命周期钩子函数里的逻辑.......o(︶︿︶)o 

我们通常希望一个函数只做一件事情，但我们的生命周期钩子函数里通常同时做了很多事情。比如我们需要在`componentDidMount`中发起ajax请求获取数据，绑定一些事件监听等等。同时，有时候我们还需要在`componentDidUpdate`做一遍同样的事情。当项目变复杂后，这一块的代码也变得不那么直观。



## HOOKS来啦

> useState                            维护组件状态 
>
> useMemo  useCallback   防止不必要的重渲染
>
> useEffect                            处理副作用 ，可以替换部分生命周期
>
> useReducer                       跟 redux 中的数据流的概念非常接近 
>
> useRef                                返回一个可变的 ref 对象 
>
> useContext                        监听 provider 更新变化 

具体的用法.....就太多了

### 先对比函数式和类的区别吧

![QN@9JAZ$0S(L2L45)L9L8CU](C:\Users\QWQ\Desktop\blog\blog_\react相关\QN@9JAZ$0S(L2L45)L9L8CU.png)

Hook 的使用范围：函数式的 React 组件中、自定义的 Hook 函数里；

Hook 必须写在函数的最外层，每一次 useState 都会改变其下标 (cursor)，React 根据其顺序来更新状态；

尽管每一次渲染都会执行 Hook API，但是产生的状态 (state) 始终是一个常量（作用域在函数内部）；



##  context

creactContext(defaultValue?)  创造context  

会影响组件独立性,不能大规模使用

```react
import React, { createContext } from 'react'
const AgeContext = createContext(20)


//-------------
function Leaf () {
    return (<div>
        <AgeContext.Consumer>
            {(data) => { return (<h1>{data} </h1>) }}
        </AgeContext.Consumer>
    </div>)
}
function Middle () {
    return (<div>
        <Leaf></Leaf>
    </div>)
}
function Hooks () {
    return (
        <AgeContext.Provider value={10}>
            <Middle></Middle>
        </AgeContext.Provider>
    )
}
export default Hooks

```



## lazy,suspense

lazy 延迟加载

suspense 加载完成之前所显示的 

# Hooks

**类组件** ：

**1.难以复用状态逻辑**：

缺少复用机制

渲染属性和高阶组件 导致层级多

**2.难以维护**：

生命周期混杂 

相关逻辑分布在不同生命周期中

3.this**指向问题**

内联函数会过度创建句柄，导致子组件过多渲染

类成员函数不能保证this

**Hooks优势**

- 没有this的问题
- 自定义hooks方便复用状态逻辑
- 副作用的关注点分离

规定

仅仅在顶层调用Hooks函数 ，保证在不同的渲染周期中hooks函数的调用顺序不变

不能再其他普通函数里调用，仅在自定义hook和函数组件里调用

## useState()

```react
import React, { useState } from 'react'

function Hooks () {
    //--------------------------------------useState只是返回变量，返回的结果按照顺序返回，底层是数组    js是单线程的保证全局唯一性。
    // 支持传入函数 可做判断(判断语句只会执行一次)
    const [count, setCount] = useState(0)
    return (
        <div>
            <button onClick={() => { setCount(count + 1) }}>1212</button>
            {count}
        </div>
    )
}
```

## useEffect

```react
 
useEffect(fn,可选参数) 执行的是副作用 渲染之后才执行  (您可能之前已经执行过数据获取，订阅或手动从React组件更改DOM的操作。我们将这些操作称为“副作用”（或简称为“效果”），因为它们会影响其他组件，并且在渲染过程中无法完成。)
可选参数为
空：每次渲染后都执行  相当于 componentDidMount 和 componentDidUpdate:
空数组：第一次执行一次。之后就不执行  相当于 componentDidMount
为数组：数组内的元素改变则执行

fn返回一个回调函数
  // useEffect 在执行副作用函数之前，会先调用上一次返回的函数
  // 如果要清除副作用，要么返回一个清除副作用的函数
 useEffect 函数有要求：要么返回清除副作用函数，要么就不返回任何内容
```

## useContext

```react
使用 const countContext = createContext()  来创建context
使用 const count = useContext(countContext) 来获取
```

## useMemo

```react
memo针对的是一个组件是否重复执行，useMemo是针对一个函数是否重复执行
memo和useMemo 不管用不用都不会影响业务（记住，传入 useMemo 的函数会在渲染期间执行。请不要在这个函数内部执行与渲染无关的操作，诸如副作用这类的操作属于 useEffect 的适用范畴，而不是 useMemo。）


useMemo是在渲染期间完成的
useMemo根据依赖是否变化来决定函数是否执行
useMemo(fn,数组)
 const double = useMemo(() => {
        return count * 2
    }, [count === 2])  执行条件和useEffect一样    如果未提供数组，则将在每个渲染上计算一个新值。
 当count=2或者3的时候会执行 false-true-false

```

useCallback

```react
//当useMemo返回的是函数 则可以使用useCallback（）来简化操作
const xx = useCallback(()=>{
   fn
},[])
```

## useRef

- 获得子组件或者dom的句柄

- 渲染周期之间共享数据的存储  //相当于 类的属性成员

  比如 在函数内创建了一个定时器，消除定时器的话，就可以使用useRef

  ```react
  const timeRef = useRef()
  timeRef.current = setTimeout(fn,5000)
  ```

  

函数组件无法获得子组件的句柄 ，因为函数组件没法实例化

## 自定义Hook

- 自定义 Hook 更像是一种约定，而不是一种功能。如果函数的名字以 use 开头，并且调用了其他的 Hook，则就称其为一个自定义 Hook
- 有时候我们会想要在组件之间重用一些状态逻辑，之前要么用 render props ，要么用高阶组件，要么使用 redux
- 自定义 Hook 可以让你在不增加组件的情况下达到同样的目的
- **Hook 是一种复用状态逻辑的方式，它不复用 state 本身**
- **事实上 Hook 的每次调用都有一个完全独立的 state**

