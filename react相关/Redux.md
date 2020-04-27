



## Redux

 如果你的UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。 

> - 用户的使用方式复杂
> - 不同身份的用户有不同的使用方式（比如普通用户和管理员）
> - 多个用户之间可以协作
> - 与服务器大量交互，或者使用了WebSocket
> - View要从多个来源获取数据

上面这些情况才是 Redux 的适用场景：多交互、多数据源   

[阮一峰老师的博客]: http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html



创建store()

```javascript
import { createStore } from 'redux'
import reducer from './reducer'
const store = createStore(
    reducer,
   // redux的调试
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
export default store
```

store.getState() 获得state



action  就像 一句话 

```javascript
 const action = {
            type: 'change_input_value',
            value:e.target.value
        }
 //执行 dispatch之后，store就会把数据传给reducer,让reducer根据传入值做出动作
 store.dispatch(action)
```

reducer  推荐使用object.assgin()来进行拷贝

**必须是个纯函数  有固定输入就一定有固定输出，而且不会有副作用**



解决拼写错误

使用变量保存  action.type



store.subscribe(fn)

用来监听store是否发生变化



Reducer 函数负责生成 State。由于整个应用只有一个 State 对象，包含所有数据，对于大型应用来说，这个 State 必然十分庞大，导致 Reducer 函数也十分庞大。

请看下面的例子。

> ```javascript
> const chatReducer = (state = defaultState, action = {}) => {
>   const { type, payload } = action;
>   switch (type) {
>     case ADD_CHAT:
>       return Object.assign({}, state, {
>         chatLog: state.chatLog.concat(payload)
>       });
>     case CHANGE_STATUS:
>       return Object.assign({}, state, {
>         statusMessage: payload
>       });
>     case CHANGE_USERNAME:
>       return Object.assign({}, state, {
>         userName: payload
>       });
>     default: return state;
>   }
> };
> ```





# redux-thunk

```javascript
//store.js
const composeEnhancers =
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
        window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
        }) : compose;

const enhancer = composeEnhancers(
    applyMiddleware(thunk),
    // other store enhancers if any

);
const store = createStore(reducer,
    enhancer
);

//-------------
 inputChange (e) {
        const action = getData()
        store.dispatch(action)
    }
//action.js
export const getData = () => {
    return (dispatch) => {
        const data = ['zc', 'ywy']
        setTimeout(() => {
            const action = initDataAction(data)
            dispatch(action)
        }, 1000)
    }
}
```





# redux saga

```javascript
import createSagaMiddleware from 'redux-saga'
import saga from './saga.js'
//生成中间件
const sagaMiddleware = createSagaMiddleware()

const composeEnhancers =
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
        window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({
        }) : compose;

const enhancer = composeEnhancers(
    applyMiddleware(sagaMiddleware),
    // other store enhancers if any

);
const store = createStore(reducer,
    enhancer
);
//--------执行saga
sagaMiddleware.run(saga)



-----------------------------saga.js
//  捕捉每一个
import { takeEvery } from 'redux-saga/effects'

//gitinit 也可以接受到 dispatch的action 
function* getInit (data) {
    //data是 监听的action 的内容
    console.log('this is getinit')
}
function* mySaga () {
    //执行init_data 执行getinit
    yield takeEvery('init_data', getInit)
}

export default mySaga;
```



### combineReducers

Redux 提供了一个`combineReducers`方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。

> ```javascript
> import { combineReducers } from 'redux';
> 
> const chatReducer = combineReducers({
>   chatLog,
>   statusMessage,
>   userName
> })
> 
> export default todoApp;
> ```

上面的代码通过`combineReducers`方法将三个子 Reducer 合并成一个大的函数。

这种写法有一个前提，就是 State 的属性名必须与子 Reducer 同名。如果不同名，就要采用下面的写法。

> ```javascript
> const reducer = combineReducers({
>   a: doSomethingWithA,
>   b: processB,
>   c: c
> })
> 
> // 等同于
> function reducer(state = {}, action) {
>   return {
>     a: doSomethingWithA(state.a, action),
>     b: processB(state.b, action),
>     c: c(state.c, action)
>   }
> }
> ```

总之，`combineReducers()`做的就是产生一个整体的 Reducer 函数。该函数根据 State 的 key 去执行相应的子 Reducer，并将返回结果合并成一个大的 State 对象。



# react中使用redux

```javascript
export default connect(mapStateToProps,ull)(TodoList)
```

## mapStateToProps

```javascript
const mapStateToProps = (state) =>{
	return {
        //return一个对象 对象的值会被添加到组件的props里面
        value:state.value
    }
}
```

## mapDispatchToProps 

```javascript
//接受store.dispatch方法
const mapDispatchToProps = (dispatch)=>{
    //也是返回一个对象 
    //组件通过 props.setValue来调用
	return {
        setValue(e){
            const action = {
                type:'set_value',
                value:e.target.value
            }
            dispatch(action)
        }
    }
}
```



combine

