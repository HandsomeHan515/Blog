# React-Redux
## 一、认识Redux
### 安装
    npm install --save redux
    npm install --save react-redux
### 一句话理解Redux

    应用中的所有state都以一个对象数树的方式存储在一个单一的store中，唯一改变state的方法就是触发action，一个描述发生什么的对象，为了描述action如何改变state树，需要编写reducers。

### 什么是Action？
    Action本质上是JavaScript的普通对象，action内必须使用一个字符串类型的type字段来表示将要执行的动作。多数情况下type会被定义成字符串常量。我们应该尽量减少在actin中传递数据。
```
Action：
export const ADD = 'ADD'
export const add = () => ({
    type: ADD
})

export const SUB = 'SUB'
export const sub = () => ({
    type: SUB
})
```
### 什么是Reducer？

1. 设计state结构
2. Action处理

**reducer是一个纯函数，接受旧的state和action，返回新的state。**

**注意: 每个 reducer 只负责管理全局 state 中它负责的一部分。每个 reducer 的 state 参数都不同，分别对应它管理的那部分 state 数据。**
```
Reducer：
import { ADD, SUB } from 'Action'

const counter = (state = 0, action) => {
    switch (action.type) {
        case ADD: 
            return state + 1
        case SUB:
            return state - 1
        default: 
            return state
    }
}
```
   
**总结：把要做的修改变成一个普通对象，这个对象叫做action，而不是直接修改state。然后编写专门的函数来决定每个action如何改变应用的state，这个函数叫做reducer。**

---

## 二、关于Redux的重点知识
### 1. 三大原则

#### (1)单一数据源
    整个应用的state被存储在一棵object tree中，并且这个object tree只存在于唯一一个store中。
#### (2)state是只读的
    唯一改变state的方法就是触发action，action是一个用于描述已发生事件的普通对象。
#### (3)使用纯函数来执行修改
    为了描述action如何改变state,需要写reducers。

**注意：永远不要在reducer中做这些操作**

1. 修改传入参数；
2. 执行有副作用的操作，如API请求和路由跳转；
3. 调用非纯函数，如Date.now(), Math.random()。

**只要传入参数相同，返回计算得到的下一个state就一定相同。没有特殊情况，没有副作用，没有API请求，没有变量修改，单纯执行计算。**

### 2.combineReducer
#### 为什么使用combineReducer?

    随着应用变得复杂，需要对 reducer 函数进行拆分，拆分后的每一块独立负责管理 state 的一部分。
    
    combineReducers 辅助函数的作用是，把一个由多个不同 reducer 函数作为 value 的 object，合并成一个最终的 reducer 函数，然后就可以对这个 reducer 调用 createStore。

    合并后的 reducer 可以调用各个子 reducer，并把它们的结果合并成一个 state 对象。state 对象的结构由传入的多个 reducer 的 key 决定。
    
    
```
import { combineReducer } from 'redux'

const todoApp = combineReducer({
    visibilityFilter,
    todos
})

export default todoApp
```

等价写法 =>

```
export const todoApp = (state = {}, action) => {
    return {
        visibilityFilter: visibilityFilter(state.visibilityFilter, action)
        todos: todos(state.todos, action)
    }
}
```

**conbineReducer所做的是生成一个函数，这个函数来调用你的一系列reducer,每个reducer根据他的key来筛选出state中的一部分数据并处理，然后这个生成的函数再将所有reducer的结果合并成一个大的对象。**

#### combineReducer示例：
##### reducers/todos.js
```
export default function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return state.concat([action.text])
  default:
    return state
  }
}
```
##### reducers/counter.js
```
export default function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1
  case 'DECREMENT':
    return state - 1
  default:
    return state
  }
}
```
##### reducers/index.js
```
import { combineReducers } from 'redux'
import todos from './todos'
import counter from './counter'

export default combineReducers({
  todos,
  counter
})
```
##### App.js
```
import { createStore } from 'redux'
import reducer from './reducers/index'

let store = createStore(reducer)
```

### 3.Redux中的connect()方法
#### (1)定义mapStateToProps方法
    
    把当前Redux store state映射到展示组件的props中。
    
#### (2)定义mapDispatchToProps方法

    接收dispatch()方法并返回期望注入到展示组件的props中的回调方法。

```
import { connect } from 'react-redux'
import { Task } from './reducers'

const mapStateToProps = (state) => {
    return {
        tasks: state.entities.tasks
    }
}

const mapDispatchToProps = (dispatch) => {
    return {
        addUnit: (taskID, unitID, units) => {
            dispatch(Task.addUnit(taskID, unitID, units))
        }
    }
}

等价于 =>

const mapDispatchToProps = (dispatch) => {
    return {
        addUnit: (taskID, unitID, units) => Task.addUnit(taskID, unitID, units))
    }
}


//多个conncet的书写方式
const VisibleCreateUnit = connect(
    mapStateToProps,
    mapDispatchToProps
)(CreateUnit)

const VisibleCreateTask = connect(
    mapStateToProps,
    mapDispatchToProps
)(CreateTask)

export { VisibleCreateUnit, VisibleCreateTask }

```

### 4.Store
#### (1)createStore()
**createStore() 的第二个参数是可选的, 用于设置 state 初始状态。这对开发同构应用时非常有用，服务器端 redux 应用的 state 结构可以与客户端保持一致, 那么客户端可以将从网络接收到的服务端 state 直接用于本地数据初始化。**
#### (2)Redux Provider
```
import { Provider } from 'react-redux'
import { createStore } from 'redux'
```
##### 在React-router中使用Provider

```
const store = createStore(appReducer, normalizedState)

<Provider store={store}>
    <Router history={browserHistory}>
        <Route path="/app" component={App} />
    </Router>
</Provider>
```
### 5.数据序列化(normalize)

    示例：Tasks中包含subTasks和units,subTasks中又包含units。
```
import { normalize, schema } from 'normalizr'

//首先使用schema最小单元即units
const unitsSchema = new schema.Entity('units')
//subTasks中包含units
const subTasksSchema = new schema.Entity('subTasks', { units: [unitsSchema] })
//tasks中包含subTasks和units
const tasksSchema = new schema.Entity('tasks', { 
    subTasks: [subTasksSchema],
    units: [unitsSchema]
})


......
const initialState = {
    tasks: {
        subTasks: {
            units: {
                ......
            }
            ......
        },
        units: {
            ......
        }
    },
    groups: {
        ......
    },
    classes: {
        ......
    },
    rosters: {
        ......
    }
}

const stateSchema = {
    tasks: [tasksSchema],
    groups: [groupsSchema],
    classes: [classesSchema],
    rosters: [rostersSchema]
}

const normalizedState = normalize(initialState, stateSchema)
console.log('normalizedState', normalizedState)

Object{
    entities: Object{
        .....
    }
    result: Object{
        tasks: array[2],
        groups: array[3],
        classes: array[4],
        rosters: array[5]
    }
}

```
**注意：键一定与数组中的键一一对应**

## Redux工作流程
1、用户发出Action
```
store.dispatch(action)
```
2、Store自动调用reducer,并且传入两个参数： 当前state, action。Reducer返回新的State
```
let nextState = reducer(previousState, action)
```
3、State一但有变化，store就会调用监听函数
```
store.subscribe(listener)
function listener() {
    let newState = store.getState()
    component.setState(newState)
}
```