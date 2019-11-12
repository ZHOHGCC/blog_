# React Hooks基本的用法

Ps: react有太多骚写法咯，个人水平有限...正在学习



React 团队希望，组件不要变成复杂的容器，最好只是数据流的管道。开发者根据需要，组合管道即可。 **组件的最佳写法应该是函数，而不是类。** （顺势而行，更方便）



### useState 状态钩子  

Two 这个组件 改变One中count的值

> ```javascript
> function One (props) {
> const [count, setCount] = useState(0)
> return (
> <div>this is one  {count}<Two {...{ count, setCount }}></Two> </div>
> )
> }
> 
> function Two (props) {
> const { setCount, count } = props
> return <div>this is Two  <button onClick={() => { setCount(count + 1) }}>click it!</button> </div>
> }
> ```
> ![usestate.png](http://blog.talentedpanda.cn/upload/images/1573524292011.png)

### useContext 共享状态钩子

```javascript
const OneContext = createContext({})
function One (props) {
  return (
    <OneContext.Provider value={{ name: "this is onecontext" }}>
      <div>this is one <Two ></Two> </div>
    </OneContext.Provider>
  )
}

function Two (props) {
  const { name } = useContext(OneContext)
  return <div style={{ 'color': 'red' }}>this is Two {name}  </div>
}
```
![useContext.png](http://blog.talentedpanda.cn/upload/images/1573524318559.png)
### useReducer()：action 钩子

```javascript
const reducer = (state, action) => {
  switch (action.type) {
    case 'one':
      return { ...state, count: state.count + 1 }
    case 'two':
      return { ...state, count: state.count + 2 }
  }

}
function One (props) {
  const [state, dispatch] = useReducer(reducer, { count: 0 })
  return (

    <div>this is one
       <button onClick={() => { dispatch({ type: 'one' }) }}>+1</button>
       <Two dispatch={dispatch}></Two>{state.count} </div>
  )
}
function Two (props) {
  const { dispatch } = props
  return <div style={{ 'color': 'red' }}>this is Two  
         <button onClick={() => { dispatch({ type: 'two' }) }}>+2</button></div>
}

```
![useReduer.png](http://blog.talentedpanda.cn/upload/images/1573524343669.png)
### useEffect()：副作用钩子   

PS：这东西用起来真棒。挺简单，用处贼大，代替一些生命周期 

```
useEffect(() => {
    console.log('this is effect')
  }, [])
```

 `useEffect()`接受两个参数。第一个参数是一个函数，异步操作的代码放在里面。第二个参数是一个数组，用于给出 Effect 的依赖项，只要这个数组发生变化，`useEffect()`就会执行。第二个参数可以省略，这时每次组件渲染时，就会执行`useEffect()` ，如果传空数组的话 就只执行一次~



### useMemo，useCallback

避免过度渲染的问题  看情况使用