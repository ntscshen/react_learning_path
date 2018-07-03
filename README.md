> React 中的一堆概念，全是概念

## React 主要概念

### JSX

- JSX：类似 HTML 的 JavaScript
- 有别于 HTML 的地方：

| HTML                    | JSX                                                                                                |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| class                   | className                                                                                          |
| onclick                 | onClick                                                                                            |
| label 标签中的 for 属性 | htmlFor                                                                                            |
| `style="color: red"`    | `style={{color: red}}`                                                                             |
|                         | 在 JSX 中可以写 JS，用{}包裹起来即可，但是不能使用多行 JS 语句( 但是你可以使用三目或者逻辑运算符 ) |
|                         |                                                                                                    |

JSX 是 React 的特色之一：其他的框架或者在传统开发模式下，通常需要学习一门模板语言。来描述 UI 的一些变量或者函数等等。而 JSX 不是模板语言，其本身就是纯 JavaScript。只是用我们熟悉的 HTML 来描述 JavaScript。

### 组件

1.  组件是 React 的核心概念，React 的基石，组件是构造软件的“零部件”
2.  React 可以理解为一个纯函数，输入什么，得到的就是什么
3.  React 是单向数据传递的，外面传递数据进入，一定是通过 props 的，外界要知道内部发生了什么变化，一定是内部暴露了一个事件
4.  React 组件分为两种：函数组件、类组件

### props 和 state

1.  React 组件正是由 props 和 state 两种类型的数据驱动渲染出组件的 UI
2.  React 组件的数据分为两种(props 和 state)：props 是组件对外的接口，state 是组件对内的接口
3.  props 是只读的，不能在组件内部修改 props
4.  state 是可变的，组件状态变更通过调用 this.setState({})方法来改变
5.  组件 state 必须能代表一个 UI 呈现的完整状态，即组件的任何 UI 改变都可以从 state 的变化中反应出来，
6.  state 中所有状态都应该用于反映组件 UI 的变化，不应该存在通过其他状态计算而来的中间状态
7.  能计算得到的状态就去计算它，就不要在 state 中单独存储
8.  组件经量无状态，所需数据通过 props 传递( 纯组件 )

> 例如：发送一个 Ajax 请求，在数据回来之前你应该在页面使用 Loading，没有必要为 Loading 单独添加一个状态进行管理，可以根据其他的状态数据得到

####### **props**

- React 组件的 props：除了支持字符串类型，可以是任何一种 JavaScript 语言支持的数据类型
- 组件绝对不要去修改传入的 props 值，严格来说，React 并没有办法阻止你去修改传入的 props 对象，这是一条红线，不然可能会遇到不可预料的 Bug
- React 组件要反馈数据給外界，可以通过 props，因为 props 的类型不限于纯数据，也可以是函数，函数类型的 props 就相当于让父组件給子组件一个回调函数，子组件在合适的时机调用函数，带上必要的参数，这样就可以把信息传递給外部组件
- 如果组件需要定义自己的构造函数，一定要在构造函数第一行通过 super 调用父类的 React.Component 的构造函数，如果在构造函数中没有调用 super(props)，那么组件实例被构造之后，所有成员的函数都无法通过 this.props 访问到父组件传递过来的 props 值
- propTypes 类型检查：一个辅助开发的功能，并不会改变组件的行为，产生的错误信息只有开发者才能看得懂。只用在开发环境，最终部署到生产环境，用 babel-react-optimize 自动去掉 propType 即可

####### **设计一个合适的 state**

- 组件中用到的变量是不是应该作为 state 属性：

1.  这个变量是否在组件的 render 方法中使用？如果不是，那么它不应该写在 state 中，更为合适的是普通属性
2.  这个变量是否可以通过其他状态(state)或者属性(props)计算得到？如果是，那么它不应该写在 state 中，计算得到更为合适
3.  这个变量是否在组件的整个生命周期中都保持不变？如果是，那么它不应该写在 state 中，更为合适的是普通属性
4.  如果是通过 props 获取的，那么就不是一个 state，请用 this.props 获取

####### **state 不可变对象**

1.  React 官方建议把 state 当做不可变对象，当 state 中的某个状态发生改变时，应该重新创建这个状态对象。而不是直接修改原来的状态
2.  如果是数字、字符串、布尔，直接給要修改的状态赋值即可
3.  如果状态是数组：不要使用(push、pop、shift、unshift、splice 等方法)，因为这些方法都在原数组的基础上修改的。如果要增加请使用( concat\ES6 展开语法 )，截取使用 slice 方法，过来数据使用 filter 方法
4.  如果状态是普通对象：使用 ES6 的 Object.assgin 和对象扩展语法 `{...preState, name:'ntscshen'}`，创建新的对象经量避免使用会直接修改原对象的方法，而是使用可以返回一个新对象的方法，为了少出 Bug，为了性能(在 shouldComponentUpdate 生命周期中只要比较前后两次状态对象的应用就可以判断 state 是否真的发送改变)

```javascript
this.setState(preState => {
  newState: [...preState, 'React state'];
});
this.setState(preState => {
  newState: [...preState, 'React state'];
  newState: preState.state.concat(['React state']);
});
```

> 1.  slice(包头不包尾)：返回一个含有提取元素的新数组
> 2.  filter()：创建一个新数组，返回一个新的通过测试的元素集合的数组，如果没有通过则返回空数组

#### 有状态组件和无状态组件

1.  无状态组件：不是每个组件都需要定义 state，如果一个组件的内部状态是不变的，就用不到 state，这样的组件称之为无状态组件
2.  有状态组件：一个组价内部状态会发生变化，就需要使用 state 来存储变化，这样的组件称之为有状态组件
3.  原则：应该尽可能多的使用无状态组件，并且在使用无状态组件时，应该尽可能多的将其定义为函数组件( 简洁的多 )，无状态组件不用关心状态的具体变化，只聚焦于 UI 的展示，更容易被复用。
4.  React 的组件设计思路：通过定义少量的有状态组件管理整个应用的变化，并且将状态通过 props 传递給其余的无状态组件，由无状态组件完成页面绝大多数的 UI 渲染工作。
5.  有状态组件：聚焦于处理状态变化的业务逻辑
6.  无状态组件：聚焦于组件的 UI 渲染
7.  当在设计组件时发现一个功能有太多的代码时，就要考虑进行组件拆分，用多个小组件来代替，每个小组件聚焦于单个功能的实现( 零部件 )，最后进行组装，满足复杂业务需求
8.  组件如何拆分：拆分组件最关键的是确定组件的 "边界"，每个组件都应该是可以独立存在的，如果两个组件逻辑太紧密，无法清晰定义各自的职责，那么也许这两个组件本身就不应该被拆分，作为同一个组件也许更为合理。

#### 事件处理

1.  驼峰式( onClick )
2.  对象形式( 处理事件的响应函数以对象形式赋值给事件 )
3.  在 React 事件中，必须显示的调用 `event.preventDefault` 来阻止事件默认行为

####### **事件处理中的 this 绑定**

> React 组件的事件处理最容易出错的地方在与 this 的执行

```javascript
// 使用箭头函数1(直接在React元素中采用箭头函数的形式)
<button onClick={event => {console.log(this.state.num)}}></button>
// 使用箭头函数2 - 逻辑封装
handleClick(event) {
  console.log(this.state.num)
}
<button onClick={event => {this.handleClick(event)}}></button>
```
> 在React元素中采用箭头函数形式的弊端：组件每次render时，都会重新创建一个新的匿名方法对象，组件越底层，开销就越大。( 当然，在大多数情况下，这点性能损耗不必在意 )

```javascript
// 使用bind绑定this指向( 还是和使用箭头函数的问题一样 )
// 不过这种方式在需要为处理函数传递额外参数时，就有了用武之地了
<button onClick={this.handleClick.bind(this, event)}></button>

// 直接将组件内的方法在constructor内赋值給组件的普通属性
constructor(props) {
  super(props);
  // this.handleClick = ::this.handleClick;
  this.handleClick = this.handleClick.bind(this);
}
<button onClick={this.handleClick}}></button>
```
> 在constructor复制給元素的事件属性好处是：每次render时，就不会重新创建回调函数，没有额外性能开销，不过这种重复性的代码还是稍显繁琐

```javascript
// 箭头函数( 使用ES7的 property initializers会自动为class中定义的方法绑定this )
handleClick = event => {
  console.log(this.state.num)
}
<button onClick={this.handleClick}}></button>
```
> 这种方式既不需要在构造函数中手动绑定this，也不需要单选组件的重复渲染问题导致的函数重复创建问题

#### 受控和非受控组件
> input元素会根据用户的输入自动改变显示的内容，而不是从组件的状态中获取显示的内容。这类状态不受React控制的表单元素，称之为非受控组件。在React中，状态的修改必须通过组件的state。为了保证组件的state是界面上所有元素状态的唯一来源，React采用受控组件达到这一目的

> 对于不同的表单元素，React的控制方式略有不同

####### **文本框**
1. 通过value设置input显示内容 -> 通过onChange监听value值变化 -> 触发事件 -> 去setState
####### **下拉列表**
1. 元素select列表中，可以通过定义option的selected属性来确定哪一个处于选中状态，在React中，只需要在select上定义value属性就可以决定哪一个option处于选中状态。 - 只需要修改select的value值即可，不需要关注option
```javascript
state = {
  value: 'mobx'
}
handlChange = event => {
  this.setState({value: event.target.value})
}
<select value={this.state.value} onChange={this.handlChange}>
  <option value="react">react</option>
  <option value="redux">redux</option>
  <option value="mobx">mobx</option>
</selet>
```
####### **复选框和单选框**
> 通常checkbox和radio的值是不变的，需要改变的是它们的checked状态，因此React控制的属性不在是value属性，而是checked属性
```javascript
state = {
  react: false,
  redux: false,
  mobx: false
};
handleChange = event => {
  this.setState({ [event.target.name]: event.target.checked });
}
<input 
  type="checkbox"
  name="react"
  value="react"
  checked={this.state.react}
  onChange={this.handleChange}
/>
```

####### **非受控组件**
1. 受控组件保证了表单的状态有React统一管理，但它需要onChange事件处理，然后把表单元素同步到state。这一过程比较繁琐。另一种替代方案是是用费受控组件：表单元素依然由表单元素自己管理，而不是交给React组件管理，使用ref获取DOM元素，通过defaultValue设置input\slect\textarta元素的默认值，checkbox和radio通过defaultChecked设置默认值
2. 虽然说非受控组件简化了表单操作的过程，但是这种方式破坏了React对组件的统一管理的一致性，会出现不已排查的Bug，一次非特殊清下，不推荐使用


#### React生命周期

## React 案例

- 基础版 todolist
- Redux 版 todolist
- 基础项目实战

倒计时
加减
todo

### React 核心

1.  JSX - Virtual DOM - DOM diffing
2.  setState 的异步
3.  Redux( 单向数据流 )
4.  路由
