# 先说说最近用的hooks吧

### 为什么要用HOOKS

#### 复用一个有状态的组件太麻烦

 react都核心思想是，将一个页面拆成一堆独立的，可复用的组件，并且用自上而下的单向数据流的形式将这些组件串联起来。但假如你在大型的工作项目中用react，你会发现你的项目中实际上很多react组件冗长且难以复用。尤其是那些写成class的组件，它们本身包含了状态（state），所以复用这类组件就变得很麻烦。

那之前，官方推荐怎么解决这个问题呢？答案是：[渲染属性（Render Props）](https://reactjs.org/docs/render-props.html)和[高阶组件（Higher-Order Components）](https://reactjs.org/docs/higher-order-components.html)。我们可以稍微跑下题简单看一下这两种模式。

渲染属性指的是使用一个值为函数的prop来传递需要动态渲染的nodes或组件。如下面的代码可以看到我们的`DataProvider`组件包含了所有跟状态相关的代码，而`Cat`组件则可以是一个单纯的展示型组件，这样一来`DataProvider`就可以单独复用了。

```
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

```
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
> useReducer                        跟 redux 中的数据流的概念非常接近 
>
> useRef                                返回一个可变的 ref 对象 
>
> useContext                        监听 provider 更新变化 

具体的用法...自己实践

### 先对比函数式和类的区别吧

<img src="C:\Users\QWQ\AppData\Roaming\Typora\typora-user-images\image-20191030154705754.png" alt="image-20191030154705754" style="zoom: 80%;" />



Hook 的使用范围：函数式的 React 组件中、自定义的 Hook 函数里；

Hook 必须写在函数的最外层，每一次 useState 都会改变其下标 (cursor)，React 根据其顺序来更新状态；

尽管每一次渲染都会执行 Hook API，但是产生的状态 (state) 始终是一个常量（作用域在函数内部）；



