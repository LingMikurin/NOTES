# 寒哥手记
- [开发环境搭建](#最开始还是从开发环境搭建开始)
- [Component的导入](#Component的导入)
- [React项目TodoList](#React项目TodoList)
- [super(props)](#super应用)
- [最原始 Junior version0.3.3](#跳转一)
- [immutable](#immutable)
- [回调函数index相关](#回调函数index相关)
- [题外话createRoot](#题外话createRoot)
- [Fragment组件化搭建](#Fragment组件化搭建部分)
- [this绑定](#this绑定)
- [TodoList代码优化。构造函数中绑定 this](#构造函数中绑定this)
- [新版setState()使用](#新版setState()使用)
- [父组件和子组件之间的通信方式](#父组件和子组件之间的通信方式)
- [Prop Types 与 DefaultProps](#junior-version0.4.2)
- [props, state and render](#props, state and render)
- [虚拟DOM](#虚拟DOM)
- [ref 的应用](#Junior version0.4.7 😂 TodoList 中 ref 的应用)
- [生命周期函数](#生命周期函数)
- [Junior version0.4.9运用生命周期性能优化，引入ajax](#Junior version0.4.9 😂)
- [ajax ，axios的应用](#Junior version0.4.10 😂 TodoList 中 ajax ，axios的应用)
- [零碎知识点](#插入补充零碎知识点)

## 此记录了个人React的自学过程，并配合一个小项目TodoList来学习，手记中有原始版本代码和最终优化版本，可以提前跳转页尾查看，也可以跟着手记走。
---------
## 最开始还是从开发环境搭建开始：

### 脚手架搭建：
```js
npx create-react-app my-app
cd my-app
npm start
```



### git commit:
```js
Success! Created todolist at /Users/ling/Documents/Deliberate Practice/React/todolist
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back
```

### 开始前的最后一脚：
```js
  cd 你的项目所在文件夹
  npm start
```
---------


## Component的导入:



### 由于使用jsx的原因 import 开头第一个字母大写，例：import React from 'react'
```js
import {Component} from 'react';
等价原来再此之前版本写法：
```
```js
import React from 'react'
const Component = React.Component
```



### 将组件 TodoList 渲染到页面中 id 为 'root' 的元素中


```js
import React from 'react';
import ReactDOM from 'react-dom';
import TodoList from './TodoList';

ReactDOM.render(<TodoList />, document.getElementById('root'));
```
### 使用 ReactDOM.createRoot() 函数渲染组件：
```js
const root = document.getElementById('root');
ReactDOM.createRoot(root).render(<TodoList />);
```
### 继续精简：
```js
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```
### 补充StricMode：
```js
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
<React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### 定义了一个名为 App 的 React 组件


```js
import React, { Component } from "react";
class App extends Component {
  render() {
    return <div>Hello</div>;
  }
}

export default App;
```
### 也可以使用类的方式来写一个组件，例如：
```js
import React from "react";

class App extends React.Component {
  render() {
    return <div>Hello</div>;
  }
}

export default App;
```
### 或者使用函数式组件的写法：
```js
const App = () => {
  return <div>Hello</div>;
}

export default App;
```
### 也可以使用更简短的 JSX 语法：
```js
const MyComponent = () => <div>Hewwllo</div>;

export default MyComponent;
```

### 补充点：
```js
//import React from 'react';

export default () => <div>Hello</div>;
//为什么删除import React from 'react';这一行代码后依然可以成功运行？
React 在 JSX 中是不需要被显式地导入的。这是因为 JSX 会被自动地转换成常规 JavaScript 调用，如：

React.createElement('div', null, 'Hello')
```
所以，即使在不导入 React 的情况下使用 JSX，这段代码也会成功运行。



然而，通常还是建议导入 React，因为导入 React 可以帮助类型检查工具（如 TypeScript）提供对 React 元素的类型信息，进而提升编写代码的效率。此外，导入 React 还可以帮助代码转换工具（如 Babel）正确地转换 JSX 代码。



### 在 JavaScript 中，只有在你使用 JSX 标签时，才需要导入 React 库。如果你删除了导入 React 的代码，你不会使用任何 JSX 标签，所以你的代码不会有任何报错。



假设你有一个组件如下：
```js
import React from 'react';

const MyComponent = () => {
  return (
    <div>Hello, World!</div>
  );
}

export default MyComponent;
```

在这个组件中，我们使用了 <div> 这个 JSX 标签。在使用 JSX 标签时，必须导入 React 库。所以你不能删除 import React from 'react'; 这行代码。



如果你改变了组件的代码，使它不使用任何 JSX 标签，就可以删除 import React from 'react'; 这行代码。例如，你可以将组件改写为：
```js
const MyComponent = () => {
  return 'Hello, World!';
}

export default MyComponent;
```

在这个组件中，我们没有使用任何 JSX 标签。所以你可以删除导入 React 的代码。



总的来说，如果你的组件中使用了 JSX 标签，就必须导入 React 库。如果你的组件中没有使用 JSX 标签，就可以不用导入 React 库。



JSX 语法使用了 React 元素，所以在使用 JSX 语法时需要引入 React。然而，在这段代码中并没有使用 JSX 语法，而是使用了一个箭头函数返回了一个字符串，所以可以在删除 React 的引入语句后依然正常运行。



### 如果你在函数式组件中使用了 JSX 语法，则必须引入 React，否则会报错。例如：
```js
import React from 'react';

const MyComponent = () => {
  return <div>Hello</div>;
};

export default MyComponent;
```
在这段代码中，如果删除了 React 的引入语句，则会报错：'React' must be in scope when using JSX.



💣注意，函数式组件不能使用类的构造函数和render方法。

另外，注意使用函数式组件时，你只能在组件函数的返回值中使用JSX语法，而不能在组件函数的其他地方使用。
这是因为函数式组件不支持在组件内部更改状态，也不需要渲染周期方法，所以只需要一个纯函数来返回需要渲染的 JSX。
因此，你不能在函数式组件内部使用 this 关键字，也不能使用生命周期方法或类中的其他方法。






---------
## React项目TodoList
这个组件由一个构造函数和一个 render 函数组成。
```js
import React, { Component, Fragment } from 'react'

class TodoList extends Component {
  constructor(props) {
    super(props)
    this.state = {
      inputValue: 'hello',
      list: [],
    }
  }

  render() {
    return (
      <Fragment>
        <div>
          <input value={this.state.inputValue} />
          <button>submit</button>
        </div>
        <ul>
          <li>Learning React</li>
          <li>Learning TypeScript</li>
          <li>Learning NodeJs</li>
        </ul>
      </Fragment>
    )
  }
}
export default TodoList
```

### super应用

类里面的constructor是优先第一个执行的：
```js
constructor(props) {
    super(props)
    this.state = {
      inputValue: 'hello',
      list: [],
    }
  }
```
在 JavaScript 中，super 是一个保留字，用于在一个子类的构造函数中引用父类的构造函数。



在这个例子中，super(props) 调用了父类（也就是 Component）的构造函数，并传入了当前组件的 props 作为参数。这样做的原因是，在继承了父类之后，子类需要调用父类的构造函数来初始化父类的内部状态。



这里也可以看到，组件类的构造函数通常是以 constructor(props) 的形式声明的，其中的 props 参数用于接收组件的属性（properties）。



super(props) 是在 JavaScript 类中调用父类的构造函数的方法。它通常会出现在子类的构造函数中，并且必须放在构造函数的第一行。



在这种情况下，通过调用 super(props)，子类可以继承父类的属性并将其传递给父类的构造函数。这对于在子类中使用父类的方法和属性非常重要。



例如，在上面的代码中，如果你想在子类中使用父类的属性 props，你就必须通过调用 super(props) 来实现。


```js
this.state

this.state = {
      inputValue: 'hello',
      list: [],
    }
```

this.state是在 class 组件中所有组件实例都可以使用的特殊变量。它存储了组件当前的状态，并且可以通过调用 setState 方法来修改状态。



在上述代码中，this.state 是在组件的构造函数中初始化的，它包含了两个属性：inputValue 和 list。当组件的状态发生改变时，组件会自动重新渲染。





<input />里的数据和 this.state里的数据进行绑定
```js
<input value={this.state.inputValue} />
jsx 语法中保存变量要加花括号。
```

---------
## 下面手记内容需要结合TodoList代码来消化



#### 跳转一
## 最原始 Junior version0.3.3

(0.3.3前的版本属于基本开发环境准备，没存在我的笔记，记录在了我的sandbox,有需求可以找我要)
```js
import React, { Component, Fragment } from "react";
import "./style.css";

class TodoList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      inputValue: "",
      list: []
    };
  }

  render() {
    return (
      <Fragment>
        <div>
          <label htmlFor="insertArea">输入内容</label>
          <input
            id="insertArea"
            className="input"
            value={this.state.inputValue}
            onChange={this.handleInputChange.bind(this)}
          />
          <button onClick={this.handleBtnClick.bind(this)}>提交</button>
        </div>
        <ul>
          {this.state.list.map((item, index) => {
            return (
              <li
                key={index}
                onClick={this.handleItemDelete.bind(this, index)}
                dangerouslySetInnerHTML={{ __html: item }}
              ></li>
            );
          })}
        </ul>
      </Fragment>
    );
  }

  handleInputChange(e) {
    this.setState({
      inputValue: e.target.value
    });
  }

  handleBtnClick() {
    this.setState({
      list: [...this.state.list, this.state.inputValue],
      inputValue: ""
    });
  }

  handleItemDelete(index) {
    // immutable
    // state 不允许我们做任何的改变
    const list = [...this.state.list];
    list.splice(index, 1);

    this.setState({
      list: list
    });
  }
}

export default TodoList;

```
### immutable

state 不允许我们做任何的改变


```js
  handleItemDelete(index) {
    // immutable
    // state 不允许我们做任何的改变
    const list = [...this.state.list];
    list.splice(index, 1);

    this.setState({
      list: list
    });
  }
```
为了简化代码这段代码如下改写可以吗？


```js
handleItemDelete(index) {
      this.state.list.splice(index,1)
      this.setState({
      list:list
      })
  }
```
和
```js
handleItemDelete(index) {
      this.state.list.splice(index,1)
      this.setState({
      list:this.state.list
      })
  }
```

### state 不允许我们做任何的改变，因为会影响之后react之后的性能优化问题。



在 React 中，state 是组件内部保存数据的地方。当组件的 state 发生变化时，React 会自动调用 render 方法来重新渲染组件。在组件中，我们应该避免直接修改 state，而是使用 setState 方法来修改 state。



直接修改 state 的做法会导致组件的状态管理变得混乱，并且不方便跟踪状态的变化。使用 setState 方法来修改 state 可以帮助我们更好地管理组件的状态，并且可以方便地跟踪状态的变化。



在上面的代码中，如果直接修改 this.state.list，那么 React 就不会自动调用 render 方法来重新渲染组件。这会导致组件的状态与界面上的显示不同步，从而出现 bug。使用 setState 方法修改 state 可以避免这种问题。



如果非需要改变的时候自己拷贝出来一个副本
```js
const list = [...this.state.list];
```

### 回调函数index相关
```js
<ul>
  {this.state.list.map((item, index) => {
    return (
      <li
        key={index}
        onClick={this.handleItemDelete.bind(this, index)}
        dangerouslySetInnerHTML={{ __html: item }}
      ></li>
    );
  })}
</ul>;
```

### key={index}



在 React 中，对于每个元素都要提供一个唯一的 key 值。key 的作用是在 DOM 中的某些变动的时候帮助 React 识别哪些元素发生了变化。



在列表渲染时，React 会通过比较每一项的 key 值来确定哪些项目发生了变化，从而决定是执行增量更新还是重新渲染。因此，如果你的列表项目的顺序发生了变化，使用 index 做为 key 值会导致 React 重新渲染整个列表。



这是因为在列表项目发生变化的时候，React 会比较之前的列表和新的列表，并根据项目的 key 值来确定每一项是新的还是变化的。如果使用 index 作为 key 值，则在列表项目发生变化的时候，React 会认为整个列表都是新的，导致重新渲染整个列表。



为什么不推荐在函数中使用index作为key的值，这是因为，当您的列表中的项目发生变化时（例如，添加，删除或移动），React将使用项目的键来确定哪些元素需要更新。如果使用的是索引来作为键，则在列表中的项目发生变化时，React将不知道需要更新哪些元素，可能导致渲染错误或性能问题。因此，您应该使用唯一的值来作为键，例如项目的ID。



所以，在使用 map 函数渲染列表使用索引做为 key 值的一个主要缺点是，当你增加、删除或插入列表中的项目时，它会导致 React 重新渲染整个列表。这样的性能是很低的，因为它会导致大量的 DOM 操作。正确的做法是使用唯一的标识符来作为 key 值。例如，如果你的列表项目有一个 id 属性，你可以将其用作 key 值。



示例代码：
```js
<ul>
  {this.state.list.map((item, index) => {
    return (
      <li
        key={item.id}
        onClick={this.handleItemDelete.bind(this, index)}
        dangerouslySetInnerHTML={{ __html: item }}
      ></li>
    );
  })}
</ul>;
```
这样，在列表中增加、删除或插入项目时，React 就可以准确地知道哪些项目发生了变化，并且只对这些项目进行重新渲染。



### 题外话createRoot 
```js
ReactDOM.render is no longer supported in React 18.

Use createRoot instead
```


使用 createRoot 来渲染组件的步骤如下：



1导入 createRoot 函数：
```js
import { createRoot } from 'react-dom';
```

2.创建根节点，并调用 createRoot 方法传入根节点：

```js
const root = document.getElementById('root');
createRoot(root).render(<App />);
```

3.在需要更新组件时，调用 render 方法更新组件：
```js
createRoot(root).render(<App />);
```

注意：createRoot 只能在顶层使用，不能用于嵌套的组件。



参考文档：

[createRoot - React  (链接有效)](https://reactjs.org/docs/react-dom.html#createroot)


使用createRoot 方法改写下面代码：
```js
import React from "react";
import ReactDOM from "react-dom";
import TodoList from "./TodoList";

ReactDOM.render(<TodoList />, document.getElementById("root"));
```

使用 createRoot 方法改写上面的代码：
```js
import React from "react";
import { createRoot } from "react-dom";
import TodoList from "./TodoList";

createRoot(document.getElementById("root")).render(<TodoList />);
```

### dangerouslySetInnerHTML={{ __html: item }}





dangerouslySetInnerHTML 是 React 中的一个特殊属性，用来在渲染时直接将 HTML 代码插入到节点中。它的用法如下：


```js
<div dangerouslySetInnerHTML={{ __html: '<p>Some HTML code</p>' }} />
```

dangerouslySetInnerHTML 是 React 中的一个属性，它的作用是将字符串当作 HTML 设置到组件的内部。



在使用 dangerouslySetInnerHTML 属性时，需要注意一点：由于 HTML 字符串中可能包含恶意代码，因此在使用这个属性时要小心。它只能用于渲染已经信任的内容，而不能用于渲染用户输入的内容。



为什么这么写呢？这是因为 dangerouslySetInnerHTML 属性需要传入一个包含 __html 属性的对象，而不是直接传入字符串。所以我们使用了花括号语法，在大括号内部创建了一个包含 __html 属性的对象，然后将对象传入 dangerouslySetInnerHTML 属性。



---------

## Junior version0.3.4 😂Fragment组件化搭建部分。





### 👌👌 change logs  👌👌






### ⛔️ before ⛔️
```js
import React, { Component, Fragment } from 'react';
import './style.css'

...

<Fragment>
  <div>
    <label htmlFor="insertArea">输入内容</label>
    <input
      id="insertArea"
      className="input"
      value={this.state.inputValue}
      onChange={this.handleInputChange.bind(this)}
    />
    <button onClick={this.handleBtnClick.bind(this)}>提交</button>
  </div>
  <ul>
    {this.state.list.map((item, index) => {
      return (
        <li
          key={index}
          onClick={this.handleItemDelete.bind(this, index)}
          dangerouslySetInnerHTML={{ __html: item }}
        ></li>
      );
    })}
  </ul>
</Fragment>;


...
```



### ✅ after ✅
```js
import React, { Component, Fragment } from "react";
import "./style.css";
import TodoItem from "./TodoItem";
...
<Fragment>
  <div>
    <label htmlFor="insertArea">输入内容</label>
    <input
      id="insertArea"
      className="input"
      value={this.state.inputValue}
      onChange={this.handleInputChange.bind(this)}
    />
    <button onClick={this.handleBtnClick.bind(this)}>提交</button>
  </div>
  <ul>
    {this.state.list.map((item, index) => {
      return (
        <div>
          <TodoItem
            content={item}
            index={index}
            deleteItem={this.handleItemDelete.bind(this)}
          />
        </div>
      );
    })}
  </ul>
</Fragment>;
...
```



### 加入 TodoItem组件
(另外建立的一个js文档，看做是父类TodoList的子类，父类子类可以互相通信)
```js
import React, { Component } from "react";

class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  render() {
    return <div onClick={this.handleClick}>{this.props.content}</div>;
  }

  handleClick() {
    this.props.deleteItem(this.props.index);
  }
}

export default TodoItem;
```

### this绑定
```js
import React, { Component } from "react";

class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  ...
```
在构造函数中绑定 this 的好处是，可以避免在每个方法中单独绑定 this 的开销，可以提升性能。此外，在构造函数中绑定 this 还可以帮助你在整个组件中维护一致的 this 绑定，使得代码更加清晰易读。



例如，你可以在构造函数中绑定 this，然后在组件的生命周期函数或其他方法中使用 this，而不需要每次都使用 .bind(this) 来绑定。




---------
## Junior version0.3.6 😂 组件TodoItem代码优化。







### 👌👌 change logs  👌👌





### ⛔️ before ⛔️
```js
import React, { Component } from "react";

class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  render() {
    return <div onClick={this.handleClick}>{this.props.content}</div>;
  }

  handleClick() {
    this.props.deleteItem(this.props.index);
  }
}

export default TodoItem;
```


### ✅ after ✅
```js
import React, { Component } from "react";
class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  render() {
    const { content } = this.props;
    return <div onClick={this.handleClick}>{content}</div>;
  }
  handleClick() {
    const { deleteItem, index } = this.props;
    deleteItem(index);
  }
}
export default TodoItem;
```

个人觉得使用对象解构语法可以使代码更加简洁和易读，对象解构语法允许你提取对象的属性，并将它们赋值给变量，这样就可以在组件的代码中直接使用这些变量。原方式是接访问对象属性的方式。



只是我个人喜好，两种写法都是可以的，重要的是要保证代码的可读性和可维护性。




---------

## 主体TodoList代码优化。构造函数中绑定 this


 



### ⛔️ before ⛔️
```js
class TodoList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      inputValue: '',
      list: []
    }
  }
  
  ...
```


### ✅ after ✅
```js
class TodoList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      inputValue: '',
      list: []
    }
    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleBtnClick = this.handleBtnClick.bind(this);
    this.handleItemDelete = this.handleItemDelete.bind(this);
  }
  
  ...
```



### 使用方法调用，避免JSX表达html页面显示内容的同时融和逻辑，避免代码冗长易于维护。


### ⛔️ before ⛔️
```js
<Fragment>
  <div>...
  
  <ul>
    {this.state.list.map((item, index) => {
      return (
        <div>
          <TodoItem
            content={item}
            index={index}
            deleteItem={this.handleItemDelete.bind(this)}
          />
        </div>
      );
    })}
  </ul>
</Fragment>;
```

```js
✅ after ✅

<Fragment>
  <div>...
  
  <ul>{this.getTodoItem()}</ul>
</Fragment>;
getTodoItem() {
    return this.state.list.map((item, index) => {
      return (
        <TodoItem
          key={index}
          content={item}
          index={index}
          deleteItem={this.handleItemDelete}
        />
      );
    });
  }

```


## 新版setState()使用
```js
⛔️ before ⛔️

  handleInputChange(e) {
    this.setState({
      inputValue: e.target.value
    });
  }

  handleBtnClick() {
    this.setState({
      list: [...this.state.list, this.state.inputValue],
      inputValue: ""
    });
  }

  handleItemDelete(index) {
    const list = [...this.state.list];
    list.splice(index, 1);
    this.setState({
      list: list
    });
  }
```

### ✅ after ✅
```js
  handleInputChange(e) {
    const value = e.target.value;
    this.setState(() => ({
      inputValue: value
    }));
  }

  handleBtnClick() {
    this.setState((prevState) => ({
      list: [...prevState.list, prevState.inputValue],
      inputValue: ""
    }));
  }

  handleItemDelete(index) {
    this.setState((prevState) => {
      const list = [...prevState.list];
      list.splice(index, 1);
      return { list };
    });
  }
```
prevState 等价于 this.state



新版的setState()可以接收函数
```js
    this.setState(() => ({
      inputValue: value
    }));
  
 ```
setState()如果传一个函数，这里会是一个异步的形式，我们需要事先保存inputValue的值。



这一行代码是用来获取输入框的值的。在 JavaScript 中，可以通过访问事件对象的 target 属性来获取事件触发的元素，通过访问 target 的 value 属性来获取该元素的值。



所以，在这里，我们通过 const value = e.target.value 这一行代码来获取输入框的值，并将这个值保存在组件的状态中，以便我们在组件的 render 方法中使用它。



此外，使用 const value = e.target.value 这一行代码的好处是，它使得我们能够清晰地看到我们正在获取输入框的值，而不是做其他任何操作。这有助于提高代码的可读性和可维护性。





### 父组件和子组件之间的通信方式
在 React 中，父组件和子组件之间的通信主要有两种方式：



父组件通过 props 向子组件传递数据

子组件通过调用父组件传递过来的回调函数向父组件发送消息

第一种方式，父组件通过 props 向子组件传递数据，是通过在渲染子组件的时候将数据作为子组件的 props 传递进去。例如：
```js
import React, { Component } from 'react';

class Parent extends Component {
  render() {
    return (
      <Child name="John" age={30} />
    );
  }
}

class Child extends Component {
  render() {
    const { name, age } = this.props;
    return (
      <div>
        My name is {name}, and I am {age} years old.
      </div>
    );
  }
}
```

在上面的代码中，Parent 组件通过在渲染 Child 组件的时候将 name 和 age 作为 props 传递进去，Child 组件就可以通过 this.props 访问到这些数据。





第二种方式是在父组件中通过 props 传递回调函数给子组件。子组件触发事件时，调用这个回调函数，在函数中可以更新父组件的状态。



父组件中定义回调函数：
```js
handleChildValueChange = value => {
  this.setState({ childValue: value });
};

```
在父组件的 render 方法中通过 props 传递回调函数给子组件：
```js
<ChildComponent onValueChange={this.handleChildValueChange} />
```

子组件中触发事件时调用回调函数：
```js
handleClick = () => {
  this.props.onValueChange('new value');
};
```

这种方式的优点是可以在父组件中自定义回调函数的行为，从而达到更新父组件状态的目的。缺点是需要在父组件和子组件之间传递较多的 props，如果组件层次较多，可能会比较麻烦。



One more 例子：



首先，我们有一个父组件 ParentComponent，它包含一个状态变量 message，和一个方法 updateMessage。updateMessage 方法接收一个参数，并将其设置为组件的 message 状态变量。
```js
class ParentComponent extends React.Component {
  state = {
    message: "Hello, world!"
  };

  updateMessage = (newMessage) => {
    this.setState({
      message: newMessage
    });
  };

  render() {
    return (
      <div>
        <p>{this.state.message}</p>
      </div>
    );
  }
}
```

然后，我们有一个子组件 ChildComponent，它希望能够向父组件发送消息，更新父组件的 message 状态变量。我们可以将 updateMessage 方法作为属性传递给子组件，让子组件在需要的时候调用这个方法。
```js
class ChildComponent extends React.Component {
  handleClick = () => {
    // 调用父组件传递过来的方法
    this.props.updateMessage("Hello from the child component!");
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Update message</button>
      </div>
    );
  }
}
```


---------

## Junior version0.4.2 😂 组件TodoItem设置 Prop Types 与 DefaultProps






### 👌👌 change logs  👌👌





### ⛔️ before ⛔️

```js
import React, { Component } from "react";

class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  render() {
    const { content } = this.props;
    return <div onClick={this.handleClick}>{content}</div>;
  }

  handleClick() {
    const { deleteItem, index } = this.props;
    deleteItem(index);
  }
}

export default TodoItem;
```

### ✅ after ✅
```js
import React, { Component } from "react";
import PropTypes from "prop-types";

class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  render() {
    const { content, test } = this.props;
    return (
      <div onClick={this.handleClick}>
        {test} - {content}
      </div>
    );
  }

  handleClick() {
    const { deleteItem, index } = this.props;
    deleteItem(index);
  }
}

TodoItem.propTypes = {
test: PropTypes.string.isRequired,
content: PropTypes.string,
deleteItem: PropTypes.func,
index: PropTypes.number
};

TodoItem.defaultProps = {
test: "hello world"
};

export default TodoItem;
```

PropTypes 和 DefaultProps 都是用来给组件的 props 进行类型检查和设置默认值的方法。



PropTypes 是一个用于确定 props 传递的值的类型的对象。它可以帮助开发者避免在应用中使用无效或不正确的 props，从而避免出现 bug。例如，在上面的代码中，我们使用了 PropTypes.string.isRequired 来表示 test 属性是必需的，并且必须是一个字符串。如果传递给 TodoItem 组件的 test 属性不是字符串，或者没有传递 test 属性，则会在控制台中输出一条警告信息。



DefaultProps 是一个用于设置组件 props 的默认值的对象。例如，在上面的代码中，我们使用了 DefaultProps.test = "hello world" 来为 TodoItem 组件的 test 属性设置默认值。如果在使用 TodoItem 组件时没有传递 test 属性，则组件会使用默认值 "hello world"。



通常，我们会在组件的顶部使用 PropTypes 和 DefaultProps，这样可以使得代码更加清晰易读。





### 加餐内容



PropTypes.oneOfType 可以用来声明组件的 prop 可以接受多种类型中的一种。例如，如果你的组件有一个名为 "color" 的 prop，你可以使用 PropTypes.oneOfType 来声明它可以接受字符串或者数字类型：
```js
MyComponent.propTypes = {
  color: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
  ]),
};
```

你还可以使用 PropTypes.arrayOf 来声明 prop 接受的是一个指定类型的数组，例如：
```js
MyComponent.propTypes = {
  colors: PropTypes.arrayOf(PropTypes.string),
};
```

或者使用 PropTypes.shape 来声明 prop 接受的是一个指定形状的对象，例如：
```js
import PropTypes from 'prop-types';
...

MyComponent.propTypes = {
  user: PropTypes.shape({
    name: PropTypes.string.isRequired,
    age: PropTypes.number,
  }),
};
```



### props, state and render


```js
import React, { Component } from "react";

class Test extends Component {
  // 当父组件的render函数被运行时，它的子组件的render都将被重新运行一次
  render() {
    console.log("Test render");
    return <div>{this.props.content}</div>;
  }
}

export default Test;
import React, { Component, Fragment } from "react";
import TodoItem from "./TodoItem";
import Test from "./Test";
import "./style.css";

class TodoList extends Component {
  constructor(props) {
    super(props);
    // 当组件的state或者props发生改变的时候，render函数就会重新执行
    this.state = {
      inputValue: "",
      list: []
    };
    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleBtnClick = this.handleBtnClick.bind(this);
    this.handleItemDelete = this.handleItemDelete.bind(this);
  }

  render() {...
  }

  getTodoItem() {...
  }

  handleInputChange(e) {...
  }

  handleBtnClick() {...
  }

  handleItemDelete(index) {...
  }
}

export default TodoList;
```

---------


## 虚拟DOM
```js
1. state 数据
2. JSX 模版
3. 数据 + 模版 结合，生成真实的DOM，来显示
4. state 发生改变
5. 数据 + 模版 结合，生成真实的DOM，替换原始的DOM

缺陷：
第一次生成了一个完整的DOM片段
第二次生成了一个完整的DOM片段
第二次的DOM替换第一次的DOM，非常耗性能
```
```js
1. state 数据
2. JSX 模版
3. 数据 + 模版 结合，生成真实的DOM，来显示
4. state 发生改变
5. 数据 + 模版 结合，生成真实的DOM，并不直接替换原始的DOM
6. 新的DOM（DoucumentFragment） 和原始的DOM 做比对，找差异
7. 找出input框发生了变化
8. 只用新的DOM中的input元素，替换掉老的DOM中的input元素

缺陷：
性能的提升并不明显
```
```js
1. state 数据
2. JSX 模版
3. 数据 + 模版 结合，生成真实的DOM，来显示 
<div id='abc'><span>hello world</span></div>
4. 生成虚拟DOM（虚拟DOM就是一个JS对象，用它来描述真实DOM）（损耗了性能）
['div', {id: 'abc'}, ['span', {}, 'hello world']]
5. state 发生变化
6. 数据 + 模版 生成新的虚拟DOM （极大的提升了性能）
['div', {id: 'abc'}, ['span', {}, 'bye bye']]
7. 比较原始虚拟DOM和新的虚拟DOM的区别，找到区别是span中内容（极大的提升性能）
8. 直接操作DOM，改变span中的内容
```
```js
1. state 数据
2. JSX 模版

3. 数据 + 模版 生成虚拟DOM（虚拟DOM就是一个JS对象，用它来描述真实DOM）（损耗了性能）
['div', {id: 'abc'}, ['span', {}, 'hello world']]

4. 用虚拟DOM的结构生成真实的DOM，来显示 
<div id='abc'><span>hello world</span></div>

5. state 发生变化

6. 数据 + 模版 生成新的虚拟DOM （极大的提升了性能）
['div', {id: 'abc'}, ['span', {}, 'bye bye']]

7. 比较原始虚拟DOM和新的虚拟DOM的区别，找到区别是span中内容（极大的提升性能）

8. 直接操作DOM，改变span中的内容

优点：
1. 性能提升了。
2. 它使得跨端应用得以实现。React Native
```


---------

## Junior version0.4.7 😂 TodoList 中 ref 的应用



### 👌👌 change logs  👌👌





### ⛔️ before ⛔️
```js
<Fragment>
  <div>
    <label htmlFor="insertArea">输入内容</label>
    <input
      id="insertArea"
      className="input"
      value={this.state.inputValue}
      onChange={this.handleInputChange}
    />
    <button onClick={this.handleBtnClick}>提交</button>
  </div>
  <ul>{this.getTodoItem()}</ul>
  <Test content={this.state.inputValue} />
</Fragment>;

...

handleInputChange(e) {
    const value = e.target.value;
    this.setState(() => ({
      inputValue: value
    }));
  }

  handleBtnClick() {
    this.setState((prevState) => ({
      list: [...prevState.list, prevState.inputValue],
      inputValue: ""
    }));
  }
```



### ✅ after ✅
```js
<Fragment>
  <div>
    <label htmlFor="insertArea">输入内容</label>
    <input
      id="insertArea"
      className="input"
      value={this.state.inputValue}
      onChange={this.handleInputChange}
      ref={(input) => {
        this.input = input;
      }}
    />
    <button onClick={this.handleBtnClick}>提交</button>
  </div>
  <ul
    ref={(ul) => {
      this.ul = ul;
    }}
  >
    {this.getTodoItem()}
  </ul>
</Fragment>;

...

handleInputChange() {
    const value = this.input.value;
    this.setState(() => ({
      inputValue: value
    }));
  }

  handleBtnClick() {
    this.setState(
      (prevState) => ({
        list: [...prevState.list, prevState.inputValue],
        inputValue: ""
      }),
      () => {
        console.log(this.ul.querySelectorAll("div").length);
      }
    );
  }
```

handleBtnClick(）里面的SetState是异步的，所以setState设置了第二个参数是个毁掉函数，解决异步问题，确保页面被render以后再输出clg。
```js
handleBtnClick() {
    this.setState(
      (prevState) => ({
        list: [...prevState.list, prevState.inputValue],
        inputValue: ""
      }),
      () => {
        console.log(this.ul.querySelectorAll("div").length);
      }
    );
  }
```



在第二种写法中，在调用 setState 时使用了第二个参数，该参数是一个回调函数，会在状态更新完成后调用。



这种写法的优势在于，你可以在状态更新完成后执行特定的操作，而不是在状态更新之前执行。这对于那些依赖于状态更新完成后的操作特别有用。



例如，在第二种写法中，回调函数中的代码会在 setState 完成后执行，因此可以保证在访问 this.ul.querySelectorAll("div").length 时，已经更新了状态，并且可以获取到最新的 DOM 元素。



第一种写法中，由于没有使用回调函数，所以代码会在 setState 调用之后立即执行。这意味着，在代码执行时，状态可能还未完成更新，所以在访问 this.ul.querySelectorAll("div").length 时可能无法获取到最新的 DOM 元素。




---------

## 生命周期函数




```js
import React, { Component, Fragment } from "react";
import TodoItem from "./TodoItem";
import "./style.css";

class TodoList extends Component {
  constructor(props) {
    super(props);
    // 当组件的state或者props发生改变的时候，render函数就会重新执行
    this.state = {
      inputValue: "",
      list: []
    };
    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleBtnClick = this.handleBtnClick.bind(this);
    this.handleItemDelete = this.handleItemDelete.bind(this);
  }

  // 在组件即将被挂载到页面的时刻自动执行
  componentWillMount() {
    console.log("componentWillMount");
  }

  render() {...
  }

  // 组件被挂载到页面之后，自动被执行
  componentDidMount() {
    console.log("componentDidMount");
  }

  // 组件被更新之前，他会自动被执行
  shouldComponentUpdate() {
    console.log("shouldComponentUpdate");
    return true;
  }

  // 组件被更新之前，它会自动执行，但是他在shouldComponentUpdate之后被执行，
  // 如果shouldComponentUpdate返回true它才执行
  // 如果返回false，这个函数就不会被执行了
  componentWillUpdate() {
    console.log("componentWillUpdate");
  }

  // 组件更新完成之后，他会被执行
  componentDidUpdate() {
    console.log("componentDidUpdate");
  }

  getTodoItem() {...
  }

  handleInputChange() {...
  }

  handleBtnClick() {...
  }

  handleItemDelete(index) {..
}

export default TodoList;
```

### React 的生命周期函数是指在组件的不同阶段执行的函数。下面是常用的生命周期函数的中文名称：



componentDidMount()：组件挂载完成时调用。

shouldComponentUpdate()：组件是否应该更新时调用。

componentDidUpdate()：组件更新完成时调用。

componentWillUnmount()：组件卸载前调用。

getDerivedStateFromProps()：从新的 props 更新 state 的机会。

getSnapshotBeforeUpdate()：在更新发生之前获取快照的机会。

这些生命周期函数可以帮助你在组件的不同阶段执行特定的操作，如获取数据、更新 DOM 等。



### 所有生命周期函数都可以不存在，但是必须有rende()存在





---------


## Junior version0.4.9 😂



做了不少改动了，先传上来完整的0.4.9记录一下。


```js
TodoLIst.js

import React, { Component, Fragment } from "react";
import TodoItem from "./TodoItem";
import axios from "axios";
import "./style.css";

class TodoList extends Component {
  constructor(props) {
    super(props);
    this.state = {
      inputValue: "",
      list: []
    };
    this.handleInputChange = this.handleInputChange.bind(this);
    this.handleBtnClick = this.handleBtnClick.bind(this);
    this.handleItemDelete = this.handleItemDelete.bind(this);
  }

  render() {
    return (
      <Fragment>
        <div>
          <label htmlFor="insertArea">输入内容</label>
          <input
            id="insertArea"
            className="input"
            value={this.state.inputValue}
            onChange={this.handleInputChange}
          />
          <button onClick={this.handleBtnClick}>提交</button>
        </div>
        <ul>{this.getTodoItem()}</ul>
      </Fragment>
    );
  }

  componentDidMount() {
    axios
      .get("/api/todolist")
      .then(() => {
        alert("succ");
      })
      .catch(() => {
        alert("error");
      });
  }

  getTodoItem() {
    return this.state.list.map((item, index) => {
      return (
        <TodoItem
          key={item}
          content={item}
          index={index}
          deleteItem={this.handleItemDelete}
        />
      );
    });
  }

  handleInputChange(e) {
    const value = e.target.value;
    this.setState(() => ({
      inputValue: value
    }));
  }

  handleBtnClick() {
    this.setState((prevState) => ({
      list: [...prevState.list, prevState.inputValue],
      inputValue: ""
    }));
  }

  handleItemDelete(index) {
    this.setState((prevState) => {
      const list = [...prevState.list];
      list.splice(index, 1);
      return { list };
    });
  }
}

export default TodoList;
```

TodoItem.js
```js
import React, { Component } from "react";
import PropTypes from "prop-types";

class TodoItem extends Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (nextProps.content !== this.props.content) {
      return true;
    } else {
      return false;
    }
  }

  render() {
    const { content } = this.props;
    return <div onClick={this.handleClick}>{content}</div>;
  }

  handleClick() {
    const { deleteItem, index } = this.props;
    deleteItem(index);
  }
}

TodoItem.propTypes = {
content: PropTypes.oneOfType([PropTypes.number, PropTypes.string]),
deleteItem: PropTypes.func,
index: PropTypes.number
};

export default TodoItem;
```

### 性能提升
```js
shouldComponentUpdate(nextProps, nextState) {
    if (nextProps.content !== this.props.content) {
      return true;
    } else {
      return false;
    }
  }
```
这段代码是用来控制组件是否重新渲染的生命周期函数。在组件的状态或者属性发生改变时，React 会调用组件的 render 方法重新渲染组件。但是有时候我们不希望组件重新渲染，这个时候就可以使用 shouldComponentUpdate 方法。



shouldComponentUpdate 方法接收两个参数：nextProps 和 nextState，它们分别代表组件即将要接收到的新属性和新状态。在 shouldComponentUpdate 方法中，我们可以比较 nextProps 和 this.props 以及 nextState 和 this.state 来决定组件是否需要重新渲染。



在上面的代码中，shouldComponentUpdate 方法比较了 nextProps.content 和 this.props.content，如果两者不相等，就返回 true 表示组件需要重新渲染。否则，返回 false 表示组件不需要重新渲染。



### ajax 请求 放在哪里?





Ajax 请求通常是放在组件的生命周期方法中的，例如 componentDidMount 或 componentDidUpdate 中。这样可以保证请求在组件渲染之后发出，并且可以在数据返回后更新组件的状态。



也可以使用其他方式来发出 Ajax 请求，例如使用第三方库或使用 async/await 语法。不过，无论使用哪种方式，最终都需要在组件中调用 setState 方法来更新组件的状态。



在 React 中，不建议将 Ajax 请求放在 render 方法中，因为这样会导致每次组件更新都会发出一个新的请求，这样会严重影响组件的性能。因此，最好将 Ajax 请求放在组件的生命周期方法中，或者使用一些优化技术，例如使用 shouldComponentUpdate 来减少不必要的请求。





### 调试工具 补充Charles 和 Postman





Charles 和 Postman 是两种常用的接口调试工具，用于模拟客户端发送 HTTP 请求并查看服务器的响应。



Charles 是一款专业的 HTTP 代理调试软件，可以拦截客户端发送的 HTTP 请求并修改请求内容，也可以拦截服务器的响应并修改响应内容。它通常用于调试网络协议、调试 Web 应用、抓取网站数据等。



Postman 是一款轻量级的 HTTP 客户端，可以方便地发送 HTTP 请求并查看响应。它提供了丰富的功能，如管理请求历史、自动生成代码等，使得它成为许多开发者和测试人员的首选工具。





---------

## Junior version0.4.10 😂 TodoList 中 ajax ，axios的应用





### 👌👌 change logs  👌👌






### ⛔️ before ⛔️
```js
  componentDidMount() {
    axios
      .get("/api/todolist")
      .then(() => {
        alert("succ");
      })
      .catch(() => {
        alert("error");
      });
  }
```



### ✅ after ✅
```js
  componentDidMount() {
    axios
      .get("/api/todolist")
      .then((res) => {
        this.setState(() => ({
          list: [...res.data]
        }));
      })
      .catch(() => {
        alert("error");
      });
  }
```

在 componentDidMount 生命周期函数中发送 ajax 请求是一种常见的做法。componentDidMount 是在组件已经挂载到 DOM 上后执行的函数，所以在这里发送 ajax 请求是一个好的选择，因为在组件挂载到 DOM 后才会发送请求，避免了发送请求的浪费。另外，在 componentDidMount 中发送 ajax 请求，也可以保证请求完成后，组件的状态已经被正确地设置。
  
last edited 30 Noc 2023   
last edited 6 Dec 2023 
last edited 13 Dec 2023   
last edited 16 Dec 2023
last edited 19 Dec 2023 
last edited 23 Dec 2023 
last edited 1 Jan 2023 
last edited 4 Jan 2023 


---------
## 插入补充零碎知识点

---------

<Fragment>是React中的一个组件，所以其名称需要首字母大写。这是React中的命名约定，所有的组件名都需要首字母大写。这样可以让React知道这是一个组件，而不是普通的标签。



React 中使用 className 代替 class 是因为 class 是 JavaScript 中的保留字，与原有的 class 关键字冲突。所以在 React 中使用 className 属性来表示 HTML 元素的 class 属性。


---------


在 React 中，你需要使用 className 属性来添加 CSS 类：



使用 className 与使用 class 没有任何区别，你可以通过 className 属性为元素添加多个 CSS 类，并使用空格分隔：
```js
<div className="container blue-background">Hello</div>
```

---------

在 React 中，我们使用 htmlFor 来替代 for 属性。这是因为 for 是 JavaScript 的关键字，在 JSX 中会导致语法错误。



例如，在传统的 HTML 中，我们可能会这样写一个表单：
```js
<label for="email">Email</label>
<input type="email" id="email" />
```

在 React 中，我们使用 htmlFor 来替代 for：
```js
<label htmlFor="email">Email</label>
<input type="email" id="email" />
```
这样就可以避免 for 这个关键字引起的问题，同时使得我们的代码更加符合 JSX 的语法规范。


---------


export default 和 export 区别



在 JavaScript 中，使用 export 关键字可以将一个模块中的内容（函数、变量、对象等）导出，以便在其他文件中引用。



export default 是 JavaScript 的一个语法糖，它允许你在模块中指定一个默认的导出。当使用 export default 导出时，在其他文件中引用时，不需要使用大括号。



例如，在模块中使用 export default 导出一个函数：
```js
export default function add(x, y) {
  return x + y;
}
```

在其他文件中引用时，可以直接使用函数名来引用：
```js
import add from './add';

console.log(add(1, 2)); // 3
```

如果你使用 export 而不是 export default 导出函数，那么在其他文件中引用时需要使用大括号：
```js
export function add(x, y) {
  return x + y;
}
import { add } from './add';

console.log(add(1, 2)); // 3
```
对于一个模块（文件）中的内容，使用 export 关键字可以将内容导出，使得其他文件可以使用 import 关键字来引入这些内容。使用 export default 关键字可以将模块中的一个内容默认导出，其他文件可以使用任意名称来引入这个内容。



举个例子，在一个文件 math.js 中有如下内容：
```js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export default function multiply(a, b) {
  return a * b;
}

```
在另一个文件中，我们可以这样引入这些内容：
```js
import { add, subtract } from "./math";
import multiply from "./math";

console.log(add(1, 2)); // 3
console.log(subtract(1, 2)); // -1
console.log(multiply(2, 3)); // 6
```

区别就是，使用 export 导出的内容，在其他文件中必须使用大括号将它们括起来，并且使用原来的名称来引入。使用 export default 导出的内容，在其他文件中可以使用任意名称来引入。



---------

原生应用

在 React 开发中，"原生应用" 指的是使用 React Native 开发的移动应用，而不是使用浏览器中的 HTML、CSS 和 JavaScript 开发的 Web 应用。



React Native 是一个开源框架，可以让你使用 JavaScript 和 React 来开发移动应用，而无需使用原生代码（如 Java 或 Swift）。它的目的是让你能够使用一套代码来开发多个平台（如 iOS 和 Android）的应用，而不需要为每个平台编写单独的代码。



与传统的 Web 应用不同，原生应用可以直接与设备的硬件和操作系统交互，并可以使用本地的 API 和功能（如访问相机、位置服务等）。由于它们是原生的，所以也更加流畅和自然，体验更好。









