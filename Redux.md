## 一. Redux介绍



###### react全家桶：

**react**（整体架构） +  **redux || mobx**（状态管理） +  **react-router**（路由） +  **axios**（ajax请求） +  **antd || react-material || antd-model**(UI框架库)

​                       

Redux是一个用来管理数据状态和UI状态的JavaScript应用工具。随着JavaScript单页应用（SPA）开发日趋复杂，JavaScript需要管理比任何时候都要多的state（状态），Redux就是降低管理难度的。（Redux支持React，Angular、jQuery甚至纯JavaScript）如果不用Redux，我们要传递state是非常麻烦的。而在Redux中，可以把数据先放在数据仓库（store-公用状态存储空间）中，这里可以统一管理状态，然后哪个组件用到了，就去stroe中查找状态。

##### Flux和Redux的关系

Redux是Flux的升级版本，早期使用React都要配合Flux进行状态管理，但是在使用中，Flux显露了很多弊端，比如多状态管理的复杂和易错，所以Redux就诞生了，现在已经完全取代了Flux。

## 二. Redux工作流程

![Redux Flow](https://img-blog.csdnimg.cn/20191202110233889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lleHVkZW5nemhpZGFv,size_16,color_FFFFFF,t_70)

## 三. Ant Design介绍和环境初始化

`Ant Design`是一套面向企业级开发的UI框架，视觉和动效做的很好。阿里开源的一套UI框架，它不只支持React，还有ng和Vue的版本，我认为不论你的前端框架用什么，Ant Design都是一个不错的选择。习惯性把AntDesign简称为antd。 目前有将近5万Star，算是React UI框架中的老大了。

[官网为:https://ant.design/index-cn](https://ant.design/index-cn)

##### 3.1 安装AntDesign

这里使用`npm`来进行安装，当然你有可以用`yarn`的方式进行安装。

```javascript
npm install antd --save
```

yarn的安装方式是:

```javascrpt
yarn add antd
```

## 四. 用Ant Design制作UI界面

##### 4.1 引入CSS样式

在使用`Ant Design`时，第一件事就是先引入CSS样式，有了样式才可以让UI组件显示正常。可以直接在/src/TodoList.js文件中直接用import引入。

```javascript
import 'antd/dist/antd.css'
```

##### 4.2 编写Input框

引入CSS样式之后，可以快乐的使用antd里的``框了，在使用的时候，你需要先引入`Input`组件。全部代码如下:

```jsx
import React, { Component } from 'react';
import 'antd/dist/antd.css'
import {Input, Button, List} from 'antd'
class TodoList extends Component {
    constructor(props) {
        super(props)
        this.state = {
            'content': ['aaaaa', 'bbbbb', 'ccccc', 'dddddd']
        }
    }
    render() { 
        return (
            <div style={{margin: 10}}>
                <div>
                    <Input
                        placeholder='Write Some things'
                        style={{width: 200}}
                    />
                    <Button type='primary'>增加</Button>
                </div>
                <div style={{margin: 10}}>
                    <List
                        bordered
                        dataSource={this.state.content}
                        renderItem={item => <List.Item>{item}</List.Item>}
                    >
                    </List>
                </div>
            </div>
        );
    }
}
 
export default TodoList
```

## 一. 创建Redux中的仓库store和reducer

从`Redux的工作流图`中可以看出，Redux工作流程中有四个部分，最重要的就是`store`这个部分，因为它把所有的数据都放到了`store`中进行管理。在编写代码的时候，因为重要，所以要优先编写`store`。

##### 1.1 编写创建store仓库

在使用`Redux`之前，需要先用`npm install`来进行安装，打开终端，并进入到项目目录，然后输入。（如果你之前安装过了，就不用再次安装了）

```javascript
npm install --save redux
```

安装好`redux`之后，在src目录下创建一个`store文件夹`，然后在文件夹下创建一个`index.js`文件。
`index.js`就是整个项目的`store`文件，打开文件，编写下面的代码。

```javascript
import { createStore } from 'redux'  // 引入createStore方法
const store = createStore()          // 创建数据存储仓库
export default store                 //暴露出去
```

这样虽然已经建立好了仓库。

##### 1.2 创建仓库管理者reducer

但是这个仓库很混乱，这时候就需要一个`有管理能力的模块`出现，这就是`Reducers`。这两个一定要一起创建出来，这样仓库才不会出现互怼现象。在store文件夹下，新建一个文件`reducer.js`，然后写入下面的代码。

```javascript
const defaultState = {}  //默认数据
export default (state = defaultState,action)=>{  //就是一个方法函数
    return state
}
```

* state: 是整个项目中需要管理的数据信息，这里我们没有什么数据，所以用空对象来表示。

这样`reducer`就建立好了，把reducer引入到store中，再创建store时，以参数的形式传递给store。

```javascript
import { createStore } from 'redux'  //  引入createStore方法
import reducer from './reducer'    
const store = createStore(reducer) // 创建数据存储仓库
export default store   //暴露出去
```

##### 1.3 组件获得store中的数据

有了`store仓库`，也有了`数据`，那如何获得stroe中的数据那？你可以在要使用的组件中，先引入store。 我们todoList组件要使用store，就在`src/TodoList.js`文件夹中，进行引入。这时候可以删除以前写的data数据了。

```javascript
import store from './store/index'
```

当然你也可以简写成这样:

```javascript
import store from './store'
```

引入`store`后可以试着在构造方法里打印到控制台一下，看看是否真正获得了数据，如果一切正常，是完全可以获取数据的。

```javascript
constructor(props){
    super(props)
    console.log(store.getState())
}
```

这时候数据还不能在UI层让组件直接使用，我们可以直接复制给组件的state，代码如下(我这里为了方便学习，给出全部代码了)。

```javascript
import React, { Component } from 'react';
import 'antd/dist/antd.css'
import {Input, Button, List} from 'antd'
import store from './store'
class TodoList extends Component {
    constructor(props) {
        super(props)
        this.state = store.getState()
    }
    render() { 
        return (
            <div style={{margin: 10}}>
                <div>
                    <Input
                        placeholder='Write Some things'
                        style={{width: 200}}
                    />
                    <Button type='primary'>增加</Button>
                </div>
                <div style={{margin: 10}}>
                    <List
                        bordered
                        dataSource={this.state.content}
                        renderItem={item => <List.Item>{item}</List.Item>}
                    >
                    </List>
                </div>
            </div>
        );
    }
}
 
export default TodoList
```

通过上面的步骤，我们从仓库里取出了数据，并用在了组件的UI界面上，也算是一个小小的进步了。 这节课我们讲了如何创建store，reduce和如何使用store中的数据。这节课的知识也算redux思想的一个精髓，也非常重要。

## 二. Redux Dev Tools的安装

通过`Redux DevTools`我们可以在控制台调试这些仓库里的数据。

##### 2.1 安装Redux Devtools

在科学上网的环境下，去Chrome商店安装即可。

##### 2.2 配置Redux Devtools

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191129155938836.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lleHVkZW5nemhpZGFv,size_16,color_FFFFFF,t_70)
在createStore()参数里假如如下代码：

```javascript
import {createStore} from 'redux'
import reducer from './reducer'
const store = createStore(
        reducer,
        window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
        )
export default store
```

这样，在redux调试工具里就可以正常使用了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191129160314722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lleHVkZW5nemhpZGFv,size_16,color_FFFFFF,t_70)

## 三. 通过Input体验Redux的流程

我们通过`Input`的改变，体验一下`Redux`的整体流程，来看看整体是如何编写代码的。我们要实现的是在TodoList的Demo中，`只要文本框中的值改变就redux中store的值就跟着改变，并且随着Redux中的state值改变，组件也跟着改变。`

##### 3.1 增加Input响应事件

如果想`Input`改变，`redux`也跟着改变，需要在`Input`组件上增加onChange响应事件， 打开src目录下的ToDolist.js文件，修改具体代码如下：

```javascript
<Input 
    placeholder={this.state.inputValue} 
    style={{ width:'250px', marginRight:'10px'}}
    //---------关键代码----start
    onChange={this.changeInputValue}
    //---------关键代码----end
/>
```

写完这一步，还要记得在constructor进行this的绑定，修改this的指向。

```javascript
constructor(props){
    super(props)
    this.state=store.getState();
    this.changeInputValue= this.changeInputValue.bind(this)
}
```

这步完成后，就可以编写changeInputValue方法的代码了。我们先在控制台打印出文本框的变化，代码如下：

```javascript
changeInputValue(e){
    console.log(e.target.value)
}
```

然后打开浏览器，按F12看一下控制台的结果。这里给出目前的全部代码:

```javascript
import React, { Component } from 'react';
import 'antd/dist/antd.css'
import {Input, Button, List} from 'antd'
import store from './store'
class TodoList extends Component {
    constructor(props) {
        super(props)
        this.state = store.getState()
        this.changeInputVal = this.changeInputVal.bind(this)
        
    }
    render() { 
        return (
            <div style={{margin: 10}}>
                <div>
                    <Input
                        placeholder='Write Some things'
                        style={{width: 200}}
                        onChange={this.changeInputVal}
                        ref={(input) => {this.input = input}}
                        value={this.state.inputVal}
                    />
                    <Button type='primary' onClick={this.addList}>增加</Button>
                </div>
                <div style={{margin: 10}}>
                    <List
                        bordered
                        dataSource={this.state.content}
                        renderItem={item => <List.Item>{item}</List.Item>}
                    >
                    </List>
                </div>
            </div>
        );
    }
    changeInputVal(e) {
        // console.log(this.input)
        const content = {
            type: 'inputVal',
            value: e.target.value
        }
        store.dispatch(content)
        console.log(this.input.state.value)
        // console.log(e.target.value)
        
    }
    addList = () => {
        // console.log(this.input.state.value)
    }

}
 
export default TodoList
```

###### 下面需要作的事就是改变Redux里的值了，我们继续向下学习。

##### 3.2 创建Action

想改变`Redux`里边`State`的值就要创建Action了。Action就是一个对象，这个对象一般有两个属性，第一个是对`Action的描述`，第二个是`要改变的值`。

```javascript
changeInputValue(e){
    const action ={
        type:'change_input_value',
        value:e.target.value
    }
}
```

这样，action就创建好了，但是要通过dispatch()方法传递给store。我们在action下面再加入一句代码。

```javascript
changeInputValue(e){
    const action ={
        type:'changeInput',
        value:e.target.value
    }
    store.dispatch(action)
}
```

这是Action就已经完全创建完成了，也和store有了联系。

##### 3.3 store的自动推送策略

由于`store`只是一个仓库，它并没有管理能力，它会把接收到的`action`自动转发给Reducer。我们现在先直接在Reducer中打印出结果看一下。打开store文件夹下面的reducer.js文件，修改代码。

```javascript
export default (state = defaultState,action)=>{
    console.log(state,action)
    return state
}
```

讲到这里，就可以解释一下两个参数了：

* state: 指的是原始仓库里的状态。
* action: 指的是action新传递的状态。

通过打印我们可以知道，`Reducer已经拿到了原来的数据和新传递过来的数据`，现在要做的就是改变store里的值。**我们先判断type是不是正确的，如果正确，我们需要从新声明一个变量newState**。`（记住：Reducer里只能接收state，不能改变state。）`,所以我们声明了一个新变量，然后再次用return返回回去。

```javascript
const defaultState = {
    'content': ['aaaaa', 'bbbbb', 'ccccc', 'dddddd'],
    'inputVal': ''
}
export default (state = defaultState, action)=>{
    if (action.type === 'inputVal') {
        console.log('改变了store里的值')
        let newState = JSON.parse(JSON.stringify(state))
        newState.inputVal = action.value
        return newState
    }
    return state
}
```

##### 3.4 让组件发生更新

现在`store`里的数据已经更新了，但是`组件还没有进行更新`，我们需要打开组件文件TodoList.js，在`constructor`里，写入下面的代码。

```javascript
constructor(props){
    super(props)
    this.state=store.getState();
    this.changeInputValue= this.changeInputValue.bind(this)
    //----------关键代码-----------start
    this.storeChange = this.storeChange.bind(this)  //转变this指向
    store.subscribe(this.storeChange) //订阅Redux的状态
    //----------关键代码-----------end
}
```

当然我们现在还没有这个`storeChange`方法，只要写一下这个方法，并且重新`setState`一次就可以实现组件也是变化的。在代码的最下方，编写storeChange方法。

```javascript
storeChange(){
     this.setState(store.getState())
 }
```

##### [整体代码见GitHub：https://github.com/1020196987/ReduxDemo/releases/tag/v1.0](https://github.com/1020196987/ReduxDemo/releases/tag/v1.0)

## 一. 工作中写Redux的小技巧-1

##### 1.1 把Action Types 单度写入一个文件

写`Redux Action`的时候，我们写了很多`Action`的派发，产生了很多`Action Types`，如果需要`Action`的地方我们就自己命名一个`type`,会出现两个基本问题：

* 这些Types如果不统一管理，不利于大型项目的使用。
* 因为Action里的type，一定要和reducer里的type一一对应在，所以这部分代码或字母写错后，浏览器里并没有明确的报错，这给调试带来了极大的困难。

那我司中会把Action Type单独拆分出一个文件。在src/store文件夹下面，新建立一个`actionTypes.js`文件，然后把Type集中放到文件中进行管理。

```javascript
export const  CHANGE_INPUT = 'changeInput'
export const  ADD_ITEM = 'addItem'
export const  DELETE_ITEM = 'deleteItem'
```

##### 1.2 引入ationType.js并使用

写好了`ationType.js`文件，可以引入到`TodoList.js`组件当中，引入代码如下：

```javascript
import { CHANGE_INPUT , ADD_ITEM , DELETE_ITEM } from './store/actionTypes'
```

引入后可以在下面的代码中进行使用这些常量代替原来的`type`值了。

```javascript
changeInputValue(e){
    const action ={
        type:CHANGE_INPUT,
        value:e.target.value
    }
    store.dispatch(action)
}
clickBtn(){
    const action = { type:ADD_ITEM }
    store.dispatch(action)
}
deleteItem(index){
    const action = {  type:DELETE_ITEM, index}
    store.dispatch(action)
}
```

同理，在reducer.js文件中引入actionType.js文件，然后把对应的字符串换成常量，整个代码如下：

```javascript
import {CHANGE_INPUT,ADD_ITEM,DELETE_ITEM} from './actionTypes'

const defaultState = {
    inputValue : 'Write Something',
    list:[
        '早上4点起床，锻炼身体',
        '中午下班游泳一小时'
    ]
}
export default (state = defaultState,action)=>{
    if(action.type === CHANGE_INPUT){
        let newState = JSON.parse(JSON.stringify(state)) //深度拷贝state
        newState.inputValue = action.value
        return newState
    }
    //state值只能传递，不能使用
    if(action.type === ADD_ITEM ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list.push(newState.inputValue)  //push新的内容到列表中去
        newState.inputValue = ''
        return newState
    }
    if(action.type === DELETE_ITEM ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list.splice(action.index,1)  //push新的内容到列表中去
        return newState
    }
    return state
}
```

## 二. 工作中写Redux的小技巧-2

目前`ToDoList`组件里有很多`Action`，并且分散在程序的各个地方，如果工程庞大，这势必会造成严重的混乱，所以需要把Redux Action放到一个文件里进行管理。

##### 2.1 编写actionCreators.js文件

在`/src/store`文件夹下面，建立一个新的文件`actionCreators.js`，先在文件中引入`actionTypes.js`文件。

```javascript
import {CHANGE_INPUT}  from './actionTypes'
```

引入后可以用const声明一个changeInputAction变量，变量是一个箭头函数，代码如下：

```javascript
import {CHANGE_INPUT}  from './actionTypes'

export const changeInputAction = (value)=>({
    type:CHANGE_INPUT,
    value
})
```

##### 2.2 修改todoList中的代码

有了文件，就可以把actionCreatores.js引入到TodoLisit中。

```javascript
import {changeInputAction} from './store/actionCreatores'
```

引入后，可以把`changeInputValue()`方法，修改为下面的样子。

```javascript
changeInputValue(e){
        const action = changeInputAction(e.target.value)
        store.dispatch(action)
    }
```

然后再浏览器中打开程序，进行测试，也是完全正常的。

这个文件写完，可以把TodoList.js文件里的所有action都改为直接调用方法的模式。代码如下：

```javascript
import React, { Component } from 'react';
import 'antd/dist/antd.css'
import { Input , Button , List } from 'antd'
import store from './store'
//关键代码-------------start
import {changeInputAction , addItemAction ,deleteItemAction} from './store/actionCreatores'
//关键代码------------end

class TodoList extends Component {
constructor(props){
    super(props)
    this.state=store.getState();
    this.changeInputValue= this.changeInputValue.bind(this)
    this.storeChange = this.storeChange.bind(this)
    this.clickBtn = this.clickBtn.bind(this)
    store.subscribe(this.storeChange) //订阅Redux的状态
}
    render() { 
        return ( 
            <div style={{margin:'10px'}}>
                <div>
                    <Input 
                        placeholder={this.state.inputValue} 
                        style={{ width:'250px', marginRight:'10px'}}
                        onChange={this.changeInputValue}
                        value={this.state.inputValue}
                    />
                    <Button 
                        type="primary"
                        onClick={this.clickBtn}
                    >增加</Button>
                </div>
                <div style={{margin:'10px',width:'300px'}}>
                    <List
                        bordered
                        dataSource={this.state.list}
                        renderItem={(item,index)=>(<List.Item onClick={this.deleteItem.bind(this,index)}>{item}</List.Item>)}
                    />    
                </div>
            </div>
         );
    }
    storeChange(){
        console.log('store changed')
        this.setState(store.getState())
    }
    //--------关键代码------start
    changeInputValue(e){
        const action = changeInputAction(e.target.value)
        store.dispatch(action)
    }
    clickBtn(){
        const action = addItemAction()
        store.dispatch(action)
    }
    deleteItem(index){
        const action = deleteItemAction(index)
        store.dispatch(action)
    }
    //--------关键代码------end
}
export default TodoList;
```

都写完了，我们就可以到浏览器中进行查看了，功能也是完全可以的。这样我们实现Redux Action和业务逻辑的分离，我觉的这一步在你的实际工作中是完全由必要作的。这样可大大提供程序的可维护性。

## 三. Redux的三个小坑

* `store`必须是唯一的，多个`store`是坚决不允许，只能有一个`store`空间
* 只有store能改变自己的内容，Reducer不能改变
* Reducer必须是纯函数

##### 3.1 store必须是唯一的

现在看`TodoList.js`的代码，就可以看到，这里有一个`/store/index.js`文件，只在这个文件中用`createStore()`方法，声明了一个`store`，之后整个应用都在使用这个store。 下面我给出了index.js内容，可以帮助你更好的回顾。

```javascript
import { createStore } from 'redux'  //  引入createStore方法
import reducer from './reducer'    
const store = createStore(
    reducer,
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()) // 创建数据存储仓库
export default store   //暴露出去
```

##### 3.2 只有store能改变自己的内容，Reducer不能改变

`Reudcer`只是返回了更改的数据，但是并没有更改`store`中的数据，`store`拿到了`Reducer`的数据，自己对自己进行了更新。

```javascript
import {CHANGE_INPUT,ADD_ITEM,DELETE_ITEM} from './actionTypes'

const defaultState = {
    inputValue : 'Write Something',
    list:[
        '早上4点起床，锻炼身体',
        '中午下班游泳一小时'
    ]
}
export default (state = defaultState,action)=>{
    if(action.type === CHANGE_INPUT){
        let newState = JSON.parse(JSON.stringify(state)) //深度拷贝state
        newState.inputValue = action.value
        return newState
    }
    //state值只能传递，不能使用
    if(action.type === ADD_ITEM ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list.push(newState.inputValue)  //push新的内容到列表中去
        newState.inputValue = ''
        return newState
    }
    if(action.type === DELETE_ITEM ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list.splice(action.index,1)  //push新的内容到列表中去
        return newState
    }
    return state
}
```

##### 3.3 Reducer必须是纯函数

先来看什么是纯函数，纯函数定义：

> 如果函数的调用参数相同，则永远返回相同的结果。它不依赖于程序执行期间函数外部任何状态或数据的变化，必须只依赖于其输入参数。

其实你可以简单的理解为返回的结果是由传入的值决定的，而不是其它的东西决定的。比如下面这段Reducer代码。

```javascript
export default (state = defaultState,action)=>{
    if(action.type === CHANGE_INPUT){
        let newState = JSON.parse(JSON.stringify(state)) //深度拷贝state
        newState.inputValue = action.value
        return newState
    }
    //state值只能传递，不能使用
    if(action.type === ADD_ITEM ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list.push(newState.inputValue)  //push新的内容到列表中去
        newState.inputValue = ''
        return newState
    }
    if(action.type === DELETE_ITEM ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list.splice(action.index,1)  //push新的内容到列表中去
        return newState
    }
    return state
}
```

它的返回结果，是完全由传入的参数`state`和`action`决定的，这就是一个纯函数。

这个在实际工作中是如何犯错的？比如在Reducer里增加一个异步ajax函数，获取一些后端接口数据，然后再返回，这就是不允许的（包括你使用日期函数也是不允许的），因为违反了调用参数相同，返回相同的纯函数规则。

## 一. Redux-thunk中间件的安装和配置

`Redux-thunk`是`Redux`最常用的插件。那什么时候会用到这个插件呢？比如在`Dispatch`一个`Action`之后，到达`reducer`之前，进行一些额外的操作，就需要用到`middleware（中间件）`。在实际工作中你可以使用中间件来进行`日志记录`、`创建崩溃报告`，`调用异步接口`或者`路由`。 这个中间件可以使用是Redux-thunk来进行增强(当然你也可以使用其它的)，它就是对Redux中dispatch的加强。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019120217273864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lleHVkZW5nemhpZGFv,size_16,color_FFFFFF,t_70)

##### 1.1 安装Redux-thunk组件

`Redux-thunk`并不在`Redux`基础组件中，也就是说需要进行新安装。安装使用npm就可以了。

```javascript
npm install --save redux-thunk
```

##### 1.2 配置Redux-thunk组件

需要在创建store的地方引入`redux-thunk`，对于我们的目录来说，就是`/store/index.js`文件。

1. 引入applyMiddleware,如果你要使用中间件，就必须在redux中引入applyMiddleware。

```javascript
import { createStore , applyMiddleware } from 'redux' 
```

1. 引入redux-thunk库

```javascript
import thunk from 'redux-thunk'
```

如果你按照官方文档来写，你直接把thunk放到createStore里的第二个参数就可以了，但以前我们配置了`Redux Dev Tools`，已经占用了第二个参数。

官方文档给的方法:

```javascript
const store = createStore(
    reducer,
    applyMiddleware(thunk)
) // 创建数据存储仓库
```

这样写是完全没有问题的，但是我们的Redux Dev Tools插件就不能使用了，如果想两个同时使用，需要使用`增强函数`。使用增加函数前需要先引入`compose`。

```javascript
import { createStore , applyMiddleware ,compose } from 'redux'
```

然后利用`compose`创造一个增强函数，就相当于建立了一个链式函数，代码如下:

```javascript
const composeEnhancers =   window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}):compose
```

有了增强函数后，就可以把thunk加入进来了，这样两个函数就都会执行了。

```javascript
const enhancer = composeEnhancers(applyMiddleware(thunk))
```

这时候直接在createStore函数中的第二个参数，使用这个enhancer变量就可以了，相当于两个函数都执行了。

```javascript
const store = createStore( reducer, enhancer) // 创建数据存储仓库
```

也许你对增加函数还不能完全理解，其实你完全把这个想成固定代码，直接使用就好，我在这里给出全部代码，方便你以后学习使用。

```javascript
import { createStore , applyMiddleware ,compose } from 'redux'  //  引入createStore方法
import reducer from './reducer'    
import thunk from 'redux-thunk'

const composeEnhancers =   window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}):compose

const enhancer = composeEnhancers(applyMiddleware(thunk))

const store = createStore( reducer, enhancer) // 创建数据存储仓库
export default store   //暴露出去
```

这样就算把Redux的中间件配置好了，可以运行项目，到浏览器看一下结果和看一下Redux Dev Tools插件了。

## 二. Redux-thunk的使用方法

我们把向后台请求数据的程序放到中间件中，这样就形成了一套完整的Redux流程，所有逻辑都是在Redux的内部完成的，这样看起来更完美，而且这样作自动化测试也会变动简单很多，所以工作中你还是要尽量按照这种写法来写。

##### 2.1 在actionCreators.js里编写业务逻辑

以前`actionCreators.js`都是定义好的`action`，根本没办法写业务逻辑，有了`Redux-thunk`之后，可以把`TodoList.js`中的`componentDidMount`业务逻辑放到这里来编写。也就是把向后台请求数据的代码放到`actionCreators.js`文件里。那我们需要引入`axios`,并写一个新的函数方法。（以前的`action是对象`，现在的`action可以是函数`了，这就是redux-thunk带来的好处）

```javascript
import axios from 'axios'
...something...
export const getTodoList = () =>{
    return ()=>{
        axios.get('https://www.easy-mock.com/mock/5cfcce489dc7c36bd6da2c99/xiaojiejie/getList').then((res)=>{
            const data = res.data
            console.log(data)
        })
    }
}
```

现在我们需要执行这个方法，并在控制台查看结果，这时候可以修改`TodoList.js`文件中的`componentDidMount`代码。

```javascript
//先引入getTodoList
import {getTodoList , changeInputAction , addItemAction ,deleteItemAction,getListAction} from './store/actionCreatores'
---something---
componentDidMount(){
    const action = getTodoList()
    store.dispatch(action)
}
```

然后我们到浏览器的控制台中查看一下，看看是不是已经得到了后端传给我们的数据，如果一切正常，应该是可以得到。得到之后，我们继续走以前的`Redux`流程就可以了。

```javascript
export const getTodoList = () =>{
    return (dispatch)=>{
        axios.get('https://www.easy-mock.com/mock/5cfcce489dc7c36bd6da2c99/xiaojiejie/getList').then((res)=>{
            const data = res.data
            const action = getListAction(data)
            dispatch(action)
            
        })
    }
}
```

`这个函数可以直接传递dispatch进去`，这是自动的，然后我们直接用`dispatch(action)`传递就好了。现在我们就可以打开浏览器进行测试了。

这时候还会有一些警告，主要是我们引入了并没有使用，我们按照警告的提示，删除没用的引入就可以了。

也许你会觉的这么写程序很绕，其实我刚开始写Redux的时候也会这么想，但是随着项目的越来越大，你会发现把共享state的业务逻辑放到你Redux提示当中是非常正确的， 它会使你的程序更加有条理。而`在自动化测试的时候`，`可以直接对一个方法进行测试`，`而对生命周期测试是困难的`。我目前接触的大公司都是要求这样写的，如果现在还不能理解里边的好处，也不用纠结，先按照这种形式进行编写。等你写过2至3个项目后，你就能理解这种写法的好处了。

## 二. React-Redux

`React-Redux`这是一个`React生态`中常用组件，它可以简化Redux流程（需要注意的是：React、Redux、React-redux是三个不同的东西）

##### 2.1 React-redux中的Provider和connect

###### 1. Provider 提供器

``是一个提供器，只要使用了这个组件，组件里边的其它所有组件都可以使用`store`了，这也是`React-redux`的核心组件了。有了``就可以把`/src/index.js`改写成下面的代码样式了

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import TodoList from './TodoList'
//---------关键代码--------start
import { Provider } from 'react-redux'
import store from './store'
//声明一个App组件，然后这个组件用Provider进行包裹。
const App = (
   <Provider store={store}>
       <TodoList />
   </Provider>
)
//---------关键代码--------end
ReactDOM.render(App, document.getElementById('root'));
```

写完这个，我们再去浏览器中进行查看，发现代码也是可以完美运行的。需要注意的是，现在还是用传统方法获取的store中的数据。有了Provider再获取数据就没有那么麻烦了。

###### 2. connect连接器的使用

现在如何简单的获取store中数据呢？先打开TodoList.js文件，引入connect，它是一个连接器（其实它就是一个方法），有了这个连接器就可以很容易的获得数据了。

```javascript
import {connect} from 'react-redux'  //引入连接器
```

这时候暴露出去的就变成了connect了，代码如下。

```javascript
export default connect(xxx,null)(TodoList);
```

###### 3. 映射关系的制作

映射关系就是把原来的`state`映射成组件中的`props属性`，比如我们想映射inputValue就可以写成如下代码。

```javascript
const stateToProps = (state)=>{
    return {
        inputValue : state.inputValue
    }
}
```

这时候再把xxx改为stateToProps

```javascript
export default connect(stateToProps,null)(TodoList)
```

然后把``里的state标签，改为props,代码如下:

```javascript
<input value={this.props.inputValue} />
```

为了方便你学习，我这里给出所有的TodoList.js的所有代码。

```javascript
import React, { Component } from 'react';
import store from './store'
import {connect} from 'react-redux'

class TodoList extends Component {
    constructor(props){
        super(props)
        this.state = store.getState()
    }
    render() { 
        return (
            <div>
                <div>
                    <input value={this.props.inputValue} />
                    <button>提交</button>
                </div>
                <ul>
                    <li>JSPang</li>
                </ul>
            </div>
            );
    }
}

const stateToProps = (state)=>{
    return {
        inputValue : state.inputValue
    }
}
 
export default connect(stateToProps,null)(TodoList);
```

写完之后再到浏览器中查看一下，发现我们映射的关系也是可以用的。







# redux-saga

`redux-saga` 是一个用于管理应用程序 Side Effect（副作用，例如异步获取数据，访问浏览器缓存等）的 library，它的目标是让副作用管理更容易，执行更高效，测试更简单，在处理故障时更容易。

可以想像为，一个 saga 就像是应用程序中一个单独的线程，它独自负责处理副作用。 `redux-saga` 是一个 redux 中间件，意味着这个线程可以通过正常的 redux action 从主应用程序启动，暂停和取消，它能访问完整的 redux state，也可以 dispatch redux action。

redux-saga 使用了 ES6 的 Generator 功能，让异步的流程更易于读取，写入和测试。 通过这样的方式，这些异步的流程看起来就像是标准同步的 Javascript 代码。

你可能已经用了 `redux-thunk` 来处理数据的读取。不同于 redux thunk，你不会再遇到回调地狱了，你可以很容易地测试异步流程并保持你的 action 是干净的。





## 基础

# Action

首先，让我们来给 action 下个定义。

**Action** 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的**唯一**来源。一般来说你会通过 [`store.dispatch()`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 将 action 传到 store。

添加新 todo 任务的 action 是这样的：

```js
const ADD_TODO = 'ADD_TODO'
{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

Action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。当应用规模越来越大时，建议使用单独的模块或文件来存放 action。

```js
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```

> ##### 样板文件使用提醒
>
> 使用单独的模块或文件来定义 action type 常量并不是必须的，甚至根本不需要定义。对于小应用来说，使用字符串做 action type 更方便些。不过，在大型应用中把它们显式地定义成常量还是利大于弊的。参照 [减少样板代码](https://www.redux.org.cn/docs/recipes/ReducingBoilerplate.html) 获取更多保持代码简洁的实践经验。

除了 `type` 字段外，action 对象的结构完全由你自己决定。参照 [Flux 标准 Action](https://github.com/acdlite/flux-standard-action) 获取关于如何构造 action 的建议。

这时，我们还需要再添加一个 action index 来表示用户完成任务的动作序列号。因为数据是存放在数组中的，所以我们通过下标 `index` 来引用特定的任务。而实际项目中一般会在新建数据的时候生成唯一的 ID 作为数据的引用标识。

```js
{
  type: TOGGLE_TODO,
  index: 5
}
```

**我们应该尽量减少在 action 中传递的数据**。比如上面的例子，传递 `index` 就比把整个任务对象传过去要好。

最后，再添加一个 action type 来表示当前的任务展示选项。

```js
{
  type: SET_VISIBILITY_FILTER,
  filter: SHOW_COMPLETED
}
```

## Action 创建函数

**Action 创建函数** 就是生成 action 的方法。“action” 和 “action 创建函数” 这两个概念很容易混在一起，使用时最好注意区分。

在 Redux 中的 action 创建函数只是简单的返回一个 action:

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

这样做将使 action 创建函数更容易被移植和测试。

在 [传统的 Flux](http://facebook.github.io/flux) 实现中，当调用 action 创建函数时，一般会触发一个 dispatch，像这样：

```js
function addTodoWithDispatch(text) {
  const action = {
    type: ADD_TODO,
    text
  }
  dispatch(action)
}
```

不同的是，Redux 中只需把 action 创建函数的结果传给 `dispatch()` 方法即可发起一次 dispatch 过程。

```js
dispatch(addTodo(text))
dispatch(completeTodo(index))
```

或者创建一个 **被绑定的 action 创建函数** 来自动 dispatch：

```js
const boundAddTodo = text => dispatch(addTodo(text))
const boundCompleteTodo = index => dispatch(completeTodo(index))
```

然后直接调用它们：

```
boundAddTodo(text);
boundCompleteTodo(index);
```

store 里能直接通过 [`store.dispatch()`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 调用 `dispatch()` 方法，但是多数情况下你会使用 [react-redux](http://github.com/gaearon/react-redux) 提供的 `connect()` 帮助器来调用。[`bindActionCreators()`](https://www.redux.org.cn/docs/api/bindActionCreators.html) 可以自动把多个 action 创建函数 绑定到 `dispatch()` 方法上。

Action 创建函数也可以是异步非纯函数。你可以通过阅读 [高级教程](https://www.redux.org.cn/docs/advanced/) 中的 [异步 action](https://www.redux.org.cn/docs/advanced/AsyncActions.html)章节，学习如何处理 AJAX 响应和如何把 action 创建函数组合进异步控制流。因为基础教程中包含了阅读高级教程和异步 action 章节所需要的一些重要基础概念, 所以请在移步异步 action 之前, 务必先完成基础教程。

## 源码

### `actions.js`

```js
/*
 * action 类型
 */

export const ADD_TODO = 'ADD_TODO';
export const TOGGLE_TODO = 'TOGGLE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

/*
 * 其它的常量
 */

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
}

/*
 * action 创建函数
 */

export function addTodo(text) {
  return { type: ADD_TODO, text }
}

export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}
```





# Reducer

**Reducers** 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的，记住 actions 只是描述了*有事情发生了*这一事实，并没有描述应用如何更新 state。











# 技巧

## 缩减样板代码

Redux 很大部分 [受到 Flux 的启发](https://www.redux.org.cn/docs/introduction/PriorArt.html)，而最常见的关于 Flux 的抱怨是必须写一大堆的样板代码。在这章中，我们将考虑 Redux 如何根据个人风格，团队偏好，长期可维护性等自由决定代码的繁复程度。

## Actions

Actions 是用来描述在 app 中发生了什么的普通对象，并且是描述突变数据意图的唯一途径。很重要的一点是 **不得不 dispatch 的 action 对象并非是一个样板代码，而是 Redux 的一个 [基本设计选择](https://www.redux.org.cn/docs/introduction/ThreePrinciples.html)**.

不少框架声称自己和 Flux 很像，只不过缺少了 action 对象的概念。在可预测性方面，这是从 Flux 或 Redux 的倒退。如果没有可序列化的普通对象 action，便无法记录或重演用户会话，也无法实现 [带有时间旅行的热重载](https://www.youtube.com/watch?v=xsSnOQynTHs)。如果你更喜欢直接修改数据，那你并不需要使用 Redux 。

Action 一般长这样:

```javascript
{ type: 'ADD_TODO', text: 'Use Redux' }
{ type: 'REMOVE_TODO', id: 42 }
{ type: 'LOAD_ARTICLE', response: { ... } }
```

一个约定俗成的做法是，action 拥有一个不变的 type 帮助 reducer (或 Flux 中的 Stores ) 识别它们。==我们建议你使用 string 而不是 [符号（Symbols）](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 作为 action type ，因为 string 是可序列化的，并且使用符号会使记录和重演变得困难。==

在 Flux 中，传统的想法是将每个 action type 定义为 string 常量：

```javascript
const ADD_TODO = 'ADD_TODO';
const REMOVE_TODO = 'REMOVE_TODO';
const LOAD_ARTICLE = 'LOAD_ARTICLE';
```

这么做的优势是什么？**人们通常声称常量不是必要的。对于小项目也许正确。** 对于大的项目，将 action types 定义为常量有如下好处：

> * 帮助维护命名一致性，因为所有的 action type 汇总在同一位置。
> * 有时，在开发一个新功能之前你想看到所有现存的 actions 。而你的团队里可能已经有人添加了你所需要的action，而你并不知道。
> * Action types 列表在 Pull Request 中能查到所有添加，删除，修改的记录。这能帮助团队中的所有人及时追踪新功能的范围与实现。
> * 如果你在 import 一个 Action 常量的时候拼写错了，你会得到 `undefined` 。在 dispatch 这个 action 的时候，Redux 会立即抛出这个错误，你也会马上发现错误。

你的项目约定取决与你自己。开始时，可能在刚开始用内联字符串（inline string），之后转为常量，也许再之后将他们归为一个独立文件。Redux 在这里没有任何建议，选择你自己最喜欢的。

## Action Creators

另一个约定俗成的做法是通过创建函数生成 action 对象，而不是在你 dispatch 的时候内联生成它们。

例如，不是使用对象字面量调用 `dispatch` ：

```javascript
// event handler 里的某处
dispatch({
  type: 'ADD_TODO',
  text: 'Use Redux'
});
```

你其实可以在单独的文件中写一个 action creator ，然后从 component 里 import：

#### `actionCreators.js`

```javascript
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  };
}
```

#### `AddTodo.js`

```javascript
import { addTodo } from './actionCreators';

// event handler 里的某处
dispatch(addTodo('Use Redux'))
```

Action creators 总被当作样板代码受到批评。好吧，其实你并不非得把他们写出来！**如果你觉得更适合你的项目，你可以选用对象字面量** 然而，你应该知道写 action creators 是存在某种优势的。

假设有个设计师看完我们的原型之后回来说，我们最多只允许三个 todo 。我们可以使用 [redux-thunk](https://github.com/gaearon/redux-thunk) 中间件，并添加一个提前退出，把我们的 action creator 重写成回调形式：

```javascript
function addTodoWithoutCheck(text) {
  return {
    type: 'ADD_TODO',
    text
  };
}

export function addTodo(text) {
  // Redux Thunk 中间件允许这种形式
  // 在下面的 “异步 Action Creators” 段落中有写
  return function (dispatch, getState) {
    if (getState().todos.length === 3) {
      // 提前退出
      return;
    }

    dispatch(addTodoWithoutCheck(text));
  }
}
```

我们刚修改了 `addTodo` action creator 的行为，使得它对调用它的代码完全不可见。**我们不用担心去每个添加 todo 的地方看一看，以确认他们有了这个检查** Action creator 让你可以解耦额外的分发 action 逻辑与实际发送这些 action 的 components 。当你有大量开发工作且需求经常变更的时候，这种方法十分简便易用。

### Action Creators 生成器

某些框架如 [Flummox](https://github.com/acdlite/flummox) 自动从 action creator 函数定义生成 action type 常量。这个想法是说你不需要同时定义 `ADD_TODO` 常量和 `addTodo()` action creator 。这样的方法在底层也生成了 action type 常量，但他们是隐式生成的、间接级，会造成混乱。因此我们建议直接清晰地创建 action type 常量。

写简单的 action creator 很容易让人厌烦，且往往最终生成多余的样板代码：

```javascript
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

export function editTodo(id, text) {
  return {
    type: 'EDIT_TODO',
    id,
    text
  }
}

export function removeTodo(id) {
  return {
    type: 'REMOVE_TODO',
    id
  }
}
```

你可以写一个用于生成 action creator 的函数：

```javascript
function makeActionCreator(type, ...argNames) {
  return function(...args) {
    let action = { type }
    argNames.forEach((arg, index) => {
      action[argNames[index]] = args[index]
    })
    return action
  }
}

const ADD_TODO = 'ADD_TODO'
const EDIT_TODO = 'EDIT_TODO'
const REMOVE_TODO = 'REMOVE_TODO'

export const addTodo = makeActionCreator(ADD_TODO, 'todo')
export const editTodo = makeActionCreator(EDIT_TODO, 'id', 'todo')
export const removeTodo = makeActionCreator(REMOVE_TODO, 'id')
```

一些工具库也可以帮助生成 action creator ，例如 [redux-act](https://github.com/pauldijou/redux-act) 和 [redux-actions](https://github.com/acdlite/redux-actions) 。这些库可以有效减少你的样板代码，并紧守例如 [Flux Standard Action (FSA)](https://github.com/acdlite/flux-standard-action) 一类的标准。

## 异步 Action Creators

[中间件](https://www.redux.org.cn/docs/Glossary.html#middleware) 让你在每个 action 对象 dispatch 出去之前，注入一个自定义的逻辑来解释你的 action 对象。异步 action 是中间件的最常见用例。

如果没有中间件，[`dispatch`](https://www.redux.org.cn/docs/api/Store.html#dispatch) 只能接收一个普通对象。因此我们必须在 components 里面进行 AJAX 调用：

#### `actionCreators.js`

```javascript
export function loadPostsSuccess(userId, response) {
  return {
    type: 'LOAD_POSTS_SUCCESS',
    userId,
    response
  };
}

export function loadPostsFailure(userId, error) {
  return {
    type: 'LOAD_POSTS_FAILURE',
    userId,
    error
  };
}

export function loadPostsRequest(userId) {
  return {
    type: 'LOAD_POSTS_REQUEST',
    userId
  };
}
```

#### `UserInfo.js`

```javascript
import { Component } from 'react';
import { connect } from 'react-redux';
import { loadPostsRequest, loadPostsSuccess, loadPostsFailure } from './actionCreators';

class Posts extends Component {
  loadData(userId) {
    // 调用 React Redux `connect()` 注入的 props ：
    let { dispatch, posts } = this.props;

    if (posts[userId]) {
      // 这里是被缓存的数据！啥也不做。
      return;
    }

    // Reducer 可以通过设置 `isFetching` 响应这个 action
    // 因此让我们显示一个 Spinner 控件。
    dispatch(loadPostsRequest(userId));

    // Reducer 可以通过填写 `users` 响应这些 actions
    fetch(`http://myapi.com/users/${userId}/posts`).then(
      response => dispatch(loadPostsSuccess(userId, response)),
      error => dispatch(loadPostsFailure(userId, error))
    );
  }

  componentDidMount() {
    this.loadData(this.props.userId);
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.userId !== this.props.userId) {
      this.loadData(nextProps.userId);
    }
  }

  render() {
    if (this.props.isLoading) {
      return <p>Loading...</p>;
    }

    let posts = this.props.posts.map(post =>
      <Post post={post} key={post.id} />
    );

    return <div>{posts}</div>;
  }
}

export default connect(state => ({
  posts: state.posts
}))(Posts);
```

然而，不久就需要再来一遍，因为不同的 components 从同样的 API 端点请求数据。而且我们想要在多个components 中重用一些逻辑（比如，当缓存数据有效的时候提前退出）。

**中间件让我们能写表达更清晰的、潜在的异步 action creators。** 它允许我们 dispatch 普通对象之外的东西，并且解释它们的值。比如，中间件能 “捕捉” 到已经 dispatch 的 Promises 并把他们变为一对请求和成功/失败的 action.

中间件最简单的例子是 [redux-thunk](https://github.com/gaearon/redux-thunk). **“Thunk” 中间件让你可以把 action creators 写成 “thunks”，也就是返回函数的函数。** 这使得控制被反转了： 你会像一个参数一样取得 `dispatch` ，所以你也能写一个多次分发的 action creator 。

> ##### 注意
>
> Thunk 只是一个中间件的例子。中间件不仅仅是关于 “分发函数” 的：而是关于你可以使用特定的中间件来分发任何该中间件可以处理的东西。例子中的 Thunk 中间件添加了一个特定的行为用来分发函数，但这实际取决于你用的中间件。

用 [redux-thunk](https://github.com/gaearon/redux-thunk) 重写上面的代码：

#### `actionCreators.js`

```javascript
export function loadPosts(userId) {
  // 用 thunk 中间件解释：
  return function (dispatch, getState) {
    let { posts } = getState();
    if (posts[userId]) {
      // 这里是数据缓存！啥也不做。
      return;
    }

    dispatch({
      type: 'LOAD_POSTS_REQUEST',
      userId
    });

    // 异步分发原味 action
    fetch(`http://myapi.com/users/${userId}/posts`).then(
      response => dispatch({
        type: 'LOAD_POSTS_SUCCESS',
        userId,
        response
      }),
      error => dispatch({
        type: 'LOAD_POSTS_FAILURE',
        userId,
        error
      })
    );
  }
}
```

#### `UserInfo.js`

```javascript
import { Component } from 'react';
import { connect } from 'react-redux';
import { loadPosts } from './actionCreators';

class Posts extends Component {
  componentDidMount() {
    this.props.dispatch(loadPosts(this.props.userId));
  }

  componentWillReceiveProps(nextProps) {
    if (nextProps.userId !== this.props.userId) {
      this.props.dispatch(loadPosts(nextProps.userId));
    }
  }

  render() {
    if (this.props.isLoading) {
      return <p>Loading...</p>;
    }

    let posts = this.props.posts.map(post =>
      <Post post={post} key={post.id} />
    );

    return <div>{posts}</div>;
  }
}

export default connect(state => ({
  posts: state.posts
}))(Posts);
```

这样打得字少多了！如果你喜欢，你还是可以保留 “原味” action creators 比如从一个容器 `loadPosts` action creator 里用到的 `loadPostsSuccess` 。

**最后，你可以编写你自己的中间件** 你可以把上面的模式泛化，然后代之以这样的异步 action creators ：

```js
export function loadPosts(userId) {
  return {
    // 要在之前和之后发送的 action types
    types: ['LOAD_POSTS_REQUEST', 'LOAD_POSTS_SUCCESS', 'LOAD_POSTS_FAILURE'],
    // 检查缓存 (可选):
    shouldCallAPI: (state) => !state.users[userId],
    // 进行取：
    callAPI: () => fetch(`http://myapi.com/users/${userId}/posts`),
    // 在 actions 的开始和结束注入的参数
    payload: { userId }
  };
}
```

解释这个 actions 的中间件可以像这样：

```javascript
function callAPIMiddleware({ dispatch, getState }) {
  return next => action => {
    const {
      types,
      callAPI,
      shouldCallAPI = () => true,
      payload = {}
    } = action

    if (!types) {
      // Normal action: pass it on
      return next(action)
    }

    if (
      !Array.isArray(types) ||
      types.length !== 3 ||
      !types.every(type => typeof type === 'string')
    ) {
      throw new Error('Expected an array of three string types.')
    }

    if (typeof callAPI !== 'function') {
      throw new Error('Expected callAPI to be a function.')
    }

    if (!shouldCallAPI(getState())) {
      return
    }

    const [ requestType, successType, failureType ] = types

    dispatch(Object.assign({}, payload, {
      type: requestType
    }))

    return callAPI().then(
      response => dispatch(Object.assign({}, payload, {
        response,
        type: successType
      })),
      error => dispatch(Object.assign({}, payload, {
        error,
        type: failureType
      }))
    )
  }
}
```

在传给 [`applyMiddleware(...middlewares)`](https://www.redux.org.cn/docs/api/applyMiddleware.html) 一次以后，你能用相同方式写你的 API 调用 action creators ：

```javascript
export function loadPosts(userId) {
  return {
    types: ['LOAD_POSTS_REQUEST', 'LOAD_POSTS_SUCCESS', 'LOAD_POSTS_FAILURE'],
    shouldCallAPI: (state) => !state.users[userId],
    callAPI: () => fetch(`http://myapi.com/users/${userId}/posts`),
    payload: { userId }
  };
}

export function loadComments(postId) {
  return {
    types: ['LOAD_COMMENTS_REQUEST', 'LOAD_COMMENTS_SUCCESS', 'LOAD_COMMENTS_FAILURE'],
    shouldCallAPI: (state) => !state.posts[postId],
    callAPI: () => fetch(`http://myapi.com/posts/${postId}/comments`),
    payload: { postId }
  };
}

export function addComment(postId, message) {
  return {
    types: ['ADD_COMMENT_REQUEST', 'ADD_COMMENT_SUCCESS', 'ADD_COMMENT_FAILURE'],
    callAPI: () => fetch(`http://myapi.com/posts/${postId}/comments`, {
      method: 'post',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ message })
    }),
    payload: { postId, message }
  };
}
```

## Reducers

Redux reducer 用函数描述逻辑更新减少了样板代码里大量的 Flux stores 。函数比对象简单，比类更简单得多。

这个 Flux store:

```javascript
let _todos = []

const TodoStore = Object.assign({}, EventEmitter.prototype, {
  getAll() {
    return _todos
  }
})

AppDispatcher.register(function (action) {
  switch (action.type) {
    case ActionTypes.ADD_TODO:
      let text = action.text.trim()
      _todos.push(text)
      TodoStore.emitChange()
  }
})

export default TodoStore
```

用了 Redux 之后，同样的逻辑更新可以被写成 reducing function：

```js
export function todos(state = [], action) {
  switch (action.type) {
  case ActionTypes.ADD_TODO:
    let text = action.text.trim()
    return [ ...state, text ]
  default:
    return state
  }
}
```

`switch` 语句 *不是* 真正的样板代码。真正的 Flux 样板代码是概念性的：发送更新的需求，用 Dispatcher 注册 Store 的需求，Store 是对象的需求 (当你想要一个哪都能跑的 App 的时候复杂度会提升)。

不幸的是很多人仍然靠文档里用没用 `switch` 来选择 Flux 框架。如果你不爱用 `switch` 你可以用一个单独的函数来解决，下面会演示。

### Reducers 生成器

写一个函数将 reducers 表达为 action types 到 handlers 的映射对象。例如，如果想在 `todos` reducer 里这样定义：

```javascript
export const todos = createReducer([], {
  [ActionTypes.ADD_TODO](state, action) {
    let text = action.text.trim();
    return [...state, text];
  }
})
```

我们可以编写下面的辅助函数来完成：

```javascript
function createReducer(initialState, handlers) {
  return function reducer(state = initialState, action) {
    if (handlers.hasOwnProperty(action.type)) {
      return handlers[action.type](state, action);
    } else {
      return state;
    }
  }
}
```

不难对吧？鉴于写法多种多样，Redux 没有默认提供这样的辅助函数。可能你想要自动地将普通 JS 对象变成 Immutable  对象，以填满服务器状态的对象数据。可能你想合并返回状态和当前状态。有多种多样的方法来 “获取所有”  handler，具体怎么做则取决于项目中你和你的团队的约定。

Redux reducer 的 API 是 `(state, action) => state`，但是怎么创建这些 reducers 由你来定。



# 计算衍生数据

[Reselect](https://github.com/faassen/reselect.git) 库可以创建可记忆的(Memoized)、可组合的 **selector** 函数。Reselect selectors 可以用来高效地计算 Redux store 里的衍生数据。

### 可记忆的 Selectors 初衷

首先访问 [Todos 列表示例](https://www.redux.org.cn/docs/basics/UsageWithReact.html):

#### `containers/VisibleTodoList.js`

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'

const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

上面的示例中，`mapStateToProps` 调用了 `getVisibleTodos` 来计算 `todos`。运行没问题，但有一个缺点：每当组件更新时都会重新计算 `todos`。如果 state tree 非常大，或者计算量非常大，每次更新都重新计算可能会带来性能问题。Reselect 能帮你省去这些没必要的重新计算。

### 创建可记忆的 Selector

我们需要一个可记忆的 selector 来替代这个 `getVisibleTodos`，只在 `state.todos` or `state.visibilityFilter` 变化时重新计算 `todos`，而在其它部分（非相关）变化时不做计算。

Reselect 提供 `createSelector` 函数来创建可记忆的 selector。`createSelector` 接收一个 input-selectors 数组和一个转换函数作为参数。如果 state tree 的改变会引起 input-selector  值变化，那么 selector 会调用转换函数，传入 input-selectors 作为参数，并返回结果。如果 input-selectors 的值和前一次的一样，它将会直接返回前一次计算的数据，而不会再调用一次转换函数。

定义一个可记忆的 selector `getVisibleTodos` 来替代上面的无记忆版本：

#### `selectors/index.js`

```js
import { createSelector } from 'reselect'

const getVisibilityFilter = (state) => state.visibilityFilter
const getTodos = (state) => state.todos

export const getVisibleTodos = createSelector(
  [ getVisibilityFilter, getTodos ],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

在上例中，`getVisibilityFilter` 和 `getTodos` 是 input-selector。因为他们并不转换数据，所以被创建成普通的非记忆的 selector 函数。但是，`getVisibleTodos` 是一个可记忆的 selector。他接收 `getVisibilityFilter` 和 `getTodos` 为 input-selector，还有一个转换函数来计算过滤的 todos 列表。

### 组合 Selector

可记忆的 selector 自身可以作为其它可记忆的 selector 的 input-selector。下面的 `getVisibleTodos` 被当作另一个 selector 的 input-selector，来进一步通过关键字（keyword）过滤 todos。

```js
const getKeyword = (state) => state.keyword

const getVisibleTodosFilteredByKeyword = createSelector(
  [ getVisibleTodos, getKeyword ],
  (visibleTodos, keyword) => visibleTodos.filter(
    todo => todo.text.indexOf(keyword) > -1
  )
)
```

### 连接 Selector 和 Redux Store

如果你在使用 React Redux，你可以在 `mapStateToProps()` 中当正常函数来调用 selectors

#### `containers/VisibleTodoList.js`

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state)
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

### 在 selectors 中访问 React Props

> 现在假使我们要支持一个新功能：支持多个 Todo 列表新功能。为了简洁起见，省略了实现这个工程会遇到的与本节不相关的内容（reducers 的变化、组件、Actions 等）

到目前为止，我们只看到 selector 接收 Redux store state 作为参数，然而，selector 也可以接收 props。

这儿有一个 `App` 的组件，它渲染了三个叫做 `VisibleTodoList` 的子组件，每个组件都带一个 `listId` 的 prop;

#### components/App.js

```js
import React from 'react'
import Footer from './Footer'
import AddTodo from '../containers/AddTodo'
import VisibleTodoList from '../containers/VisibleTodoList'

const App = () => (
  <div>
    <VisibleTodoList listId="1" />
    <VisibleTodoList listId="2" />
    <VisibleTodoList listId="3" />
  </div>
)
```

每个 `VisibleTodoList` 容器根据 `listId` props 的值选择不同的 state 切片，让我们修改 `getVisibilityFilter` 和 `getTodos` 来接收 props。

#### selectors/todoSelectors.js

```js
import { createSelector } from 'reselect'

const getVisibilityFilter = (state, props) =>
  state.todoLists[props.listId].visibilityFilter

const getTodos = (state, props) =>
  state.todoLists[props.listId].todos

const getVisibleTodos = createSelector(
  [ getVisibilityFilter, getTodos ],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_COMPLETED':
        return todos.filter(todo => todo.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(todo => !todo.completed)
      default:
        return todos
    }
  }
)

export default getVisibleTodos
```

`props` 可以通过 `mapStateToProps` 传递给 `getVisibleTodos`:

```js
const mapStateToProps = (state, props) => {
  return {
    todos: getVisibleTodos(state, props)
  }
}
```

现在，`getVisibleTodos` 可以访问 `props`，一切看上去都是如此的美好。

**但是这儿有一个问题！**

使用带有多个 `visibleTodoList` 容器实例的 `getVisibleTodos` selector 不能使用函数记忆功能。

#### containers/VisibleTodoList.js

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'

const mapStateToProps = (state, props) => {
  return {
    // 警告：下面的 selector 不会正确记忆
    todos: getVisibleTodos(state, props)
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

用 `createSelector` 创建的 selector 只有在参数集与之前的参数集相同时才会返回缓存的值。如果我们交替的渲染 `VisibleTodoList listId="1" />` 和 `VisibleTodoList listId="2" />`，共享的 selector 将交替的接收 `listId: 1` 和 `listId: 2`。这会导致每次调用时传入的参数不同，因此 selector 将始终重新计算而不是返回缓存的值。我们将在下一节了解如何解决这个限制。

### 跨多组件的共享 Selector

> 这节中的例子需要 React Redux v4.3.0 或者更高的版本

为了跨越多个 `VisibleTodoList` 组件共享 selector，**于此同时**正确记忆。每个组件的实例需要有拷贝 selector 的私有版本。

我们创建一个 `makeGetVisibleTodos` 的函数，在每个调用的时候返回一个 `getVisibleTodos` selector 的新拷贝。

#### selectors/todoSelectors.js

```js
import { createSelector } from 'reselect'

const getVisibilityFilter = (state, props) =>
  state.todoLists[props.listId].visibilityFilter

const getTodos = (state, props) =>
  state.todoLists[props.listId].todos

const makeGetVisibleTodos = () => {
  return createSelector(
    [ getVisibilityFilter, getTodos ],
    (visibilityFilter, todos) => {
      switch (visibilityFilter) {
        case 'SHOW_COMPLETED':
          return todos.filter(todo => todo.completed)
        case 'SHOW_ACTIVE':
          return todos.filter(todo => !todo.completed)
        default:
          return todos
      }
    }
  )
}
```

我们还需要一种每个容器访问自己私有 selector 的方式。`connect` 的 `mapStateToProps` 函数可以帮助我们。

**如果 `connect` 的 `mapStateToProps` 返回的不是一个对象而是一个函数，他将被用做为每个容器的实例创建一个单独的 `mapStateToProps` 函数。**

下面例子中的 `makeMapStateToProps` 创建一个新的 `getVisibleTodos` selectors，返回一个独占新 selector 的权限的 `mapStateToProps` 函数。

```js
const makeMapStateToProps = () => {
  const getVisibleTodos = makeGetVisibleTodos()
  const mapStateToProps = (state, props) => {
    return {
      todos: getVisibleTodos(state, props)
    }
  }
  return mapStateToProps
}
```

如果我们通过 `makeMapStateToProps` 来 `connect`，`VisibleTodosList` 容器的每个组件都会拥有含私有 `getVisibleTodos` selector 的 `mapStateToProps`。不论 `VisibleTodosList` 容器的展现顺序如何，记忆功能都会正常工作。

#### container/VisibleTodosList.js

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { makeGetVisibleTodos } from '../selectors'

const makeMapStateToProps = () => {
  const getVisibleTodos = makeGetVisibleTodos()
  const mapStateToProps = (state, props) => {
    return {
      todos: getVisibleTodos(state, props)
    }
  }
  return mapStateToProps
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  makeMapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

### 下一步

查看 [官方文档](https://github.com/reactjs/reselect) 和 [FAQ](https://github.com/reactjs/reselect#faq)。当因为太多的衍生计算和重复渲染导致出现性能问题时，大多数的 Redux 项目会开始使用 Reselect。所以在你创建一个大型项目的时候确保你对 reselect 是熟悉的。你也可以去研究他的 [源码](https://github.com/reactjs/reselect/blob/master/src/index.js)，这样你就不认为他是黑魔法了。







# 实现撤销历史

在应用中构建撤销和重做功能往往需要开发者刻意地付出一些精力。对于经典的 MVC 框架来说，这不是一个简单的问题，因为你需要克隆所有相关的 model 来追踪每一个历史状态。此外，你需要考虑整个撤销堆栈，因为用户的初始更改也是可撤销的。

这意味着在 MVC 应用中实现撤销和重做功能时，你不得不使用一些类似于 [Command](https://en.wikipedia.org/wiki/Command_pattern) 的特殊的数据修改模式来重写你的应用代码。

然而你可以用 Redux 轻而易举地实现撤销历史，因为以下三个原因：

* 不存在多个模型的问题，你需要关心的只是 state 的子树。
* state 是不可变数据，所有修改被描述成独立的 action，而这些 action 与预期的撤销堆栈模型很接近了。
* reducer 的签名 `(state, action) => state` 可以自然地实现 “reducer enhancers” 或者 “higher order reducers”。它们在你为 reducer 添加额外的功能时保持着这个签名。撤销历史就是一个典型的应用场景。

## 理解撤销历史

### 设计状态结构

撤销历史也是应用 state 的一部分，我们没有必要以不同的方式实现它。当你实现撤销和重做这个功能时，无论 state 如何随着时间不断变化，你都需要追踪 state 在不同时刻的**历史记录**。

如果我们希望在这样一个应用中实现撤销和重做的话，我们必须保存更多的 state 以解决下面几个问题：

* 撤销或重做留下了哪些信息？
* 当前的状态是什么？
* 撤销堆栈中过去（和未来）的状态是什么？

### 设计算法

无论何种特定的数据类型，重做历史记录的 state 结构始终一致：

```js
{
  past: Array<T>,
  present: T,
  future: Array<T>
}
```

让我们讨论一下如何通过算法来操作上文所述的 state 结构。我们可以定义两个 action 来操作该 state：`UNDO` 和 `REDO`。在 reducer 中，我们希望以如下步骤处理这两个 action：

#### 处理 Undo

* 移除 `past` 中的**最后一个**元素。
* 将上一步移除的元素赋予 `present`。
* 将原来的 `present` 插入到 `future` 的**最前面**。

#### 处理 Redo

* 移除 `future` 中的**第一个**元素。
* 将上一步移除的元素赋予 `present`。
* 将原来的 `present` 追加到 `past` 的**最后面**。

#### 处理其他 Action

* 将当前的 `present` 追加到 `past` 的**最后面**。
* 将处理完 action 所产生的新的 state 赋予 `present`。
* 清空 `future`。

### 设计算法

无论何种特定的数据类型，重做历史记录的 state 结构始终一致：

```js
{
  past: Array<T>,
  present: T,
  future: Array<T>
}
```

让我们讨论一下如何通过算法来操作上文所述的 state 结构。我们可以定义两个 action 来操作该 state：`UNDO` 和 `REDO`。在 reducer 中，我们希望以如下步骤处理这两个 action：

#### 处理 Undo

* 移除 `past` 中的**最后一个**元素。
* 将上一步移除的元素赋予 `present`。
* 将原来的 `present` 插入到 `future` 的**最前面**。

#### 处理 Redo

* 移除 `future` 中的**第一个**元素。
* 将上一步移除的元素赋予 `present`。
* 将原来的 `present` 追加到 `past` 的**最后面**。

#### 处理其他 Action

* 将当前的 `present` 追加到 `past` 的**最后面**。
* 将处理完 action 所产生的新的 state 赋予 `present`。
* 清空 `future`。

### 初识 Reducer Enhancers

你可能已经熟悉 [higher order function](https://en.wikipedia.org/wiki/Higher-order_function) 了。如果你使用过 React，也应该熟悉 [higher order component](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)。我们把这种模式加工一下，将其运用到 reducers。

**reducer enhancer**（或者 **higher order reducer**）作为一个函数，==接收 reducer 作为参数并返回一个新的 reducer==，这个新的 reducer 可以处理新的 action，或者维护更多的 state，亦或者将它无法处理的 action 委托给原始的 reducer 处理。这不是什么新模式，[`combineReducers()`](https://www.redux.org.cn/docs/api/combineReducers.html)也是 reducer enhancer，因为它同样接收多个 reducer 并返回一个新的 reducer。

###  编写 Reducer Enhancer

```js
function undoable(reducer) {
  // 以一个空的 action 调用 reducer 来产生初始的 state
  const initialState = {
    past: [],
    present: reducer(undefined, {}),
    future: []
  }

  // 返回一个可以执行撤销和重做的新的reducer
  return function (state = initialState, action) {
    const { past, present, future } = state;

    switch (action.type) {
      case 'UNDO':
        const previous = past[past.length - 1]
        const newPast = past.slice(0, past.length - 1)
        return {
          past: newPast,
          present: previous,
          future: [ present, ...future ]
        }
      case 'REDO':
        const next = future[0]
        const newFuture = future.slice(1)
        return {
          past: [ ...past, present ],
          present: next,
          future: newFuture
        }
      default:
        // 将其他 action 委托给原始的 reducer 处理
        const newPresent = reducer(present, action);
        if (present === newPresent) {
          return state
        }
        return {
          past: [ ...past, present ],
          present: newPresent,
          future: []
        }
    }
  }
}
```

我们现在可以将任意的 reducer 封装到`可撤销`的 reducer enhancer，从而处理 `UNDO` 和 `REDO` 这两个 action。

```js
// 这是一个 reducer。
function todos(state = [], action) {
  /* ... */
}

// 处理完成之后仍然是一个 reducer！
const undoableTodos = undoable(todos)

import { createStore } from 'redux'
const store = createStore(undoableTodos)

store.dispatch({
  type: 'ADD_TODO',
  text: 'Use Redux'
})

store.dispatch({
  type: 'ADD_TODO',
  text: 'Implement Undo'
})

store.dispatch({
  type: 'UNDO'
})
```

还有一个重要注意点：你需要记住当你恢复一个 state 时，必须把 `.present` 追加到当前的 state 上。你也不能忘了通过检查 `.past.length` 和 `.future.length` 确定撤销和重做按钮是否可用。

你可能听说过 Redux 受 [Elm 架构](https://github.com/evancz/elm-architecture-tutorial/) 影响颇深，所以不必惊讶于这个示例与 [elm-undo-redo package](http://package.elm-lang.org/packages/TheSeamau5/elm-undo-redo/2.0.0) 如此相似。

## 使用 Redux Undo

以上这些信息都非常有用，但是有没有一个库能帮助我们实现`可撤销`功能，而不是由我们自己编写呢？当然有！来看看 [Redux Undo](https://github.com/omnidan/redux-undo)，它可以为你的 Redux 状态树中的任何部分提供撤销和重做功能。

在这个部分中，你会学到如何让 [示例：Todo List](https://www.redux.org.cn/docs/basics/ExampleTodoList.html) 拥有可撤销的功能。你可以在 [`todos-with-undo`](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)找到完整的源码。

### 安装

首先，你必须先执行

```
npm install --save redux-undo
```

这一步会安装一个提供`可撤销`功能的 reducer enhancer 的库。

### 封装 Reducer

你需要通过 `undoable` 函数强化你的 reducer。例如，如果之前导出的是 todos reducer，那么现在你需要把这个 reducer 传给 `undoable()` 然后把计算结果导出：

#### `reducers/todos.js`

```js
import undoable, { distinctState } from 'redux-undo'

/* ... */

const todos = (state = [], action) => {
  /* ... */
}

const undoableTodos = undoable(todos, {
  filter: distinctState()
})

export default undoableTodos
```

这里的 `distinctState()` 过滤器会忽略那些没有引起 state 变化的 actions，可撤销的 reducer 还可以通过[其他选择](https://github.com/omnidan/redux-undo#configuration)进行配置，例如为撤销和重做的 action 设置 action type。

值得注意的是虽然这与调用 `combineReducers()` 的结果别无二致，但是现在的 `todos` reducer 可以传递给 Redux Undo 增强的 reducer。

#### `reducers/index.js`

```js
import { combineReducers } from 'redux'
import todos from './todos'
import visibilityFilter from './visibilityFilter'

const todoApp = combineReducers({
  todos,
  visibilityFilter
})

export default todoApp
```

你可以在 reducer 合并层次中的任何层级对一个或多个 reducer 执行 `undoable`。我们只对 `todos` reducer 进行封装而不是整个顶层的 reducer，这样 `visibilityFilter` 引起的变化才不会影响撤销历史。

### 更新 Selectors

现在 `todos` 相关的 state 看起来应该像这样：

```js
{
  visibilityFilter: 'SHOW_ALL',
  todos: {
    past: [
      [],
      [ { text: 'Use Redux' } ],
      [ { text: 'Use Redux', complete: true } ]
    ],
    present: [ { text: 'Use Redux', complete: true }, { text: 'Implement Undo' } ],
    future: [
      [ { text: 'Use Redux', complete: true }, { text: 'Implement Undo', complete: true } ]
    ]
  }
}
```

这意味着你必须通过 `state.todos.present` 访问 state 而不是原来的 `state.todos`：

#### `containers/VisibleTodoList.js`

```js
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos.present, state.visibilityFilter)
  }
}
```

### 添加按钮

现在只剩下给撤销和重做的 action 添加按钮。

首先，为这些按钮创建一个名为 `UndoRedo` 的容器组件。由于展示部分非常简单，我们不再需要把它们分离到单独的文件去：

#### `containers/UndoRedo.js`

```js
import React from 'react'

/* ... */

let UndoRedo = ({ canUndo, canRedo, onUndo, onRedo }) => (
  <p>
    <button onClick={onUndo} disabled={!canUndo}>
      Undo
    </button>
    <button onClick={onRedo} disabled={!canRedo}>
      Redo
    </button>
  </p>
)
```

你需要使用 [React Redux](https://github.com/reactjs/react-redux) 的 connect 函数生成容器组件，然后检查 `state.todos.past.length` 和 `state.todos.future.length` 来判断是否启用撤销和重做按钮。你不再需要给撤销和重做编写 action creators 了，因为 Redux Undo 已经提供了这些 action creators：

#### `containers/UndoRedo.js`

```js
/* ... */

import { ActionCreators as UndoActionCreators } from 'redux-undo'
import { connect } from 'react-redux'

/* ... */

const mapStateToProps = (state) => {
  return {
    canUndo: state.todos.past.length > 0,
    canRedo: state.todos.future.length > 0
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onUndo: () => dispatch(UndoActionCreators.undo()),
    onRedo: () => dispatch(UndoActionCreators.redo())
  }
}

UndoRedo = connect(
  mapStateToProps,
  mapDispatchToProps
)(UndoRedo)

export default UndoRedo
```

现在把这个 `UndoRedo` 组件添加到 `App` 组件：

#### `components/App.js`

```js
import React from 'react'
import Footer from './Footer'
import AddTodo from '../containers/AddTodo'
import VisibleTodoList from '../containers/VisibleTodoList'
import UndoRedo from '../containers/UndoRedo'

const App = () => (
  <div>
    <AddTodo />
    <VisibleTodoList />
    <Footer />
    <UndoRedo />
  </div>
)

export default App
```

就是这样！在[示例文件夹](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo)下执行 `npm install` 和 `npm start` 试试看吧！





# 子应用隔离

考虑一下这样的场景：有一个大应用（对应 `` 组件）包含了很多小的“子应用”（对应 `SubApp` 组件）：

```js
import React, { Component } from 'react'
import SubApp from './subapp'

class BigApp extends Component {
  render() {
    return (
      <div>
        <SubApp />
        <SubApp />
        <SubApp />
      </div>
    )
  }
}
```

这些 `` 是完全独立的。它们并不会共享数据或 action，也互不可见且不需要通信。

这时最好的做法是不要把它混入到标准 Redux 的 reducer 组件中。 对于一般型的应用，还是建议使用 reducer 组件。但对于 “应用集合”，“仪表板”，或者企业级软件这些把多个本来独立的工具凑到一起打包的场景，可以试下子应用的方案。

子应用的方案还适用于有多个产品或垂直业务的大团队。小团队可以独立发布子应用或者互相独立于自己的“应用壳”中。

下面是 connect 过的子应用的根组件。 像其它组件一样，它还可以渲染更多子组件，connect 或者没有 connect 的都可以。通常只要把它使用 `` 渲染就够了。

```js
class App extends Component { ... }
export default connect(mapStateToProps)(App)
```

但是，如果不想让外部知道子应用的组件是 Redux 应用的话，可以不调用 `ReactDOM.render()`。

或者可以在“大应用”中同时运行它的多个实例呢，还能保证每个在黑盒里运行，外界对 Redux 无感知。

为了使用 React API 来隐藏 Redux 的痕迹，在组件的构造方法里初始化 store 并把它包到一个特殊的组件中：

```js
import React, { Component } from 'react'
import { Provider } from 'react-redux'
import reducer from './reducers'
import App from './App'

class SubApp extends Component {
  constructor(props) {
    super(props)
    this.store = createStore(reducer)
  }

  render() {
    return (
      <Provider store={this.store}>
        <App />
      </Provider>
    )
  }
}
```

这样的话每个实例都是独立的。

如果应用间需要共享数据，*不* 推荐使用这个模式。 但是，如果大应用完全不需要访问子应用内部数据的话非常有用， 同时我们还想把 Redux 作为一种内部细节实现方式对外部隐藏。 每个组件实例都有它自己的 store，所以它们彼此是*不可见*的。





# Reducer 基础概念

一个 Redux reducer 函数需要具备：

* 应该有类似 `(previousState, action) => newState` 特征的函数，函数的类型与 [Array.prototype.reduce(reducer, ?initialValue)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 这个函数很相似。
* 应该是"纯"函数，纯函数意味着不能突变（原文mutate，意指直接修改引用所指向的值）它的参数，如果在函数中执行 API 调用，或者在函数外部修改值，又或者调用一个非纯函数比如 `Date.now()` 或 `Math.random()`，那么就会带来一些副作用。这意味着 state 的更新应该在**"不可变（immutable）"**的理念下完成，这就是说**总是去返回一个新的更新后的对象**，而不是直接去修改原始的 state tree。

> ##### 关于不可变（immutability）和突变（mutation）以及副作用
>
> 突变是一种不鼓励的做法，因为它通常会打乱调试的过程，以及 React Redux 的 `connect` 函数：
>
> * 对于调试过程, Redux DevTools 期望重放 action 记录时能够输出 state 值，而不会改变任何其他的状态。**突变或者异步行为会产生一些副作用，可能使调试过程中的行为被替换，导致破坏了应用。**
> * 对于 React Redux `connect` 来说，为了确定一个组件（component）是否需要更新，它会检查从 `mapStateToProps` 中返回的值是否发生改变。为了提升性能，`connect` 使用了一些依赖于不可变 state 的方法。并且使用浅引用（shallow reference）来检测状态的改变。这意味着**直接修改对象或者数组是不会被检测到的并且组件不会被重新渲染。**
>
> 其他的副作用像在 reducer 中生成唯一的 ID 或者时间戳时也会导致代码的不可预测并且难以调试和测试。

因为上面这些规则，在去学习具体的组织 Redux reducer 的技术之前，了解并完全理解下面这些核心概念是十分重要的。

#### Redux Reducer 基础

**核心概念**：

* 理解 state 和 state shape
* 通过拆分 state 来确定各自的更新职责（**reducer 组合**）
* 高阶 reducers
* 定义 reducer 的初始化状态

#### 纯函数和副作用

**核心概念**：

* 副作用
* 纯函数
* 如何理解组合函数

#### 纯函数和副作用

**核心概念**：

* 副作用
* 纯函数
* 如何理解组合函数

#### 不可变数据的管理

**核心概念**：

* 可变与不可变
* 安全地以不可变的方式更新对象和数组
* 避免在函数和语句中突变 state

#### 范式化数据

**核心概念**：

* 数据库的组织结构
* 拆分相关/嵌套数据到单独的表中
* 为每个被赋值的对象都存储一个单独的标识
* 通过 ID 引用对象
* 通过对象 ID 来查找表，通过一组 ID 来记录顺序
* 通过关系来联系各个对象



# Reducer 和 State 的基本结构

## Reducer 的基本结构

首先必须明确的是，==整个应用只有一个**单一的 reducer 函数**==：这个函数是传给 `createStore` 的第一个参数。一个单一的 reducer 最终需要做以下几件事：

* reducer 第一次被调用的时候，`state` 的值是 `undefined`。reducer 需要在 action 传入之前提供一个默认的 state 来处理这种情况。
* reducer 需要先前的 state 和 dispatch 的 action 来决定需要做什么事。
* 假设需要更改数据，应该用更新后的数据创建新的对象或数组并返回它们。
* 如果没有什么更改，应该返回当前存在的 state 本身。

写 reducer 最简单的方式是把所有的逻辑放在一个单独的函数声明中，就像这样：

```javascript
function counter(state, action) {
    if (typeof state === 'undefined') {
         state = 0; // 如果 state 是 undefined，用这个默认值初始化 store
     }
     if (action.type === 'INCREMENT') {
         return state + 1;
     }
     else if (action.type === 'DECREMENT') {
         return state - 1;
     }
     else {
         return state; // 未识别 action 会经过这里
     }
}
```

这个简单的函数满足上面提到的所有基本要求。在最开始会返回一个默认的值初始化 store；根据 action 的 type 决定 state 是哪种类型的更新，最后返回新的 state；如果没有什么要发生，会返回先前的 state。

这里有一些对这个 reducer 的简单调整。首先，重复的 `if/else` 语句看上去是很烦人的，可以使用 `switch` 语句代替他。其次，我们可以使用==ES6 的默认参数==来处理初始 state 不存在的情况。有了这些变化，reducer 看上去会长成这样：

```javascript
function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```

这是典型 Redux reducer 的基本结构。

## State 的基本结构

Redux 鼓励你根据要管理的数据来思考你的应用程序。数据就是你应用的 state，state 的==结构和组织方式==通常会称为 =="shape"==。在你组织 reducer 的逻辑时，state 的 shape 通常扮演一个重要的角色。

Redux state 中顶层的状态树通常是一个普通的 JavaScript  对象（当然也可以是其他类型的数据，比如：数字、数据或者其他专门的数据结构，但大多数库的顶层值都是一个普通对象）。在顶层对象中组织数据最常见的方式是将数据划分为子树，每个顶层的 key 对应着和特定域或者切片相关联的数据。例如，Todo 应用的 state 通常长这样：

```javascript
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
```

在这个例子中，`todos` 和 `visibilityFilter` 都是 state 的顶层 Key，他们分别代表着一个某个特定概念的数据切片。

大多数应用会处理多种数据类型，通常可以分为以下三类：

* 域数据（Domain data）: 应用需要展示、使用或者修改的数据（比如 从服务器检索到的所有 todos
* 应用状态（App state）: 特定于应用某个行为的数据（比如 “Todo #5 是现在选择的状态”，或者 “正在进行一个获取 Todos 的请求”）
* UI 状态（UI state）: 控制 UI 如何展示的数据（比如 “编写 TODO 模型的弹窗现在是展开的”）

Store 代表着应用核心，因此应该用域数据（Domain data）和应用状态数据（App state）定义 State，而不是用 UI 状态（UI state）。举个例子，`state.leftPane.todoList.todos` 这样的结构就是一个坏主意，因为整个应用的核心是 “todos” 而不仅仅是 UI 的一个模块。 `todos` 这个切片才应该是 state 结构的顶层。

UI 树和状态树之间很少有 1 对 1 的关系。除非你想明确的跟踪你的 Redux Store 中存储的 UI 数据的各个方面，但即使是这样，UI 数据的结构和域数据的结构也是不一样的。

一个典型的应用 state 大致会长这样：

```javascript
{
    domainData1 : {},
    domainData2 : {},
    appState1 : {},
    appState2 : {},
    ui : {
        uiState1 : {},
        uiState2 : {},
    }
}
```



# 拆分 Reducer 逻辑

对于任何一个有意义的应用来说，将所有的更新逻辑都放入到单个 reducer  函数中都将会让程序变得不可维护。虽然说对于一个函数应该有多长没有准确的规定，但一般来讲，函数应该比较短，并且只做一件特定的事。因此，把很长的，同时负责很多事的代码拆分成容易理解的小片段是一个很好的编程方式。

因为 Redux reducer 也仅仅是一个函数，上面的概念也适用。你可以将 reducer 中的一些逻辑拆分出去，然后在父函数中调用这个新的函数。

这些新的函数通常分为三类：

1. 一些小的工具函数，包含一些可重用的逻辑片段
2. 用于处理特定情况下的数据更新的函数，参数除了 `(state, action)` 之外，通常还包括其它参数
3. 处理给定 state 切片的所有更新的函数，参数格式通常为 `(state, action)`

为了清楚起见，这些术语将用于区分不同类型的功能和不同的用例：

* **reducer:** 任何符合 `(state, action) -> newState` 格式的函数（即，可以用做 `Array.reducer` 参数的任何函数）。
* ==**root reducer:** 通常作为 `createStore` 第一个参数的函数。他是唯一的一个在所有的 reducer 函数中必须符合 `(state, action) -> newState` 格式的函数。==
* **slice reducer:** 一个负责处理状态树中一块切片数据的函数，通常会作为 `combineReducers` 函数的参数。
* **case function:** 一个负责处理特殊 action 的更新逻辑的函数。可能就是一个 reducer 函数，也可能需要其他参数才能正常工作。
* **higher-order reducer:** 一个以 reducer 函数作为参数，且/或返回一个新的 reducer 函数的函数（比如： `combineReducers`, `redux-undo`）。

在各种讨论中 “sub-reducer” 这个术语通常表示那些不是 root reducer  的任何函数，但这个表述并不是很精确。一些人认为应该表示 "业务逻辑（business login）" （与应用程序特定行为相关的功能）或者  “工具函数（utility functions）”（非应用程序特定的通用功能）。

将复杂的环境分解为更小，更易于理解的过程就是术语中的 [函数分解(functional decomposition)](http://stackoverflow.com/questions/947874/what-is-functional-decomposition)。这个术语可以用在任何代码中。在 Redux 中，使用第三个方法来构造 reducer 逻辑是非常普遍的，即更新逻辑被委托在基于 state 切片的的其他函数中。Redux 将这个概念称为 **reducer composition**，带目前为止，这个方法是构建 reducer 逻辑最常用的方法。事实上， Redux 包含一个 [`combineReducers()`](https://www.redux.org.cn/docs/api/combineReducers.html) 的工具函数，它专门抽象化基于 state 切片的其他 reducer 函数的工作过程。但是你必须明确的是，这并不是唯一模式。实际上，完全可以用所有的三种方法拆分逻辑，通常情况下，这也是一个好主意。[Refactoring Reducers](https://www.redux.org.cn/docs/recipes/reducers/RefactoringReducersExample.html) 章节会演示一些实例。



# `combineReducers` 用法

## 核心概念

基于 Redux 的应用程序中最常见的 state 结构是一个简单的 ==JavaScript 对象==，它最外层的每个 key  中拥有特定域的数据。类似地，给这种 state 结构写 reducer 的方式是分拆成多个 reducer，拆分之后的 reducer  都是相同的结构（state, action），并且每个函数独立负责管理该特定切片 state 的更新。多个拆分之后的 reducer  可以响应一个 action，在需要的情况下独立的更新他们自己的切片 state，最后组合成新的 state。

这个模式是如此的通用，Redux 提供了 `combineReducers` 去实现这个模式。这是一个高阶 Reducer 的示例，他接收一个拆分后 reducer 函数组成的对象，返回一个新的 Reducer 函数。

在使用 `combineReducer` 的时候你需要重点注意下面几个方法：

* 首先，`combineReducer` ==只是一个工具函数==，用于简化编写 Redux reducer 时最常见的场景。你没有必要一定在你的应用程序中使用它，他不会处理每一种可能的场景。你完全可以不使用它来编写 reducer，或者对于 `combinerReducer` 不能处理的情况编写自定义的 reducer。（参见 [combineReducers](https://www.redux.org.cn/docs/recipes/reducers/BeyondCombineReducers.html) 章节中的例子和建议）
* 虽然 Redux 本身并不管你的 state 是如何组织的，但是 combineReducer 强制地约定了几个规则来帮助使用者们避免常见的错误（参见 [combineReducer](https://www.redux.org.cn/docs/api/combineReducers.html)）
* 一个常见的问题是 Reducer 在 dispatch action 的时候是否调用了所有的 reducer。当初你可能觉得“不是”，因为真的只有一个根 reducer 函数啊。但是 `combineReducer` 确实有着这样的特殊效果。在生成新的 state 树时，`combinerReducers` 将调用每一个拆分之后的 reducer 和与当前的 Action，如果有需要的话会使得每一个 reducer 有机会响应和更新拆分后的 state。所以，在这个意义上，==`combineReducers` 会调用所有的 reducer，严格来说是它包装的所有 reducer==。
* 你可以在任何级别的 reducer 中使用 `combineReducer`，不仅仅是在创建根 reducer 的时候。在不同的地方有多个组合的 reducer 是非常常见的，他们组合到一起来创建根 reducer。

## 定义 State 结构

这里有两种方式来定义 Store state 的初始结构和内容。==首先==，`createStore` 函数可以将 `preloadedState` 作为第二个参数。这主要用于初始化那些在其他地方有持久化存储的 state，例如浏览器的 localStorage，==另外==一种方式是当 state 是 `undefined` 的时候返回 initial state。这两种方法在 [初始化 state 章节](https://www.redux.org.cn/docs/recipes/reducers/InitializingState.html) 中有着更加详细的描述，但是在使用 `combineReducers` 的时候需要注意其他的一些问题。

`combineReducers` 接收拆分之后的 reducer 函数组成的对象，并且创建出具有相同键对应状态对象的函数。这意味着如果没有给 `createStore` 提供预加载 state，输出 state 对象的 key 将由输入的拆分之后 reducer 组成对象的 key 决定。这些名称之间的相关性并不总是显而易见的，尤其是在使用 ES6 的时候（如模块的默认导出和对象字面量的简写时）。

这儿有一些如何用 ES6 中对象字面量简写方式使用 `combineReducers` 的例子。

```javascript
// reducers.js
export default theDefaultReducer = (state = 0, action) => state;

export const firstNamedReducer = (state = 1, action) => state;

export const secondNamedReducer = (state = 2, action) => state;


// rootReducer.js
import {combineReducers, createStore} from "redux";

import theDefaultReducer, {firstNamedReducer, secondNamedReducer} from "./reducers";


// 使用 ES6 的对象字面量简写方式定义对象结构
const rootReducer = combineReducers({
    theDefaultReducer,
    firstNamedReducer,
    secondNamedReducer
});

const store = createStore(rootReducer);
console.log(store.getState());
// {theDefaultReducer : 0, firstNamedReducer : 1, secondNamedReducer : 2}
```

因为我们使用了 ES6 中的对象字面量简写方式，在最后的 state 中 key 的名字和 import 进来的变量的名字一样，这可能并不是经常期望的，经常会对不熟悉 ES6 的人造成困惑。

同样的，结果的名字也有点奇怪，在 state 中 key 的名字包含 “reducer” 这样的词通常不是一个好习惯，key  应该反映他们特有域或者数据类型。这意味着我们应该明确拆分之后 reducer 对象中 key 的名称，定义输出 state 对象中的  key，或者在使用对象字面量简写方式的时候，仔细的重命名的拆分之后的 reducer 以设置 key。

一个比较好用的使用示例如下：

```javascript
import {combineReducers, createStore} from "redux";

// 将 default import 进来的名称重命名为任何我们想要的名称。我们也可以重命名 import 进来的名称。
import defaultState, {firstNamedReducer, secondNamedReducer as secondState} from "./reducers";

const rootReducer = combineReducers({
    defaultState,                   // key 的名称和 default export 的名称一样
    firstState : firstNamedReducer, // key 的名字是单独取的，而不是变量的名字
    secondState,                    // key 的名称和已经被重命名过的 export 的名称一样
});

const reducerInitializedStore = createStore(rootReducer);
console.log(reducerInitializedStore.getState());
// {defaultState : 0, firstState : 1, secondState : 2}
```

这种 state 的结构恰好能反应所涉及的数据，因为我们特别的设置了我们传递给 `combineReducers` 的 key。



# `combineReducers` 进阶

Redux 引入了非常实用的 `combineReducers`  工具函数，但我们却粗暴地将它限制于单一的应用场景：把不同片段的 state 的更新工作委托给一个特定的 reducer，以此更新由普通的  JavaScript 对象构成的 state 树。它不解决 Immutable.js Maps 所构建的 state  tree，也不会把其余部分的 state 作为额外参数传递给 reducer 或者排列 reducer 的调用顺序，它同样不关心 reducer 如何工作。

于是一个常见问题出现了，“`combineReducers` 如何处理这些应用场景呢？”通常给出的回答只是“你不能这么做，你可能需要通过其他方式解决”。**一旦你突破 `combineReducers` 的这种限制，就是创建各色各样的“自定义” reducer 的时候了**，不管是为了解决一次性场景的特殊 reducer，还是能够被广泛复用的 reducer。本文为几种典型的应用场景提供了建议，但你也可以自由发挥。

## 结合 Immutable.js 对象使用 reducers

由于目前 `combineReducers` 只能处理普通的 JavaScript 对象，对于把 Immutable.js Map 对象作为顶层 state 树的应用程序来说，可能无法使用 `combineReducers` 管理应用状态。因为很多开发者采用了 Immutable.js，所以涌现了大量提供类似功能的工具，例如 [redux-immutable](https://github.com/gajus/redux-immutable)。这个第三方包实现了一个能够处理 Immutable Map 数据而非普通的 JavaScript 对象的 `combineReducers`。

## 不同 reducers 之间共享数据

类似地，如果 `sliceReducerA` 为了处理特殊的 action 正好需要来自 `sliceReducerB` 的部分 state 数据，或者 `sliceReducerB` 正好需要全部的 state 作为参数，单单就 `combineReducers` 是无法解决这种问题的。可以这样来解决：把所需数据当额外参数的形式传递给自定义函数，例如：

```js
function combinedReducer(state, action) {
    switch(action.type) {
        case "A_TYPICAL_ACTION" : {
            return {
                a : sliceReducerA(state.a, action),
                b : sliceReducerB(state.b, action)
            };
        }
        case "SOME_SPECIAL_ACTION" : {
            return {
                // 明确地把 state.b 作为额外参数进行传递
                a : sliceReducerA(state.a, action, state.b),
                b : sliceReducerB(state.b, action)
            }        
        }
        case "ANOTHER_SPECIAL_ACTION" : {
            return {
                a : sliceReducerA(state.a, action),
                // 明确地把全部的 state 作为额外参数进行传递
                b : sliceReducerB(state.b, action, state)
            }         
        }    
        default: return state;
    }
}
```

另一种解决“共享片段数据更新” (shared-slice updates) 问题的简单方法是，==给 action 添加额外数据==。可以通过 thunk 函数或者类似的方法轻松实现，如下：

```js
function someSpecialActionCreator() {
    return (dispatch, getState) => {
        const state = getState();
        const dataFromB = selectImportantDataFromB(state);

        dispatch({
            type : "SOME_SPECIAL_ACTION",
            payload : {
                dataFromB
            }
        });
    }
}
```

因为 B 的数据已经存在于 action 中，所以它的父级 reducer 不需要做任何特殊的处理就能把数据暴露给 `sliceReducerA`。

第三种方法是：使用 `combineReducers` 组合 reducer 来处理“简单”的场景，每个 reducer 依旧只更新自己的数据；同时新加一个 reducer 来处理多块数据交叉的“复杂”场景；最后写一个包裹函数依次调用这两类 reducer 并得到最终结果：

```js
const combinedReducer = combineReducers({
    a : sliceReducerA,
    b : sliceReducerB
});

function crossSliceReducer(state, action) {
    switch(action.type) {
        case "SOME_SPECIAL_ACTION" : {
            return {
                // 明确地把 state.b 作为额外参数进行传递
                a : handleSpecialCaseForA(state.a, action, state.b),
                b : sliceReducerB(state.b, action)
            }        
        }
        default : return state;
    }
}

function rootReducer(state, action) {
    const intermediateState = combinedReducer(state, action);
    const finalState = crossSliceReducer(intermediateState, action);
    return finalState;
}
```

已经有一个库 [reduce-reducers](https://github.com/acdlite/reduce-reducers) 可以简化以上操作流程。它接收多个 reducer 然后对它们依次执行 `reduce()` 操作，并把产生的中间值依次传递给下一个 reducer：

```js
// 与上述手动编写的 `rootReducer` 一样
const rootReducer = reduceReducers(combinedReducers, crossSliceReducer);
```

值得注意的是，如果你使用 `reduceReducers` 你应该确保第一个 reducer 能够定义初始状态的 state 数据，因为后续的 reducers 通常会假定 state 树已经存在，也就不会为此提供默认状态。

## 更多建议

再次强调，Reducer *只是*普通的函数，明确这一概念非常重要。`combineReducers` 虽然实用也只是冰山一角。除了用 switch 语句编写函数，还可以用条件逻辑；函数不仅可以彼此组合，也可以调用其他函数。也许你可能需要这样的一个 reducer，它既能够重置 state，也能够响应特定的 action。你可以这样做：

```js
const undoableFilteredSliceA = compose(undoReducer, filterReducer("ACTION_1", "ACTION_2"), sliceReducerA);
const rootReducer = combineReducers({
    a : undoableFilteredSliceA,
    b : normalSliceReducerB
});
```

注意 `combineReducers` 无需知道也不关心任何一个负责管理 `a` 数据的 reducer。所以我们并不需要像以往一样修改 `combineReducers` 来实现撤销功能 —— 我们只需把各种函数组合成一个新函数即可。

另外 `combineReducers` 只是 Redux 内置的 reducer 工具，大量形式各异的可复用的第三方 reducer 工具层出不穷。在 [Redux Addons 目录](https://github.com/markerikson/redux-ecosystem-links)中列举了很多可供使用的第三方工具。也许这些工具解决不了你的应用场景，但你随时都可以实现一个能够满足你需求的工具函数。



# 管理范式化数据

我们经常使用 Normaizr 库将嵌套式数据转化为适合集成到 store 中的范式化数据。但这并不解决针对范式化的数据进一步更新后在应用的其他地方使用的问题。

## 标准方法

### 简单合并

一种方法是==将 action 的内容合并到现有的 state==。在这种情况下，我们需要一个对数据的深拷贝（非浅拷贝）。Lodash 的 `merge` 方法可以帮我们处理这个：

```javascript
import merge from "lodash/object/merge";

function commentsById(state = {}, action) {
    switch(action.type) {
        default : {
           if(action.entities && action.entities.comments) {
               return merge({}, state, action.entities.comments.byId);
           }
           return state;
        }
    }
}
```

这样做会让 reducer 保持最小的工作量，==但需要 action creator 在 action dispatch 之前做大量的工作来将数据转化成正确的形态。在删除数据项时这种方式也是不适合的==。

### reducer 切片组合

如果我们有一个由切片 reducer 组成的嵌套数据，每个切片 reducer 都需要知道如何响应这个 action。因为我们需要让  action 囊括所有相关的数据。譬如更新相应的 Post 对象需要生成一个 comment 的 id，然后使用 id 作为 key  创建一个新的 comment 对象，并且让这个 comment 的 id 包括在所有的 comment id  列表中。下面是一个如何组合这样数据的例子：

```javascript
// actions.js
function addComment(postId, commentText) {
    // 为这个 comment 生成一个独一无二的 ID
    const commentId = generateId("comment");

    return {
        type : "ADD_COMMENT",
        payload : {
            postId,
            commentId,
            commentText
        }
    };
}


// reducers/posts.js
function addComment(state, action) {
    const {payload} = action;
    const {postId, commentId} = payload;

    // 查找出相应的文章，简化其余代码
    const post = state[postId];

    return {
        ...state,
        // 用新的 comments 数据更新 Post 对象
        [postId] : {
             ...post,
             comments : post.comments.concat(commentId)             
        }
    };
}

function postsById(state = {}, action) {
    switch(action.type) {
        case "ADD_COMMENT" : return addComment(state, action);
        default : return state;
    }
}

function allPosts(state = [], action) {
    // 省略，这个例子中不需要它
}

const postsReducer = combineReducers({
    byId : postsById,
    allIds : allPosts
});


// reducers/comments.js
function addCommentEntry(state, action) {
    const {payload} = action;
    const {commentId, commentText} = payload;

    // 创建一个新的 Comment 对象
    const comment = {id : commentId, text : commentText};

    // 在查询表中插入新的 Comment 对象
    return {
        ...state,
        [commentId] : comment
    };
}

function commentsById(state = {}, action) {
    switch(action.type) {
        case "ADD_COMMENT" : return addCommentEntry(state, action);
        default : return state;
    }
}


function addCommentId(state, action) {
    const {payload} = action;
    const {commentId} = payload;
    // 把新 Comment 的 ID 添加在 all IDs 的列表后面
    return state.concat(commentId);
}

function allComments(state = [], action) {
    switch(action.type) {
        case "ADD_COMMENT" : return addCommentId(state, action);
        default : return state;
    }
}

const commentsReducer = combineReducers({
    byId : commentsById,
    allIds : allComments
});
```

这个例子之所有有点长，是因为它展示了不同切片 reducer 和 case reducer 是如何配合在一起使用的。注意这里对 “委托”  的理解。postById reducer 切片将工作委拖给 addComment，addComment 将新的评论 id  插入到相应的数据项中。同时 commentsById 和 allComments 的 reducer 切片都有自己的 case  reducer，他们更新评论查找表和所有评论 id 列表的表。

## 其他方法

### 基于任务的更新

reducer 仅仅是个函数，因此有无数种方法来拆分这个逻辑。使用切片 reducer 是最常见，但也可以在更面向任务的结构中组织行为。由于通常会涉及到更多嵌套的更新，因此常常会使用 [dot-prop-immutable](https://github.com/debitoor/dot-prop-immutable)、[object-path-immutable](https://github.com/mariocasciaro/object-path-immutable) 等库实现不可变更新。

```javascript
import posts from "./postsReducer";
import comments from "./commentsReducer";
import dotProp from "dot-prop-immutable";
import {combineReducers} from "redux";
import reduceReducers from "reduce-reducers";

const combinedReducer = combineReducers({
    posts,
    comments
});


function addComment(state, action) {
    const {payload} = action;
    const {postId, commentId, commentText} = payload;

    // State here is the entire combined state
    const updatedWithPostState = dotProp.set(
        state,
        `posts.byId.${postId}.comments`,
        comments => comments.concat(commentId)
    );

    const updatedWithCommentsTable = dotProp.set(
        updatedWithPostState,
        `comments.byId.${commentId}`,
        {id : commentId, text : commentText}
    );

    const updatedWithCommentsList = dotProp.set(
        updatedWithCommentsTable,
        `comments.allIds`,
        allIds => allIds.concat(commentId);
    );

    return updatedWithCommentsList;
}

const featureReducers = createReducer({}, {
    ADD_COMMENT : addComment,
};

const rootReducer = reduceReducers(
    combinedReducer,
    featureReducers
);
```

这种方法让 `ADD_COMMENT` 这个 case 要干哪些事更加清楚，但需要更新嵌套逻辑和对特定状态树的了解。最后这取决于你如何组织 reducer 逻辑，或许你根本不需要这样做。

