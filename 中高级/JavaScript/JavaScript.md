# JavaScript

本部分是在工作中需要注意的一些关于JavaScript相关比较高级的题目，如果出现错误，希望大家指出！

### 目录

* [1. 什么是堆？什么是栈？它们之间有什么区别和联系？](#1-什么是堆什么是栈它们之间有什么区别和联系)
* [2. 内部属性Class是什么？](#2-内部属性-Class-是什么)
* [3. Javascript 中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？](#3-Javascript -中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是)
* [4. redux、react-redux、vuex的原理？](#4-redux、react-redux、vuex的原理？)



#### 1. 什么是堆？什么是栈？它们之间有什么区别和联系？

```
堆和栈的概念存在于数据结构中和操作系统内存中。

在数据结构中，栈中数据的存取方式为先进后出。而堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。完全
二叉树是堆的一种实现方式。

在操作系统中，内存被分为栈区和堆区。

栈区内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

堆区内存一般由程序员分配释放，若程序员不释放，程序结束时可能由垃圾回收机制回收。
```

#### 2. 内部属性 [[Class]] 是什么？

```
所有 typeof 返回值为 "object" 的对象（如数组）都包含一个内部属性 [[Class]]（我们可以把它看作一个内部的分类，而非
传统的面向对象意义上的类）。这个属性无法直接访问，一般通过 Object.prototype.toString(..) 来查看。例如：

Object.prototype.toString.call( [1,2,3] );
// "[object Array]"

Object.prototype.toString.call( /regex-literal/i );
// "[object RegExp]"

```

#### 3. Javascript 中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

```
hasOwnProperty

所有继承了 Object 的对象都会继承到 hasOwnProperty 方法。这个方法可以用来检测一个对象是否含有特定的自身属性，和
in 运算符不同，该方法会忽略掉那些从原型链上继承到的属性。
```

#### 4. redux、react-redux、vuex的原理？

背景：不同组件需要共享一些数据，之前只能把数据提升到高层级组件，通过props传递，react还可以使用context，vue还能使用$attrs，但context可以随意修改数据，这样会导致很难定位数据问题，$attrs也只能在父子组件，不能兄弟之间，而且使用也很麻烦。

思路：

1. 增加数据修改的复杂度，增加一个中间层，所有修改数据都得通过dispatch来进行修改，所有数据都存储在state对象里面。
2. 为了通用性，封装了生成store的函数createStore，vue通过实例化Vuex.Store来获取store
3. store实例里面添加了dispatch、getState、subscribe三个方法，dispatch用来修改数据，getState用来获取数据，subscribe用来监听数据变化。实际修改state的部分叫reducer，这个是一个纯函数，传入action，根据action的一个唯一标识来修改state，最近实践是通过一个函数来产生这个action对象，调用dispatch传入action。
4. redux是一个框架，可以应用于任何地方，react-redux主要是为了方便react使用，结合context来传递store，把所有与store进行交互的逻辑全部提取到container组件，通过connect来传递props和state的映射关系，传递修改state的逻辑与dispatch的对应关系。
5. vuex也是通过dispatch来修改state，异步的修改通过传入action，同步通过mutation来修改，最终还是统一通过mutation来修改state，模块多了之后可以通过modules来组织代码，每个module里面都有state、mutation、action。