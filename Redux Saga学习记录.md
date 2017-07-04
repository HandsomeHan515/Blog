# Redux Saga

## 简述
- Reducers负责处理action的state更新；
- Sagas负责协调那些复杂或异步的操作。

## 安装

    npm install --save redux-saga

```
// ...
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

// ...
import { rootSaga } from './sagas'

const sagaMiddleware = createSagaMiddleware()
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)
sagaMiddleware.run(rootSaga)

const action = type => store.dispatch({type})

```
## 辅助函数
    
    用来在一些特定的action被发起到Store时派生任务。
    
1. takeEvery
2. put: 用于创建dispatch Effect
3. take: 通过全面控制action观察进程来构建复杂的控制流
4. fork: 无阻塞调用

---

```
yield fetch(url) => yield call(fetch, url)

```
### take
    等待dispatch匹配某个action
```
while(true) {
    yield take('Click_Action')
    yield fork(clickButtonSaga)
}
```

### put 
    触发某个action,作用和dispatch相同
```
yield put({type: 'CLICK'})
```
   
```
//具体例子
import { call, put } from 'redux-saga/effects'

export function* fetchData(action) {
    try {
        const data = yield call(fetch, url)
        yield put({type: 'FETCH_SUCCESS', data})
    } catch (error) {
        yield put({type: 'FETCH_FAILED', error})
    }
}
```
### call
    有阻塞的调用saga或者返回promise的函数，只在触发某个动作
    
### takeEvery
    循环监听某个触发动作，通常会使用while循环替代
```
import {takeEvery} from 'redux-saga/effects'

function* watchFetchData() {
  yield takeEvery('FETCH_REQUESTED', fetchData)
}
```
### takeLatest
    对于触发多个action的时候，只执行最后一个，其他的会自动取消
```
import { takeLatest } from 'redux-saga/effects'

function* watchFetchData() {
  yield takeLatest('FETCH_REQUESTED', fetchData)
}
```
### fork和cancel
    通常fork和cancel配合使用，实现非阻塞任务，take是阻塞状态，也就是实现执行take的时候，无法继续向下执行，fork是非阻塞的，同样可以使用cancel取消一个fork任务
```
function* authorize(user, password) {
  try {
    const token = yield call(Api.authorize, user, password)
    yield put({type: 'LOGIN_SUCCESS', token})
  } catch(error) {
    yield put({type: 'LOGIN_ERROR', error})
  }
}

function* loginFlow() {
  while(true) {
    const {user, password} = yield take('LOGIN_REQUEST')
    yield fork(authorize, user, password)
    yield take(['LOGOUT', 'LOGIN_ERROR'])
    yield call(Api.clearItem('token'))
  }
}
```
    当执行yield fork(authorize, user, password),同时执行yield take(['LOGOUT', 'LOGIN_ERROR'])
    
### 错误处理
    我们假设远程数据读取因为某些原因失败了，API函数API.fetch返回一个被拒绝(rejected)的Promise
```
import Api from './Api'
import { call, put } from 'redux-saga/effects'

// ...
function* fetchProducts() {
    try {
        const products = yield call(Api.fetch, '/products')
        yield put({ type: 'PRODUCTS_RECEIVED', products })
    } catch (err) {
        yield put({ type: 'PRODUCTS_REQUEST_FAILED', err)
    }
}
```

### takeEvery的使用
    saga中的take并不支持action的循环调用，即遍历数组执行action，为解决该问题，可以使用takeEvery来执行action，此时即可实现遍历数组执行action。
    
```
import { call, put, takeEvery } from 'redux-saga/effects'

function* fetchUsr(action) {
    const payload = action
    try {
        const user = yield call(api.getUser, payload)
        yield put({ type: GET_ENTITIES_USER, user })
    } catch (err) {
        console.log('err: %o', err)
    }
}

function* mySaga() {
    //在每个 'GET_USER' action 被发起时调用 fetchUser
    //允许并发（译注：即同时处理多个相同的 action）
    takeEvery(actions.GET_USER, fecthUser)
}
```
