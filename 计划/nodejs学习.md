# nodejs 学习笔记 #

----------


## 第一篇 ：入门##



1.  node 官网下载，运行官网例子
1.  cnode 社区 学习搭建多人博客
>JavaScript中，函数和其他变量一样都是可以被传递的。进行函数传递
    
    举例来说，你可以这样做：
    
    function say(word) {
      console.log(word);
    }
    
    function execute(someFunction, value) {
      someFunction(value);
    }
    
    execute(say, "Hello");
    请仔细阅读这段代码！在这里，我们把 say 函数作为execute函数的第一个变量进行了传递。这里返回的不是 say 的返回值，而是 say 本身！
    
    这样一来， say 就变成了execute 中的本地变量 someFunction ，execute可以通过调用 someFunction() （带括号的形式）来使用 say 函数。
    
    当然，因为 say 有一个变量， execute 在调用 someFunction 时可以传递这样一个变量。
    
    我们可以，就像刚才那样，用它的名字把一个函数作为变量传递。但是我们不一定要绕这个“先定义，再传递”的圈子，我们可以直接在另一个函数的括号中定义和传递这个函数：
    
    function execute(someFunction, value) {
      someFunction(value);
    }
    
    execute(function(word){ console.log(word) }, "Hello");
    我们在 execute 接受第一个参数的地方直接定义了我们准备传递给 execute 的函数。
    
    用这种方式，我们甚至不用给这个函数起名字，这也是为什么它被叫做 匿名函数 。
    
    这是我们和我所认为的“进阶”JavaScript的第一次亲密接触，不过我们还是得循序渐进。现在，我们先接受这一点：在JavaScript中，一个函数可以作为另一个函数接收一个参数。我们可以先定义一个函数，然后传递，也可以在传递参数的地方直接定义函数。

* 我理解到可以分成模块，通过`exports.start = start;`导出然后在需要用到的文件里面导入。good
* 下面学习路由
* >我们需要的所有数据都会包含在request对象中，该对象作为onRequest()回调函数的第一个参数传递。但是为了解析这些数据，我们需要额外的Node.JS模块，它们分别是url和querystring模块。

                               url.parse(string).query
                                           |
           url.parse(string).pathname      |
                       |                   |
                       |                   |
                     ------ -------------------
    http://localhost:8888/start?foo=bar&hello=world
                                ---       -----
                                 |          |
                                 |          |

              querystring(string)["foo"]    |
                                            |
                         querystring(string)["hello"]

##路由
>在C++或C#中，当我们谈到对象，指的是类或者结构体的实例。对象根据他们实例化的模板（就是所谓的类），会拥有不同的属性和方法。但在JavaScript里对象不是这个概念。在JavaScript中，对象就是一个键/值对的集合 -- 你可以把JavaScript的对象想象成一个键为字符串类型的字典。但如果JavaScript的对象仅仅是键/值对的集合，它又怎么会拥有方法呢？好吧，这里的值可以是字符串、数字或者……函数！

## 异步式I/O与事件式编程 ##

* **阻塞与线程**
> 什么是阻塞（block）呢？线程在执行中如果遇到磁盘读写或网络通信（统称为 I/O 操作），
通常要耗费较长的时间，这时操作系统会剥夺这个线程的 CPU 控制权，使其暂停执行，同
时将资源让给其他的工作线程，这种线程调度方式称为 阻塞。当 I/O 操作完毕时，操作系统
将这个线程的阻塞状态解除，恢复其对CPU的控制权，令其继续执行。这种 I/O 模式就是通
常的同步式 I/O（Synchronous I/O）或阻塞式 I/O （Blocking I/O）。
   
    function readFileCallBack(err, data) {
    if (err) {
    console.error(err);
    } else {
    console.log(data);
    }
    }
    var fs = require('fs');
    fs.readFile('file.txt', 'utf-8', readFileCallBack);
    console.log('end.');

>eg : fs.readFile 调用时所做的工作只是将异步式 I/O 请求发送给了操作系统，然后立即
返回并执行后面的语句，执行完以后进入事件循环监听事件。当 fs 接收到 I/O 请求完成的
事件时，事件循环会主动调用回调函数以完成后续工作。因此我们会先看到 end.，再看到
file.txt 文件的内容。

*   **事件**

>Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。在开发者看来，事
件由 EventEmitter 对象提供。前面提到的 fs.readFile 和 http.createServer 的回
调函数都是通过 EventEmitter 来实现的。下面我们用一个简单的例子说明 EventEmitter
的用法：

    //event.js
    var EventEmitter = require('events').EventEmitter;
    var event = new EventEmitter();
    event.on('some_event', function() {
    console.log('some_event occured.');
    });
    setTimeout(function() {
    event.emit('some_event');
    }, 1000);

>运行这段代码，1秒后控制台输出了 some_event occured.。其原理是 event 对象注册了事件 some_event 的一个监听器，然后我们通过 setTimeout 在1000毫秒以后向event对象发送事件 some_event，此时会调用 some_event 的监听器。

* **模块和包** 


	* 什么是模块？
		 >Node.js 提供了 require 函数来调用其他模块，而且模块都是基于文件的，机制十分简单
		 >
	* 如何创建加载模块？
		>Node.js 提供了 exports 和 require 两个对象，其中 exports 是模块公开的接口，require 用于从外部获取一个模块的接口，即所获取模块的 exports 对象。
	* 单次加载，后面加载的会覆盖前面的
	* ---------------
	* 什么是包？
		>包是在模块基础上更深一步的抽象，Node.js 的包类似于 C/C++ 的函数库或者 Java/.Net的类库
		>即所谓的包。包通常是一些模块的集合，在模块的基础上提供了更高层的抽象，相当于提供了一些固定接口的函数库。通过定制package.json，我们可以创建更复杂、更完善、更符合规范的包用于发布
	* 如何创建包？
	>
	    //somepackage/index.js	
    	exports.hello = function() {
   		 console.log('Hello.');
		};然后在 somepackage 之外建立 getpackage.js，
    	内容如下：//getpackage.js var somePackage = require('./somepackage'); 
    		somePackage.hello();运行 node getpackage.js，控制台将输出结果 Hello.。

## 核心模块 ##

*   **全局对象**
	>  Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global对象的属性。
	* *全局对象与全局变量*
		> global 最根本的作用是作为全局变量的宿主。按照 ECMAScript 的定义，满足以下条件的变量是全局变量：
 在最外层定义的变量；
 全局对象的属性；
 隐式定义的变量（未定义直接赋值的变量）。
	
	* 	> process 是一个全局变量，即 global 对象的属性。它用于描述当前 Node.js 进程状态的对象，提供了一个与操作系统的简单接口。
	* 	console 用于标准的输出
	* 	util 是一个 Node.js 核心模块，提供常用函数的集合，用于弥补核心 JavaScript 的功能过于精简的不足。
		> **util.inherits**:Sub 仅仅继承了 Base 在原型中定义的函数，而构造函数内部创造的 base 属性和 sayHello 函数都没有被 Sub 继承。同时，在原型中定义的属性不会被 console.log 作为对象的属性输出
		> **util.inspect(object,[showHidden],[depth],[colors])**:是一个将任意对象转换为字符串的方法，通常用于调试和错误输出。它至少接受一个参数 object，即要转换的对象。
		>
				        var util = require('util');
			    function Person() {
			    this.name = 'byvoid';
			    this.toString = function() {
			    return this.name;
			    };
			    }
			    var obj = new Person();
			    console.log(util.inspect(obj));
			    console.log(util.inspect(obj, true));

		>事件驱动 events 事件的发射和监听
		>
				var events = require('events');
			var emitter = new events.EventEmitter();
			emitter.on('someEvent', function(arg1, arg2) {
			console.log('listener1', arg1, arg2);
			});
			emitter.on('someEvent', function(arg1, arg2) {
			console.log('listener2', arg1, arg2);
			});
			emitter.emit('someEvent', 'byvoid', 1991);


		* EventEmitter.on(event, listener) 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数 listener。
		* EventEmitter.emit(event, [arg1], [arg2], [...]) 发射 event 事件，传递若干可选参数到事件监听器的参数表。
		* EventEmitter.once(event, listener) 为指定事件注册一个单次监听器，即
		监听器最多只会触发一次，触发后立刻解除该监听器。
		* EventEmitter.removeListener(event, listener) 移除指定事件的某个监听
		器，listener 必须是该事件已经注册过的监听器。
		*EventEmitter.removeAllListeners([event]) 移除所有事件的所有监听器，如果指定 event，则移除指定事件的所有监听器。
		
		> #### 文件系统 ####
		> ### http服务器与客户端 ###
*   **常用工具**
*   **事件机制**
*   **文件系统访问**
*   **http服务器与客户端**