## Redux

 如果你的UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。 

> - 用户的使用方式复杂
> - 不同身份的用户有不同的使用方式（比如普通用户和管理员）
> - 多个用户之间可以协作
> - 与服务器大量交互，或者使用了WebSocket
> - View要从多个来源获取数据

上面这些情况才是 Redux 的适用场景：多交互、多数据源   

[阮一峰老师的博客]: http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html



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