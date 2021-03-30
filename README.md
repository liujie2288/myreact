# 搭建自己的react

> 通过[文章](https://pomb.us/build-your-own-react/)解读，了解React运行机制


## 回顾

一个React引用程序最简代码，如下：

```jsx
  const element = <h1 title="foo">hello</h1>;
  const container = document.getElementById('root');
  ReactDOM.render(element,container);
```

`element`变量对应的值是一个jsx语法，它不是javascript语法，不能被浏览器识别，需要经过babel编译之后，转换成React函数`React.createElemnt`的调用，如：

```javascript
  React.createElement('h1',{
    title:'foo',
  },'hello')
```

该函数将会返回一个对象，我们可以称为“元素”。该对象用于表示一个元素的结构，比如元素具体的类型，元素有哪里属性，元素的子节点是什么等等。

```javascript
  const element = {
    type:'h1',
    props: {
      title:'foo',
      children:'hello', // children通常是一个包含更多元素的数组，因此更像一个树形结构。可以很方便的来表达HTML树结构
    }
  }
```

该对象也就是平时所说的`virtual dom`，它不是真正的DOM对象，而是通过原生的Javascript对象来表达一个DOM对象。

有了这个JS对象，那怎么把它转换成真正的DOM并插入到DOM中呢？`ReactDOM.render`就发挥了魔力。

首先，ReactDOM内部会创建一个和元素对应的DOM节点。

```javascript
  const node = document.createElement(element.type)
```

