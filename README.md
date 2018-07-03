> React学习路线

## React主要概念
#### JSX
- JSX：神似HTML的JavaScript
- 有别于HTML的地方：
| HTML | JSX |
| --- | --- |
| class | className |
| onclick | onClick |
| label标签中的for属性 | htmlFor |
| style="color: red" | style={{color: red}} |
|  | 在JSX中可以写JS，用{}包裹起来即可，但是不能使用多行JS语句( 但是你可以使用三目或者逻辑运算符 ) |

  JSX是React的特色之一：其他的框架或者在传统开发模式下，通常需要学习一门模板语言。来描述UI的一些变量或者函数等等。而JSX不是模板语言，其本身就是纯JavaScript。只是用我们熟悉的HTML来描述JavaScript。

  
#### 组件
> 1. 组件是React的核心概念，React的基石，组件是构造软件的“零部件”
> 2. React可以理解为一个纯函数，输入什么，得到的就是什么
> 3. React是单向数据传递的，外面传递数据进入，一定是通过props的，外界要知道内部发生了什么变化，一定是内部暴露了一个事件
> 4. React组件分为两种：函数组件、类组件

#### props和state
> 1. React组件正是由props和state两种类型的数据驱动渲染出组件的UI
> 2. React组件的数据分为两种(props和state)：props是组件对外的接口，state是组件对内的接口
> 3. props是只读的，不能在组件内部修改props
> 4. state是可变的，组件状态变更通过调用this.setState({})方法来改变
> 5. 组件state必须能代表一个UI呈现的完整状态，即组件的任何UI改变都可以从state的变化中反应出来，
> 6. state中所有状态都应该用于反映组件UI的变化，不应该存在通过其他状态计算而来的中间状态
> 例如：发送一个Ajax请求，在数据回来之前你应该在页面使用Loading，没有必要为Loading单独添加一个状态进行管理，可以根据其他的状态数据得到
> 7. 能计算得到的状态就去计算它，就不要在state中单独存储
> 8. 组件经量无状态，所需数据通过props传递( 纯组件 )
**props**
> - React组件的props：除了支持字符串类型，可以是任何一种JavaScript语言支持的数据类型
> - 组件绝对不要去修改传入的props值，严格来说，React并没有办法阻止你去修改传入的props对象，这是一条红线，不然可能会遇到不可预料的Bug
> - React组件要反馈数据給外界，可以通过props，因为props的类型不限于纯数据，也可以是函数，函数类型的props就相当于让父组件給子组件一个回调函数，子组件在合适的时机调用函数，带上必要的参数，这样就可以把信息传递給外部组件
> - 如果组件需要定义自己的构造函数，一定要在构造函数第一行通过super调用父类的React.Component的构造函数，如果在构造函数中没有调用super(props)，那么组件实例被构造之后，所有成员的函数都无法通过this.props访问到父组件传递过来的props值
> - propTypes类型检查：一个辅助开发的功能，并不会改变组件的行为，产生的错误信息只有开发者才能看得懂。只用在开发环境，最终部署到生产环境，用babel-react-optimize自动去掉propType即可
**设计一个合适的state**
> - 组件中用到的变量是不是应该作为state属性：
> 1. 这个变量是否在组件的render方法中使用？如果不是，那么它不应该写在state中，更为合适的是普通属性
> 2. 这个变量是否可以通过其他状态(state)或者属性(props)计算得到？如果是，那么它不应该写在state中，计算得到更为合适
> 3. 这个变量是否在组件的整个生命周期中都保持不变？如果是，那么它不应该写在state中，更为合适的是普通属性
> 4. 如果是通过props获取的，那么就不是一个state，请用this.props获取

**state不可变对象**
> 1. React官方建议把state当做不可变对象，当state中的某个状态发生改变时，应该重新创建这个状态对象。而不是直接修改原来的状态
> 2. 如果是数字、字符串、布尔，直接給要修改的状态赋值即可
> 3. 如果状态是数组：不要使用(push、pop、shift、unshift、splice等方法)，因为这些方法都在原数组的基础上修改的。如果要增加请使用( concat\ES6展开语法 )，截取使用slice方法，过来数据使用filter方法
```javascript
this.setState((preState) => {
  newState: [...preState, 'React state']
});
this.setState((preState) => {
  newState: [...preState, 'React state']
  newState: preState.state.concat(['React state']);
});
```
> 4. 如果状态是普通对象：使用ES6的Object.assgin和对象扩展语法 `{...preState, name:'ntscshen'}`，创建新的对象经量避免使用会直接修改原对象的方法，而是使用可以返回一个新对象的方法，为了少出Bug，为了性能(在shouldComponentUpdate生命周期中只要比较前后两次状态对象的应用就可以判断state是否真的发送改变)

slice(包头不包尾)：返回一个含有提取元素的新数组
filter()：创建一个新数组，返回一个新的通过测试的元素集合的数组，如果没有通过则返回空数组



#### 有状态组件和无状态组件
> 1. 无状态组件：不是每个组件都需要定义state，如果一个组件的内部状态是不变的，就用不到state，这样的组件称之为无状态组件
> 2. 有状态组件：一个组价内部状态会发生变化，就需要使用state来存储变化，这样的组件称之为有状态组件
> 3. 原则：应该尽可能多的使用无状态组件，并且在使用无状态组件时，应该尽可能多的将其定义为函数组件( 简洁的多 )，无状态组件不用关心状态的具体变化，只聚焦于UI的展示，更容易被复用。
> 4. React的组件设计思路：通过定义少量的有状态组件管理整个应用的变化，并且将状态通过props传递給其余的无状态组件，由无状态组件完成页面绝大多数的UI渲染工作。
> 5. 有状态组件：聚焦于处理状态变化的业务逻辑
> 6. 无状态组件：聚焦于组件的UI渲染
> 7. 当在设计组件时发现一个功能有太多的代码时，就要考虑进行组件拆分，用多个小组件来代替，每个小组件聚焦于单个功能的实现( 零部件 )，最后进行组装，满足复杂业务需求
> 8. 组件如何拆分：拆分组件最关键的是确定组件的 "边界"，每个组件都应该是可以独立存在的，如果两个组件逻辑太紧密，无法清晰定义各自的职责，那么也许这两个组件本身就不应该被拆分，作为同一个组件也许更为合理。


- 事件监听
- 受控和非受控组件

## React案例
- 基础版todolist
- Redux版todolist
- 基础项目实战

倒计时
加减
todo

### React核心
1. JSX - Virtual DOM - DOM diffing
2. setState的异步
3. Redux( 单向数据流 )
4. 路由

