# 一、HTML中的JavaScript

## script属性

``` html
<!--
- async：可选。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其他脚本加载。只对外部脚本文件有效。
- charset：可选。使用 src 属性指定的代码字符集。这个属性很少使用，因为大多数浏览器不 在乎它的值。
- crossorigin：可选。配置相关请求的CORS（跨源资源共享）设置。默认不使用CORS。crossorigin= "anonymous"配置文件请求不必设置凭据标志。crossorigin="use-credentials"设置凭据 标志，意味着出站请求会包含凭据。
- defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。 在 IE7 及更早的版本中，对行内脚本也可以指定这个属性。
- integrity：可选。允许比对接收到的资源和指定的加密签名以验证子资源完整性（SRI， Subresource Integrity）。如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错， 脚本不会执行。这个属性可以用于确保内容分发网络（CDN，Content Delivery Network）不会提 供恶意内容。
- src：可选。表示包含要执行的代码的外部文件。
- type：可选。代替 language，表示代码块中脚本语言的内容类型（也称 MIME 类型）。按照惯 例，这个值始终都是"text/javascript"，尽管"text/javascript"和"text/ecmascript" 都已经废弃了。JavaScript 文件的 MIME 类型通常是"application/x-javascript"，不过给 type 属性这个值有可能导致脚本被忽略。在非 IE 的浏览器中有效的其他值还有 "application/javascript"和"application/ecmascript"。如果这个值是 module，则代 码会被当成 ES6 模块，而且只有这时候代码中才能出现 import 和 export 关键字。
- language：废弃。最初用于表示代码块中的脚本语言（如"JavaScript"、"JavaScript 1.2" 或"VBScript"）。大多数浏览器都会忽略这个属性，不应该再使用它。
-->
```

## script使用方式

```html
<!--
script使用方式: 
    - 在HTML网页 script 标签中直接编写代码
    - 使用 script 的 src 属性引入外部代码

    注意点:
        - script 元素中不能直接编写 '</script>', 浏览器会把其当成 script 结束标签. 如果想要避免这个问题, 可以使用转义字符 '<\/script>'
        - 使用 src 属性的script 不应该在标签中书写任何代码, 浏览器只会去下载执外部文件
        - script 元素可以使用外部域的 JavaScript 文件
-->
```

## script执行顺序

```html
<!--
    - 标签位置
      把所有 JavaScript文件都放在<head>里，也就意味着必须把所有 JavaScript 代码都下载、解析和解释完成后，才能开始渲染页面（页面在浏览器解析到<body>的起始标签时开始渲染）。对于需要很多 JavaScript 的页面，这会导致页面渲染的明显延迟，在此期间浏览器窗口完全空白。为解决这个问题，一般将所有 JavaScript 引用放在<body>元素中的页面内容后面
    - 推迟执行脚本
      使用 defer 属性延迟代码执行,脚本会被延迟到整个页面都解析完毕后再运行。在<script>元素上.设置 defer 属性，相当于告诉浏览器立即下载，但延迟执行。(只适用于外部脚本)
    - 异步执行脚本
      使用 async 和 defer 一样都会告诉浏览器立即下载, 不过, 与 defer 不同的是，标记为 async 的脚本并不保证能按照它们出现的次序执行
      添加 async 属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到该异步脚本下载和执行后再加载其他脚本。正因为如此，异步脚本不应该在加载期间修改 DOM。
    - 动态加载脚本
      除了<script>标签，还有其他方式可以加载脚本。因为 JavaScript 可以使用 DOM API，所以通过向 DOM 中动态添加 script 元素同样可以加载指定的脚本。只要创建一 script 元素并将其添加到DOM 即可。
      let script = document.createElement('script');
      script.src = './01-index.js';
      script.async = false;
      document.head.appendChild(script);
      以这种方式获取的资源对浏览器预加载器是不可见的。这会严重影响它们在资源获取队列中的优先级。根据应用程序的工作方式以及怎么使用，这种方式可能会严重影响性能。要想让预加载器知道这些动态请求文件的存在，可以在文档头部显式声明它们
      <link rel="preload" href="01-index.js">
-->
```

## 其它

``` html
<!--
       1.行内代码与外部文件
       虽然可以直接在 HTML 文件中嵌入 JavaScript 代码，但通常认为最佳实践是尽可能将 JavaScript 代码放在外部文件中。不过这个最佳实践并不是明确的强制性规则。
       - 可维护性。 JavaScript 代码如果分散到很多 HTML 页面，会导致维护困难。而用一个目录保存所有 JavaScript 文件，则更容易维护，这样开发者就可以独立于使用它们的 HTML 页面来编辑代码。
       - 缓存。 。浏览器会根据特定的设置缓存所有外部链接的 JavaScript 文件，这意味着如果两个页面都用到同一个文件，则该文件只需下载一次。这最终意味着页面加载更快。
       ... ...

       2.文档模式
       E5.5引入了文档模式的概念，通过使用DOCTYPE实现模式切换，它的主要作用是告诉浏览器以哪种模式呈现，如何解析文档.
       混杂模式: 向后兼容, 实现IE5.5以下版本浏览器的渲染模式, 支持一些非标准的特性
       标准模式: 根据 web 标准去解析 HTML

       3.noscript元素(标签)
       如果浏览器不支持脚本或浏览器对脚本的支持被关闭。就会渲染 <noscript> 元素中的内容. 如果浏览器支持并启用脚本，则<noscript>元素中的任何内容都不会被渲染。
    -->
```

# 二、语言基础



## 语法

```html
<!--
        1.区分大小写
        ECMAScript 中一切都区分大小写。无论是变量、函数名还是操作符，都区分大小写。

        2.标识符
        所谓标识符，就是变量、函数、属性或函数参数的名称。
        标识符命名规则:
            - 第一个字符必须是一个字母、下划线（_）或美元符号（$）；
            - 剩下的其他字符可以是字母、下划线、美元符号或数字。
        推荐命名方式: 驼峰命名

        3.注释
        ECMAScript 采用 C 语言风格的注释，包括单行注释和块注释。
        单行注释: //
        多行注释: /* */

        4.严格模式
        ECMAScript 5 增加了严格模式（strict mode）的概念。严格模式是一种不同的 JavaScript 解析和执
行模型，ECMAScript 3 的一些不规范写法在这种模式下会被处理，对于不安全的活动将抛出错误。
        可以单独指定一个函数在严格模式下执行,只要把这个预处理指令放到函数体开头即可
        - 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
        - 消除代码运行的一些不安全之处，保证代码运行的安全;
        - 提高编译器效率，增加运行速度;
        - 为未来新版本的Javascript做好铺垫;

        "use strict"
        x = 123;        // 报错

        5.语句
        记着加分号有助于防止省略造成的问题，比如可以避免输入内容不完整。
        let sum = a + b;

        if 之类的控制语句只在执行多条语句时要求必须有代码块。
        if (test) {
            console.log(test);
        }

        5.关键字和保留字
        有特定用途的关键字, 不能用作标识符
        break	do	instanceof	typeof
        case	else	new	var
        catch	finally	return	void
        continue	for	switch	while
        debugger*	function	this	with
        default	if	throw	delete
        in	try

        将来可能用得上, 不能用作标识符
        abstract	enum	int	short
        boolean	export	interface	static
        byte	extends	long	super
        char	final	native	synchronized
        class	float	package	throws
        const	goto	private	transient
        debugger	implements	protected	volatile
        double	import	public
    -->
```

## 变量

 ECMAScript 变量是松散类型的，意思是变量可以用于保存任何类型的数据。每个变量只不过是一个用于保存任意值的命名占位符。有 3 个关键字可以声明变量：var、const 和 let。其中，var 在ECMAScript 的所有版本中都可以使用，而 const 和 let 只能在 ECMAScript 6 及更晚的版本中使用。

### var

```javascript
/*
        1. var 声明:
        // 定义变量
        var message;    // 默认保存 undefined

        // 变量初始化
        var num = 123;

        // 更改变量值
        var num = 123;
        num = 456;
        console.log(num);
        num = "abc";        // 合法, 但不推荐. 修改了变量保存数值的类型
        console.log(num);

        // 定义多个变量
        var num = 123,
            str = 'abc',
            boo = true

        2.var 作用域:
        使用 var 操作符定义的变量会成为包含它的函数的局部变量
        function test(){
            var num = 123;
            console.log(num);   // 局部变量

            age = 18;           // 全局变量, 不推荐
        }
        test();

        3.var 声明提升:
        使用 var 时，下面的代码不会报错。这是因为使用这个关键字声明的变量会自动提升到函数作用域顶部
        “提升”（hoist），也就是把所有变量声明都拉到函数作用域的顶部。
        function foo(){
            console.log(age);
            var age = 26;
        }
        foo();  // undefined

        // 等价于
        function foo(){
            var age;
            console.log(age);
            age = 26;
        }
        foo();
         */
```

### let

```JavaScript
/*
        1.let 声明
        - 不允许重复声明同一个变量
        - let 在全局作用域中声明的变量不会成为 window 对象的属性。（var 声明的变量则会）
        - let 声明的变量不会在作用域中被提升
        let age = 123;
        let age = 123;

        console.log(name);
        let name = 123;     // 报错

        let num = 123;
        console.log(window.num)

        2.作用域
        - let 定义的变量分为 全局作用域, 局部作用域, 块级作用域
        - 不同作用域之间同名变量不会报错
        if(true){
            // let 定义的变量存在块级作用域
            let num = 123;
            console.log(num);
        }

        let name = 'zs';
        if(true){
            let name = 'ls';
            console.log(name);
        }
        console.log(name);

        3.for 循环中的 let 声明
        - let 出现之前，for 循环定义的迭代变量会渗透到循环体外部
        - 在使用 var 的时候，最常见的问题就是对迭代变量的奇特声明和修改

        //for(let i = 0; i < 5; i++){
        //}
        //console.log(i); // 报错

        // for(var i = 0; i < 5; i++){
        //     setTimeout(() => {
        //         console.log(i);  // 5 5 5 5 5
        //     })
        // }

        // for(let i = 0; i < 5; i++){
        //     setTimeout(() => {
        //         console.log(i);     // 0 1 2 3 4
        //     })
        // }
        */
```

### const

```JavaScript
/*
        1.const 声明
        const 的行为与 let 基本相同，唯一一个重要的区别是用它声明变量时必须同时初始化变量。
        - const 定义的值不能修改
        - const也不允许重复申明
        - const 声明的的作用域也是块
        - const 可以修改对象内部的属性
        - const 使用 for-in / for-of循环
        */
```

总结:
            1. 不使用 var
                        有了 let 和 const，大多数开发者会发现自己不再需要 var 了。限制自己只使用 let 和 const 有助于提升代码质量，因为变量有了明确的作用域、声明位置，以及不变的值。
               2. const 优先，let 次之
                      声明可以让浏览器运行时强制保持变量不变，也可以让静态代码分析工具提前发现不合法的赋值操作。因此，很多开发者认为应该优先使用 const 来声明变量,只在提前知道未来会有修改时，再使用 let。这样可以让开发者更有信心地推断某些变量的值永远不会变，同时也能迅速发现因意外赋值导致的非预期行为。
