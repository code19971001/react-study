### React基础

> React是由Facebook于2013年开源，专注与构建用户界面的JS库。

#### 为什么要学习React？

原生JS的痛点?

- 原生JS操作DOM比较繁琐，效率低(DOM-API操作UI)。
- 使用JS直接操作DOM，浏览器会进行大量的重绘重排。
- 原生JS没有组件化编码方案，代码的复用率低。

React的优点有哪些？

- React采用组件化模式，声明式编码，提高开发的效率和组件复用率。
- 在React Native中可以使用React语法进行移动端开发。
- 使用虚拟DOM+优秀的Diffing算法，尽量减少与真实DOM的交互。当数据发生变化，但是如果有其对应的虚拟DOM，还可以进行复用。

#### Get Start

> https://reactjs.org/

##### 创建DOM

1.使用JSX的方式--推荐

```js
const VDOM = <h1>Hello, React</h1>
```

2.使用JS的方式--不推荐使用

```htm
const vDom02 = React.createElement('h1',{id:'title'},'Hello,React')
```

##### 渲染

```js
ReactDOM.render(vDom02,document.getElementById("hello"))
```

##### JSX语法

> 使用类似于XML的方式描述

JSX语法规则：

- 定义虚拟DOM的时候不要写引号。
- 标签中混入js表达式的时候使用{ }包裹起来。
- 样式的类名指定不要使用class，要使用className.
- 内联样式需要使用style={{key:value}}的形式来写。
- 虚拟DOM不能有多个根标签，根标签只能有一个。
- 标签必须闭合。
- 标签中的标签首字母。
  - 如果是小写字母开头，则将该标签转为html中同名标签，如果html中无此标签对应的同名元素，则报错。The tag <good> is unrecognized in this browser
  - 如果是大写开头，则将该标签认为该标签为react组件，那么就去渲染指定组件，如果组件没有定义则报错。Uncaught ReferenceError: Good is not defined

```html
  <div id="test"></div>
  <script type="text/babel">
    const myId="test001";
    const myData='Hello,react';
    //1.创建虚拟DOM
    const vDOM = (
      <div>
        <h2 className="title" id={myId.toLowerCase()}>
          <span style={{color:'red',fontSize:'30px'}}>{myData.toLowerCase()}</span>
        </h2>
        <input type='text' name='username' placeholder='请输入用户名'/>
        <good>good good study</good>
      </div>
    );
    //2.渲染虚拟DOM
    ReactDOM.render(
      vDOM,
      document.getElementById('test')
    )
  </script>
```

小练习

使用JSX语法实现数据的动态展示

```html
<body>
  <div id="test"></div>
  <script type="text/babel">
  /**
   * {这里可以写js表达式，但是不可以写js语句}
   * 一定要区分：【js语句(代码)】与【js表达式】
   *    1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方：
   *      1）a
   *      2）a+b
   *      3）demo(1)
   *      4）arr.map()
   *      5）function test(){}
   *    2.语句或者代码
   *      1）if(){}
   *      2）for()
   *      3）switch(){ }
   *         
   */
    //模拟一些数据：
    const data = ['Angular','React','Vue'];
    const data1= {'name1':'Angular','name2':'React','name3':'Vue'}
    //1.创建一个虚拟dom
    //写死的方式实现效果。
    const vDOM = (
      <div>
        <h2>前端js框架列表</h2>  
        <ul>
          <li>Angular</li>
          <li>React</li>
          <li>Vue</li>  
        </ul>
      </div>
    )
    //2.使用map来进行遍历。注意：key是一个唯一标识。
    const vDOM2 = (
      <div>
        <h2>前端JS框架列表</h2>
        <ul>
          {
            data.map((name,index) => <li key={index}>{name}</li>
            )
          }
        </ul>  
      </div>
    )


    //2.渲染虚拟dom到页面
    ReactDOM.render(
      vDOM2,
      document.getElementById('test')
    );

  </script>

</body>
```

##### 模块化和组件化

> 模块化和组件化都是

怎么理解模块？拆js

- 理解：向外提供特定功能的js程序，一般是一个js文件。
- 为什么要拆：随着业务逻辑的等级，代码越来越多且复杂。
- 作用：代码复用，简化js编写，提高运行效率。
- 模块化：代码以模块的方式实现，那就是模块化项目。

怎么理解组件？

- 理解：用来实现局部功能效果的代码和资源的集合(html/cs/js/image等)
- 为什么：一个界面的功能比较复杂，将页面拆分，可以简化复杂度。比如header，leftMenu。
- 作用：复用编码，简化项目编码，提供运行效率。
- 组件化：代码以组件的方式实现，那就是组件化项目。

##### 定义组件

函数式组件：适用于简单组件，无状态

```js
  <script type="text/babel">
    
    //1.创建函数式组件：函数的首字母要大写
    function Demo(){
      //此处的this是undefined,因为babel编译之后开启了严格模式。
      console.log(this);
      //必须要有返回值，最低标准是要有骨架
      return <h2>我是用函数定义的组件(适用于简单组件的定义)</h2>
    }
    //2.渲染组件到页面:标签需要自闭和。
    ReactDOM.render(
      <Demo />,
      document.getElementById('test')
    )

    /**
      *执行ReactDOM.render之后，会发生什么？
      * 1.React解析组件标签，找到Demo组件。
      * 2.发现组件时使用函数定义的，然后调用该函数，
      *    将返回的虚拟DOM转换为真实的DOM，随后呈现在页面总。
      */
  
  </script>
```

类式组件：适用于复杂组件，有状态

```js
  <script type="text/babel">
    //1.创建一个类式组件：
    //1）继承react中的一个内置类:React.Component。
    //2）写一个render方法。
    //3）render方法需要有返回值。
    class Demo extends React.Component{

      //位于类的原型对象上，供实例来进行调用。
      render(){  
        //Demo {props: {…}, context: {…}, refs: {…}, 
        //updater: {…}, _reactInternalFiber: FiberNode, …}
        //this是什么？？Demo的实例对象<==>Demo组件实例对象。
        console.log(this);
        return (
          <h1>我是用类定义的组件(使用与[复杂组件]的定义)</h1>
        )
      }
    }
    //2.渲染组件
    ReactDOM.render(
      <Demo/>,
      document.getElementById('test')
    );
    /**
     * 执行ReactDOM.render()之后发生了什么？
     * 1）React解析组件标签。找到MyComponent组件。
     * 2）发现组件是使用类定义的，随后new出该类的实例，并通过调用该类的组件
     * 的render方法。
     * 3）将render返回的虚拟DOM转换为真实的DOM，随后呈现在页面中。
     * 
     */
```

##### 组件实例三大属性之state

> state是组件对象最重要的属性，值是对象(可以包含多个key-value的组合)，驱动着页面的改变，又被称为“状态机”，通过更新组件的state来更新对应的页面显示(重新渲染组件)。

标准写法：为了初始化state，不得不借用构造方法，比较被动。

```js
<body>
  <div id="test"></div>

  <script type="text/babel">
    //1.创建组件:借助构造器初始化state。
    class Weather extends React.Component {
      //可以省略掉，但是如果存在状态，则需要我们使用构造器
      constructor(props) {
        //如果我们传入参数，那么就必须要调用super
        super(props);
        this.state = { isHot: true, wind:'微风' }
        //bind会返回一个新的函数，并且更改了其中this的指向
        this.changeWeather = this.changeWeather.bind(this)
      }

      //
      changeWeather() {
        console.log('标题被点击了！'+this.state);
        //alert();
        var isHot = this.state.isHot
        //通过Weather实例调用changeWeather时，changeWeather中的this才是Weather实例。
        //注意：
        //1.状态不可以直接更改，直接更改不会驱动React的渲染，需要借助React的内置API setState进行更改,setState位于React.Compnent原型对象上。
        //2.更新是一种合并而不是替换。
        //3.构造器只会执行一次；Render会调用n+1次，1代表第一次，n为状态更新的次数。
        //this.state.isHot = !isHot;
        this.setState({isHot:!isHot})
        console.log(this.state.isHot)
      }


      /**
       * 对于React来说，事件都被改成小驼峰。而且方法的调用一定不能加{}； 
       * changeWeather放在Weather的原型对象上，供实例使用,所以我们使用的时候需要使用对象this来使用。
       * 类中默认开启了局部的严格模式，所以直接调用会变成undefined;没有严格模式的情况下是window；对象调用this是调用的对象。
       */
      render() {
        console.log("render"+this);
        const { isHot,wind } = this.state;
        return (
          //这里算是直接调用，不是实例调用，又因为位于类中，所以有局部严格模式，所以this是undefined
          <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'},{wind}</h1>
        )
      }
    }

    //渲染组件
    ReactDOM.render(
      <Weather />,
      document.getElementById('test')
    );
 
    //方式1：
    // const title=document.getElementById('title');
    // title.addEventListener('click',()=>{
    //   console.log('标题被点击了！');
    // })
   
    /*
   //方式2：
    const title=document.getElementById('title');
    title.onclick=()=>{
      console.log('标题被点击了！');
    }
    */

  </script>

</body>
```

精简写法：赋值语句+箭头函数，如果我们想用初始化，可以直接在类中使用该语法。赋值语句会在当前对象上添加一个该属性。

```js
<body>
  <div id="test"></div>

  <script type="text/babel">
    class Weather extends React.Component {


      //向对象Weather上添加一个属性state，并进行初始化
      state={ isHot: true, wind:'微风' }

      //箭头函数：使用外层函数的this作为自己的this
      changeWeather = () => {
        console.log("this is:"+this)
        console.log('标题被点击了！'+this.state);
        var isHot = this.state.isHot
        this.setState({isHot:!isHot})
        console.log(this.state.isHot)
      }

      render() {
        console.log("render"+this);
        const { isHot,wind } = this.state;
        return (
          //这里算是直接调用，不是实例调用，又因为位于类中，所以有局部严格模式，所以this是undefined
          <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'},{wind}</h1>
        )
      }
    }

    //渲染组件
    ReactDOM.render(
      <Weather />,
      document.getElementById('test')
    );

  </script>

</body>
```

总结：

- state是组件对象最重要的一个属性，值是对象(可以包含多个key-value的组合)。
- 组件被称为“状态机”，通过更新组件的state来更新对应的页面显示(重新渲染组件)。
- 组件render()中的this是该组件实例对象。
- 组件中自定义的方法中的this为undefined，如何解决？
  - 在构造函数中使用函数对象的bind()来强制绑定this。
  - 利用箭头函数的特性。
- 状态数据不能直接修改或更新，需要借助React对象上的setState()来更新。

##### 组件三大属性之props

> 可以通过外部给组件传递属性，props会收集我们传递的参数。

```js
<body>

  <div id="text"></div>

  <script type="text/babel">

    class Person extends React.Component {

      /*三个问题：给类加上propTyps来进行限制。react15版本挂载在React上；react15.5被拆分到propTypes包中，会额外因为全局的PropTypes对象
       *1.需要对传入的类型进行限制：
       *2.需要对属性必要性进行限制：
       *3.需要对属性进行赋默认值：
      */
      static propTypes = {
        name: PropTypes.string.isRequired,
        sex: PropTypes.string,
        age: PropTypes.number,
        spark: PropTypes.func
      }
      static defaultProps = {
        sex: '人妖',
        age: 18
      }

      render() {
        //react会将我们写的属性收集起来放到props中
        const { name, sex, age } = this.props
        console.log(this);
        return (
          <ul>
            <li>姓名：{name}</li>
            <li>性别：{sex}</li>
            <li>年龄：{age + 1}</li>
          </ul>
        )
      }
    }

    const p = { name: 'tom', sex: '男', age: 18 }
    //ReactDOM.render(<Person name={p.name} sex = {p.sex} age= {p.age} />, document.getElementById("text"))
    //...是展开运算符：可以用于展开数组；合并数组[...arr1,...arr2]；可以用作批量传输参数。
    //在原生的js中，因为使用...是引用的传递，所以不能用来展开对象。我们可以使用{...p}来构建字面量对象时使用的语法，这是合法的新语法。
    //{}是react中的分隔符，在React+Babel中允许使用...来展开对象。
    function spark() {
      console.log("this is speak" + this);
    }
    ReactDOM.render(<Person {...p} spark={spark} />, document.getElementById("text"))
  </script>

</body>
```

总结：

- 每个组件对象都会有props属性

- 组件标签的所有属性都保存在props中

- Props传入组件的值是只读的，不可修改。

- PropTypes:对标签属性进行类型和必要性的限制。

  - PropTypes.string：指定类型。
  - PropTypes.isRequired：指定必填项。
  - Person.defaultProps来指定prop属性的默认值。
  - spark:PropTypes.func：限制传入的spark的值必须为函数。

  ```js
  <script src="https://cdn.bootcdn.net/ajax/libs/prop-types/15.8.1/prop-types.js"></script>   
  Person.propTypes = {
      name:PropTypes.string.isRequired,//name必须传入并且为string
      sex:PropTypes.string,
      age:PropTypes.number,
      spark:PropTypes.func：//spark必须为函数
  }
  Person.defaultProps = {
      sex:'人妖',
      age:18
  }
  ```

- 构造器的作用：

  - 给this.state赋值对象来初始化内部state。可以直接写在类中来代替
  - 为事件处理函数绑定实例：标准state写法，使用bind绑定this。可以使用赋值语句+箭头函数来代替。

- 我们在构造器中是否接收props，是否传递给super，取决于我们是否需要在构造器中调用this.props，如果不接，不传，那么我们使用this.props会出现undefined错误。**这种情况几乎用不到**，因此，类中的构造器，能不写就不写。

- 函数式组件是可以使用props的：尽管没有实例，但是存在参数

  ```js
  <body>
  
    <div id="text"></div>
  
    <script type="text/babel">
  
      function Person(props) {
        //react会将我们写的属性收集起来放到props中
        const { name, sex, age } = props
        console.log(this);
        return (
          <ul>
            <li>姓名：{name}</li>
            <li>性别：{sex}</li>
            <li>年龄：{age + 1}</li>
          </ul>
        )
      }
      
      Person.propTypes = {
        name: PropTypes.string.isRequired,
        sex: PropTypes.string,
        age: PropTypes.number,
        spark: PropTypes.func
      }
      Person.defaultProps = {
        sex: '人妖',
        age: 18
      }
  
      const p = { name: 'tom', sex: '男' }
      function spark() {
        console.log("this is speak" + this);
      }
      ReactDOM.render(<Person {...p} spark={spark} />, document.getElementById("text"))
    </script>
  
  </body>
  ```

##### 组件实例的三大属性之refs

> 类似于html标签中的id，ref主要用于打标签。react会自动将标签中的ref属性收集起来放到当前对象的refs属性中。

使用方式1：string的方式

```js
  <script type="text/babel">

    class Demo extends React.Component {
      showData = ()=>{
        const {input1} = this
        console.log(input1);
      }
      showData2 = ()=>{
        const {input2} = this.refs
        console.log(input2);
      }

      render() {

        return (
          <div>
              
              <input ref={currentNode => this.input1 = currentNode} type="text" placeholder="点击按钮提示数据"/>&nbsp;
              <button onClick={this.showData}>点击提示左侧按钮</button>&nbsp;
              <input onBlur={this.showData2} ref="input2" type="text" placeholder="失去焦点提示数据"/>
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
```

使用方式2：回调的方式，将当前节点传入，并挂在在当前对象的input1属性上；

```js
showData = ()=>{
    const {input1} = this
    console.log(input1);
}

<input ref={currentNode => this.input1 = currentNode} type="text" placeholder="点击按钮提示数据"/>&nbsp;
```

![image-20230222233851120](https://blog-images-code1997.oss-cn-hangzhou.aliyuncs.com/2022-11/image-20230222233851120.png)

执行次数：如果ref回调函数是以内联函数的方式定义的，在**更新过程**中他会被执行两次，第一次传入参数为null，第二次会传入参数DOM元素。这是因为他在每次渲染时会创建一个新的函数实例，所以React清空旧的ref并且设置新的。通过将ref的回调函数定义为class的绑定函数可以避免以上问题。但是一般情况下是无关紧要的，因此也可以直接使用回调的方式。

```js
saveInput = currentNode=>{
    this.input3 = currentNode;
}

<input onBlur={this.showData2} ref={this.saveInput} type="text" placeholder="失去焦点提示数据"/>
```

使用方式3：使用createRef()，专人专用，也是React推荐的一种方式。

```js
<body>
  <div id="test"></div>
  <script type="text/babel">

    class Demo extends React.Component {

      //React.createRef()调用后可以返回一个容器，该容器中可以存储被ref所标识的节点
      myRef = React.createRef() 
    
      showData3 = ()=>{
        console.log(this);
        console.log(this.myRef);
      }

      render() {
        return (
          <div>
              {/*这是注释的写法*/}
              <input onBlur={this.showData3} ref={this.myRef} type="text" placeholder="失去焦点提示数据"/>
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
}
```

总结：

- 条件允许的情况下尽量避免使用字符串形式的ref


