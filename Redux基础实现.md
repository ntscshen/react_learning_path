> 使用Redux的目的：为整个应用提供一个唯一可信任的数据源( Store )

![http://occeqxmsk.bkt.clouddn.com/storeDispatch.svg](http://occeqxmsk.bkt.clouddn.com/storeDispatch.svg)
> 在没理解之前阅读文档，大概率是看不懂它在说什么的，理解了之后再看文档就会觉得，说的好有道理。那先从最基本的使用开始(“先观其表”)，然后尝试着理解其源码实现，最后通过项目+文档，去更加深入的理解

### Redux基础用法
```javascript
// 状态变更的具体实现
let reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'delItem':
      let shen = state.list.filter((item, idx) => idx !== action.value);
      return Object.assign({}, state, { list: shen });
      break;
    default:
      return state;
  }
};
// 1. 创建createStore(reducer)
let store = createStore(reducer);
// 2. 获取store中的状态
this.state = store.getState();
// 3. 监听store状态的变化，以修改组件中的state
store.subscribe(() => {
  this.setState(store.getState());
});
// 4. 某一个UI状态的变更，用对象描述其变更
const action = { type: 'delItem', value: index };
// 5. 派发给reducer处理
store.dispatch(action);

// 创建state的唯一方法就是通过createStore(reducer);
// 修改state的唯一方法就是通过派发action：store.dispatch(action);
// 获取state的唯一办法就是订阅数据的变化：this.setState(store.getState());
// 更新state：store.subscribe();

用户触发状态更新 -> 创建action并dispatch -> Reducer处理并返回新状态更新Store状态  -> 发布更新状态通知更新组件state
```

>一个简易的状态管理实现：
### 创建宿主环境
```javascript
function createStore(reducer) {
  let state;// 默认Redux状态
  let listeners = [];// 订阅
  function getState() {}
  function dispatch(action) {}
  function subscribe(listener) {}
  return {getState, dispatch, subscribe};
}
```
### 获取State数据
```javascript
function getState() {
  // state为不可变数据
  return JSON.parse(JSON.stringify(state));
}
```
### 修改State数据(派发 - 具体实现)
```javascript
function dispatch(action) {
  // 1. 传入(原始state数据, 需要修改的state数据) ,返回新的修改后数据, 变更state
  state = reducer(state, action);
  // 2. 更新组件中订阅的内容
  for (let i = 0; i < listeners.length; i++) {
    const listener = listeners[i];
    listener();
  }
}
function reducer(state, action) {
  switch(action.type) {
    case 'delItem':
      let newList = state.list.filter((item, idx) => idx !== action.value);
      return {
        ...state,
        list: newList
      }
      break;
    default:
      return state;
  }
}

// 派发给reducer处理
const action = { type: 'delItem', value: index };
store.dispatch(action);
```

### 订阅State数据的变更
```javascript
function subscribe(listener) {
  listeners.push(listener);// 订阅
  return function() {// 取消订阅
    listeners = listeners.filter(item => item !== listener);
  };
}
```
> 在React组件内需要写的有：
1. 创建Redux环境(createStore)
2. 获取初始状态(getState)
3. 监听状态变更(subscribe)
4. 触发状态修改(dispatch)
>在Redux中需要写的有：
1. 获取内部状态(getState)
2. 修改内部状态(dispatch) - 修改后需要通通知订阅(subscribe)内容执行
3. 订阅(subscribe)
4. reducer(变更私有数据(通过对比老数据和需要修改的数据得到最新的数据)，action只是描述了即将要发生状态变更的事实