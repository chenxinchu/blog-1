# javascript执行上下文
当javascript执行一段代码时，会创建对应的执行上下文<br/>

对于每个执行上下文，都有三个重要属性：<br/>
1. 变量对象(Variable object，VO)
2. 作用域链(Scope chain)
3. this

## 变量对象
变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明

### 全局上下文中变量对象
全局上下文中的变量对象就是全局对象<br/>
全局对象是由 Object 构造函数实例化的一个对象,因此它预定义了一大堆函数和属性，例如使用Math.max(),其实就是使用了全局对象。<br/>
全局对象可以使用this引用，在客户端javascript中以window引用。


### 函数上下文中变量对象
1. 函数上下文的变量对象初始化只包括 Arguments 对象

2. 在进入执行上下文时会给变量对象添加形参、函数声明、变量声明等初始的属性值

3. 在代码执行阶段，会再次修改变量对象的属性值

## javascript 作用域链
当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链

### 函数创建
函数有一个内部属性 [[scope]]，当函数创建的时候，就会保存所有父变量对象到其中，你可以理解 [[scope]] 就是所有父变量对象的层级链，但是注意：[[scope]] 并不代表完整的作用域链！<br/>
举个例子：
```js
function foo() {
    function bar() {
        ...
    }
}
```
函数创建时，各自的[[scope]]为：
```js
foo.[[scope]] = [
  globalContext.VO
];

bar.[[scope]] = [
    fooContext.AO,
    globalContext.VO
];
```

### 函数激活
当函数激活时，进入函数上下文，创建 VO/AO 后，就会将活动对象添加到作用链的前端。<br/>

这时候执行上下文的作用域链，我们命名为 Scope：<br/>

```Scope = [AO].concat([[Scope]]);```
至此，作用域链创建完毕。

## 执行上下文栈