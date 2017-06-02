1 . `请解释事件代理 (event delegation)`

    > 当需要对很多元素添加事件的时，可以通过将事件添加到它们的父节点通过委托来触发处理函数。其中利用到了浏览器的事件冒泡机制。

    ```
    var delegate = function(client, clientMethod) {
      return function() {
          return clientMethod.apply(client, arguments);
      }
    }

    var agentMethod = delegate (client, clientMethod);
    agentMethod();


    // 获取父节点，并为它添加一个click事件
    document.getElementById("parent-list").addEventListener("click",function(e) {
      // 检查事件源e.targe是否为Li
      if(e.target && e.target.nodeName.toUpperCase == "LI") {
        // 真正的处理过程在这里
        console.log("List item ",e.target.id.replace("post-")," was clicked!");
      }
    });

    ```



***

2 . `谈谈浏览器的事件冒泡机制`

    > 对于事件的捕获和处理，不同的浏览器厂商有不同的处理机制，我们以W3C对DOM2.0定义的标准事件为例
    DOM2.0模型将事件处理流程分为三个阶段：一、事件捕获阶段，二、事件目标阶段，三、事件起泡阶段。

    - 事件捕获：当某个元素触发某个事件（如onclick），顶层对象document就会发出一个事件流，随着DOM树的节点向目标元素节点流去，直到到达事件真正发生的目标元素。在这个过程中，事件相应的监听函数是不会被触发的。

    - 事件目标：当到达目标元素之后，执行目标元素该事件相应的处理函数。如果没有绑定监听函数，那就不执行。

    - 事件起泡：从目标元素开始，往顶层元素传播。途中如果有节点绑定了相应的事件处理函数，这些函数都会被一次触发。如果想阻止事件起泡，可以使用e.stopPropagation()（Firefox）或者e.cancelBubble=true（IE）来组织事件的冒泡传播。

***

3 . `JavaScript 中 this 是如何工作的。`

    - 作为函数调用，this 绑定全局对象，浏览器环境全局对象为 window 。
    - 内部函数的 this 也绑定全局对象，应该绑定到其外层函数对应的对象上，这是 JavaScript的缺陷，用that替换。
    - 作为构造函数使用，this 绑定到新创建的对象。
    - 作为对象方法使用，this 绑定到该对象。
    - 使用apply或call调用 this 将会被显式设置为函数调用的第一个参数。

***

4 . `谈谈CommonJs 、AMD  和CMD `

    > CommonJS规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域            CommonJS的使用代表：NodeJS

    > AMD 即Asynchronous Module Definition 异步模块定义 它是一个在浏览器端模块化开发的规范 AMD 是 RequireJS 在推广过程中对模块定义的规范化的产出

    > CMD 即Common Module Definition  通用模块定义 其代表为SeaJS

    requireJS主要解决两个问题

    - 多个js文件可能有依赖关系，被依赖的文件需要早于依赖它的文件加载到浏览器
    - js加载的时候浏览器会停止页面渲染，加载文件越多，页面失去响应时间越长

    CMD和AMD的区别

    - AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块
    - CMD推崇就近依赖，只有在用到某个模块的时候再去require

***

5 . `谈谈对IIFE的理解`

    > IIFE即Immediately-Invoked Function Expression 立即执行函数表达式

    不推荐
    ```
    (function(){})();
    ```
    推荐
    ```
    (function(){}());
    ```
    在javascript里，括号内部不能包含语句，当解析器对代码进行解释的时候，先碰到了()，然后碰到function关键字就会自动将()里面的代码识别为函数表达式而不是函数声明。

    知识拓展：

    ```
    function(){ /* code */ }();  解释下该代码能正确执行吗？
    ```
    不行，在javascript代码解释时，当遇到function关键字时，会默认把它当做是一个函数声明，而不是函数表达式，如果没有把它显视地表达成函数表达式，就报错了，因为函数声明需要一个函数名，而上面的代码中函数没有函数名。（以上代码，也正是在执行到第一个左括号(时报错，因为(前理论上是应该有个函数名的。）

    ```
    function foo(){ /* code */ }();  解释下该代码能正确执行吗？
    ```

    在一个表达式后面加上括号，表示该表达式立即执行；而如果是在一个语句后面加上括号，该括号完全和之前的语句无法匹配，而只是一个分组操作符，用来控制运算中的优先级（小括号里的先运算）相当于先声明了一个叫foo的函数，之后进行()内的表达式运算，但是()（分组操作符）内的表达式不能为空，所以报错。（以上代码，也就是执行到右括号时，发现表达式为空，所以报错）。

***

6 . `.call 和 .apply 的区别是什么？`

    > foo.call(this, arg1,arg2,arg3) == foo.apply(this, arguments)==this.foo(arg1, arg2, arg3)

    call, apply方法区别是,从第二个参数起, call方法参数将依次传递给借用的方法作参数, 而apply直接将这些参数放到一个数组中再传递, 最后借用方法的参数列表是一样的.

***

7 . `请解释 Function.prototype.bind？`

bind() 方法的主要作用就是将函数绑定至某个对象，bind() 方法会创建一个函数，函数体内this对象的值会被绑定到传入bind() 函数的值。

原理
```
Function.prototype.bind = function(context) {
 var self = this; // 保存原函数
 return function() { // 返回一个新函数
  return self.apply(context, arguments); // 执行新函数时，将传入的上下文context作为新函数的this
 }
}
```

用法：
```
var paint = {
 color: "red",
 count: 0,
 updateCount: function() {
  this.count++;
  console.log(this.count);
 }
};

// 事件处理函数绑定的错误方法：
document.querySelector('button')
 .addEventListener('click', paint.updateCount); // paint.updateCount函数的this指向变成了该DOM对象

// 事件处理函数绑定的正确方法：
document.querySelector('button')
 .addEventListener('click', paint.updateCount.bind(paint)); // paint.updateCount函数的this指向变成了paint
```

***

8 . `请解释原型继承 (prototypal inheritance) 的原理。`

    > 当查找一个对象的属性时，JavaScript 会向上遍历原型链，直到找到给定名称的属性为止。——出自JavaScript秘密花园

    JavaScript中的每个对象，都有一个内置的 _proto_ 属性。这个属性是编程不可见的（虽然ES6标准中开放了这个属性，然而浏览器对这个属性的可见性的支持不同），它实际上是对另一个对象或者 null 的引用。

    当一个对象需要引用一个属性时，JavaScript引擎首先会从这个对象自身的属性表中寻找这个属性标识，如果找到则进行相应读写操作，若没有在自身的属性表中找到，则在 _proto_ 属性引用的对象的属性表中查找，如此往复，直到找到这个属性或者 _proto_ 属性指向 null 为止。

    以下代码展示了JS引擎如何查找属性：
    ```
    //__proto__ 是一个不应在你代码中出现的非正规的用法，这里仅仅用它来解释JavaScript原型继承的工作原理。
    function getProperty(obj, prop) {
    if (obj.hasOwnProperty(prop))
        return obj[prop]
    else if (obj.__proto__ !== null)
        return getProperty(obj.__proto__, prop)
    else
        return undefined
    }
    ```

    JS的ECMA规范只允许我们采用 new 运算符来进行原型继承

    原型继承

    ```
    function Point(x, y) {
    this.x = x;
    this.y = y;
    }
    Point.prototype = {
        print: function () { console.log(this.x, this.y); }
    };

    var p = new Point(10, 20);
    p.print(); // 10 20
    ```

    顺便阐述下new 运算符是如何工作的?

    - **创建类的实例**。这步是把一个空的对象的 __proto__ 属性设置为 F.prototype 。
    - **初始化实例**。函数 F 被传入参数并调用，关键字 this 被设定为该实例。
    - **返回实例**。

    ```
    function New (f) {
        var n = { '__proto__': f.prototype }; /*第一步*/
        return function () {
            f.apply(n, arguments);            /*第二步*/
            return n;                         /*第三步*/
        };
    }
    ```

    JavaScript中真正的原型继承

    ```
    Object.create = function (parent) {
        function F() {}
        F.prototype = parent;
        return new F();
    };
    ```

    使用真正的原型继承（如 Object.create 以及 __proto__）还是存在以下缺点：
    - 标准性差：__proto__ 不是一个标准用法，甚至是一个不赞成使用的用法。同时原生态的 Object.create 和道爷写的原版也不尽相同。
    - 优化性差： 不论是原生的还是自定义的 Object.create ，其性能都远没有 new 的优化程度高，前者要比后者慢高达10倍。

    **ES6 内部实现类和类的继承**

    ```
    class Parent {
        constructor(name) { //构造函数
              this.name = name;
        }
        say() {
              console.log("Hello, " + this.name + "!");
        }
    }

    class Children extends Parent {
        constructor(name) { //构造函数
            super(name);    //调用父类构造函数
            // ...
        }
        say() {
              console.log("Hello, " + this.name + "! hoo~~");
        }
    }
    ```



参考：
- http://blog.csdn.net/xujie_0311/article/details/44466573
- http://blog.vjeux.com/2011/javascript/how-prototypal-inheritance-really-works.html

***


9 . `请尽可能详尽的解释 AJAX 的工作原理`

    > Ajax 的原理简单来说通过 XmlHttpRequest 对象来向服务器发异步请求，从服务器获得数据，然后用 JavaScript来操作 DOM 而更新页面。这其中最关键的一步就是从服务器获得请求数据。

    不使用ajax工作原理

    ![不使用ajax浏览网页的原理](https://camo.githubusercontent.com/cfeb768d2133c5a18520a29b69fe3102a47cd5c4/687474703a2f2f7969616e62696e2e71696e6975646e2e636f6d2f66652d616a61782d612e706e67)
    使用ajax工作原理
    ![使用ajax网页的原理](https://camo.githubusercontent.com/c466c7fba75ed3430a88fee04eb58eb581b02958/687474703a2f2f7969616e62696e2e71696e6975646e2e636f6d2f66652d616a61782d622e706e67)

***

10 . `javascript中"attribute" 和 "property" 的区别是什么？`

    > property 和 attribute非常容易混淆，两个单词的中文翻译也都非常相近（property：属性，attribute：特性），但实际上，二者是不同的东西，属于不同的范畴。每一个DOM对象都会有它默认的基本属性，而在创建的时候，它只会创建这些基本属性，我们在TAG标签中自定义的属性是不会直接放到DOM中的。

    - property是DOM中的属性，是JavaScript里的对象；
    - attribute是HTML标签上的特性，它的值只能够是字符串；
    - DOM有其默认的基本属性，而这些属性就是所谓的“property”，无论如何，它们都会在初始化的时候再DOM对象上创建。
    - 如果在TAG对这些属性进行赋值，那么这些值就会作为初始值赋给DOM的同名property。

***