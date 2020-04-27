



## Redux,Vuex

 尤大也说过VUEX是吸收了Redux的经验，放弃了一些特性并做了一些优化，代价就是VUEX只能和VUE配合。

而Redux则是一个纯粹的状态管理系统，React利用React-Redux将它与React框架结合起来。  

#### 对比：
因为VUEX可以以一敌多，接下来将对以下两方面进行分析
- Redux vs VUEX
- React-Redux vs VUEX

#### I

###### Redux
- 核心对象：store
- 数据存储：state
- 状态更新提交接口：==dispatch==
- 状态更新提交参数：带type和payload的==Action==
- 状态更新计算：==reducer==
- 限制：reducer必须是纯函数，不支持异步
- 特性：支持中间件

###### VUEX
- 核心对象：store
- 数据存储：state
- 状态更新提交接口：==commit==
- 状态更新提交参数：带type和payload的mutation==提交对象/参数==
- 状态更新计算：==mutation handler==
- 限制：mutation handler必须是非异步方法
- 特性：支持带缓存的getter，用于获取state经过某些计算后的值

###### Redux vs VUEX 对比分析
store和state是最基本的概念，VUEX没有做出改变。其实VUEX对整个框架思想并没有任何改变，只是某些内容变化了名称或者叫法，通过改名，以图在一些细节概念上有所区分。
- **VUEX弱化了dispatch的存在感**。VUEX认为状态变更的触发是一次“提交”而已，而调用方式则是框架提供一个提交的commit API接口。
- **VUEX取消了Redux中Action的概念**。不同于Redux认为状态变更必须是由一次"行为"触发，VUEX仅仅认为在任何时候触发状态变化只需要进行mutation即可。Redux的Action必须是一个对象，而VUEX认为只要传递必要的参数即可，形式不做要求。
- **VUEX也弱化了Redux中的reducer的概念**。reducer在计算机领域语义应该是"规约"，在这里意思应该是根据旧的state和Action的传入参数，"规约"出新的state。在VUEX中，对应的是mutation，即"转变"，只是根据入参对旧state进行"转变"而已。

总的来说，VUEX通过弱化概念，在任何东西都没做实质性削减的基础上，使得整套框架更易于理解了。

另外VUEX支持getter，运行中是带缓存的，算是对提升性能方面做了些优化工作，言外之意也是鼓励大家多使用getter。

#### II

###### React-Redux
- 状态注入组件：==<Provider/>组件结合connect方法==
- ==容器组件：通过connect关联了state的组件，并被传入dispatch接口==
- 展示组件：不与state或dispatch直接产生关系
- 特性：connect支持mapStatesToProps方法，用于自定义映射

###### VUEX
- 状态注入组件：==Vue.use(Vuex)将Vuex应用为全局的plugin，再将store对象传入根VUE实例==
- ==容器组件：没有这个概念==
- 展示组件：在组件中可以获取this.$store.state.*，也进行this.$store.commit()等等
- 特性：VUEX提供mapState，mapGetter，mapMutation等方法，用于生成store内部属性对组件内部属性的映射

###### React-Redux vs VUEX 对比分析
通过使用方式上的较大差异，也可以看出理念上的不同。
- **和组件结合方式的差异**。VUE通过VUEX全局插件的使用，结合将store传入根实例的过程，就可以使得store对象在运行时存在于任何vue组件中。而React-Redux则除了需要在较外层组件结构中使用<Provider/>以拿到store之外，还需要显式指定容器组件，即用connect包装一下该组件。这样看来我认为VUE是更推荐在使用了VUEX的框架中的每个组件内部都使用store，而React-Redux则提供了自由选择性。而VUEX即不需要使用外层组件，也不需要类似connect方式将组件做一次包装，我认为出发点应该是可能是为了避免啰嗦。
- **容器组件的差异**。React-Redux提倡容器组件和表现组件分离的最佳实践，而VUEX框架下不做区分，全都是表现（展示）组件。我觉得不分优劣，React-Redux的做法更清晰、更具有强制性和规范性，而VUEX的方式更加简化和易于理解。

总的来说，就是谁包谁，谁插谁的问题。Redux毕竟是独立于React的状态管理，它与React的结合则需要对React组件进行一下外包装。而VUEX就是为VUE定制，作为插件、以及使用插入的方式就可以生效，而且提供了很大的灵活性。
