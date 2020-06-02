JS面试题

https://www.kancloud.cn/chenmk/web-knowledges/1201811

前端 个人规划


语言方面
* 学习Deno
容器化方面
* Docker
* Kubernetes
云计算方面
* Serverless
架构方面
* 完善Full-Stack 架构
    * 第一层，是核心业务逻辑，前、后端功能、API、数据；
    * 第二层，是业务架构，具体包括应用框架、技术架构、数据库等；
    * 第三层：是业务运维，包括日志、监控告警、扩展性、负载均衡等；
    * 第四层：是底层架构，包括计算资源、系统及网络安全、灾备等。
第一层到第二层部分：全栈工程师们想做的东西
第二层部分到第四层：Serverless 可以解决的问题

前端如何真正晋级成全栈：腾讯 Serverless 前端落地与实践
https://juejin.im/post/5e609f8a6fb9a07c85143624

如何看待2020年web大前端市场发展趋势？
https://blog.csdn.net/qq_35847766/article/details/103926542

工程化与架构
https://www.kancloud.cn/chenmk/web-knowledges/1201811

Script Error产生的原因和解法
https://zhuanlan.zhihu.com/p/86652579

介绍一下你对浏览器内核的理解？
主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎。

渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理信息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。
JS引擎则：解析和执行javascript来实现网页的动态效果。最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。

常见的浏览器内核有哪些？
* Trident内核：IE,360，傲游，搜狗，世界之窗，腾讯等。[又称MSHTML]
* Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等
* Presto内核：Opera7及以上。 [Opera内核原为：Presto，现为：Blink;]
* Webkit内核：Safari,Chrome等。 [ Chrome的：Blink（WebKit的分支）]


判断 js 类型的方式
1. typeof
可以判断出'string','number','boolean','undefined','symbol' 但判断 typeof(null) 时值为 'object'; 判断数组和对象时值均为 'object'
2. instanceof
原理是 构造函数的 prototype 属性是否出现在对象的原型链中的任何位置
复制
function A() {}
let a = new A();
a instanceof A     //true,因为 Object.getPrototypeOf(a) === A.prototype;
3. Object.prototype.toString.call()
常用于判断浏览器内置对象,对于所有基本的数据类型都能进行判断，即使是 null 和 undefined
4. Array.isArray()
用于判断是否为数组
回到顶部
ES5 和 ES6 分别几种方式声明变量
ES5 有俩种：var 和 function ES6 有六种：增加四种，let、const、class 和 import
注意：let、const、class声明的全局变量再也不会和全局对象的属性挂钩
回到顶部
闭包的概念？优缺点？
闭包的概念：闭包就是能读取其他函数内部变量的函数。
优点：
1. 避免全局变量的污染
2. 希望一个变量长期存储在内存中（缓存变量）
缺点：
1. 内存泄露（消耗）
2. 常驻内存，增加内存使用量
回到顶部
浅拷贝和深拷贝
* 浅拷贝
复制
// 第一层为深拷贝
Object.assign()
Array.prototype.slice()
扩展运算符 ...
* 深拷贝
复制
JSON.parse(JSON.stringify())
递归函数
复制
function cloneObject(obj) {
  var newObj = {} //如果不是引用类型，直接返回
  if (typeof obj !== 'object') {
    return obj
  }
  //如果是引用类型，遍历属性
  else {
    for (var attr in obj) {
      //如果某个属性还是引用类型，递归调用
      newObj[attr] = cloneObject(obj[attr])
    }
  }
  return newObj
}
如何实现一个深拷贝 详细解析赋值、浅拷贝和深拷贝的区别
回到顶部
数组去重的方法
1.ES6 的 Set
复制
let arr = [1,1,2,3,4,5,5,6]
let arr2 = [...new Set(arr)]
2.reduce()
复制
let arr = [1,1,2,3,4,5,5,6]
let arr2 = arr.reduce(function(ar,cur) {
  if(!ar.includes(cur)) {
    ar.push(cur)
  }

  return ar
},[])
3.filter()
复制
// 这种方法会有一个问题：[1,'1']会被当做相同元素，最终输入[1]
let arr = [1,1,2,3,4,5,5,6]
let arr2 = arr.filter(function(item,index) {
  // indexOf() 方法可返回某个指定的 字符串值 在字符串中首次出现的位置
  return arr.indexOf(item) === index
})
回到顶部
DOM 事件有哪些阶段？谈谈对事件代理的理解
分为三大阶段：捕获阶段--目标阶段--冒泡阶段
事件代理简单说就是：事件不直接绑定到某元素上，而是绑定到该元素的父元素上，进行触发事件操作时(例如'click')，再通过条件判断，执行事件触发后的语句(例如'alert(e.target.innerHTML)')
好处：(1)使代码更简洁；(2)节省内存开销
回到顶部
js 执行机制、事件循环
      JavaScript 语言的一大特点就是单线程，同一个时间只能做一件事。单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。JavaScript 语言的设计者意识到这个问题，将所有任务分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous），在所有同步任务执行完之前，任何的异步任务是不会执行的。       当我们打开网站时，网页的渲染过程就是一大堆同步任务，比如页面骨架和页面元素的渲染。而像加载图片音乐之类占用资源大耗时久的任务，就是异步任务。关于这部分有严格的文字定义，但本文的目的是用最小的学习成本彻底弄懂执行机制，所以我们用导图来说明： 
￼
导图要表达的内容用文字来表述的话：       同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入 Event Table 并注册函数。当指定的事情完成时，Event Table 会将这个函数移入 Event Queue。主线程内的任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。上述过程会不断重复，也就是常说的 Event Loop(事件循环)。       我们不禁要问了，那怎么知道主线程执行栈为空啊？js 引擎存在 monitoring process 进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去 Event Queue 那里检查是否有等待被调用的函数。换一张图片也许更好理解主线程的执行过程：
￼
       上图用文字表述就是：主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为 Event Loop（事件循环）。只要主线程空了，就会去读取"任务队列"，这就是 JavaScript 的运行机制。
      说完 JS 主线程的执行机制，下面说说经常被问到的 JS 异步中 宏任务（macrotasks）、微任务（microtasks）执行顺序。JS 异步有一个机制，就是遇到宏任务，先执行宏任务，将宏任务放入 Event Queue，然后再执行微任务，将微任务放入 Event Queue，但是，这两个 Queue 不是一个 Queue。当你往外拿的时候先从微任务里拿这个回调函数，然后再从宏任务的 Queue 拿宏任务的回调函数。如下图： 
￼
宏任务：整体代码 script，setTimeout，setInterval
微任务：Promise，process.nextTick
参考链接：这一次，彻底弄懂 JavaScript 执行机制
回到顶部
介绍下 promise.all
      Promise.all()方法将多个Promise实例包装成一个Promise对象（p），接受一个数组（p1,p2,p3）作为参数，数组中不一定需要都是Promise对象，但是一定具有Iterator接口，如果不是的话，就会调用Promise.resolve将其转化为Promise对象之后再进行处理。       使用Promise.all()生成的Promise对象（p）的状态是由数组中的Promise对象（p1,p2,p3）决定的。
1. 如果所有的Promise对象（p1,p2,p3）都变成fullfilled状态的话，生成的Promise对象（p）也会变成fullfilled状态， p1,p2,p3三个Promise对象产生的结果会组成一个数组返回给传递给p的回调函数。
2. 如果p1,p2,p3中有一个Promise对象变为rejected状态的话，p也会变成rejected状态，第一个被rejected的对象的返回值会传递给p的回调函数。 Promise.all()方法生成的Promise对象也会有一个catch方法来捕获错误处理，但是如果数组中的Promise对象变成rejected状态时， 并且这个对象还定义了catch的方法，那么rejected的对象会执行自己的catch方法。 并且返回一个状态为fullfilled的Promise对象，Promise.all()生成的对象会接受这个Promise对象，不会返回rejected状态。
回到顶部
async 和 await
主要考察宏任务和微任务，搭配promise，询问一些输出的顺序
原理：async 和 await 用了同步的方式去做异步，async 定义的函数的返回值都是 promise，await 后面的函数会先执行一遍，然后就会跳出整个 async 函数来执行后面js栈的代码
回到顶部
ES6 的 class 和构造函数的区别
class 的写法只是语法糖，和之前 prototype 差不多，但还是有细微差别的，下面看看：
1. 严格模式
类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。考虑到未来所有的代码，其实都是运行在模块之中，所以 ES6 实际上把整个语言升级到了严格模式。
2. 不存在提升
类不存在变量提升（hoist），这一点与 ES5 完全不同。
复制
new Foo(); // ReferenceError
class Foo {}
3. 方法默认是不可枚举的
ES6 中的 class，它的方法（包括静态方法和实例方法）默认是不可枚举的，而构造函数默认是可枚举的。细想一下，这其实是个优化，让你在遍历时候，不需要再判断 hasOwnProperty 了
4. class 的所有方法（包括静态方法和实例方法）都没有原型对象 prototype，所以也没有[[construct]]，不能使用 new 来调用。
5. class 必须使用 new 调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用 new 也可以执行。
6. ES5 和 ES6 子类 this 生成顺序不同
ES5 的继承先生成了子类实例，再调用父类的构造函数修饰子类实例。ES6 的继承先 生成父类实例，再调用子类的构造函数修饰父类实例。这个差别使得 ES6 可以继承内置对象。
7. ES6可以继承静态方法，而构造函数不能
回到顶部
transform、translate、transition 分别是什么属性？CSS 中常用的实现动画方式
三者属性说明 transform 是指变换、变形，是 css3 的一个属性，和 width，height 属性一样； translate 是 transform 的属性值，是指元素进行 2D(3D)维度上位移或范围变换; transition 是指过渡效果，往往理解成简单的动画，需要有触发条件。
这里可以补充下 transition 和 animation 的比较，前者一般定义开始结束两个状态，需要有触发条件；而后者引入了关键帧、速度曲线、播放次数等概念，更符合动画的定义，且无需触发条件
回到顶部
介绍一下rAF(requestAnimationFrame)
      专门用来做动画，不卡顿，用法和setTimeout一样。对 rAF 的阐述 MDN 资料
      定时器一直是 js 动画的核心技术，但它们不够精准，因为定时器时间参数是指将执行代码放入 UI 线程队列中等待的时间，如果前面有其他任务队列执行时间过长，则会导致动画延迟，效果不精确等问题。       所以处理动画循环的关键是知道延迟多长时间合适：时间要足够短，才能让动画看起来比较柔滑平顺，避免多余性能损耗；时间要足够长，才能让浏览器准备好变化渲染。这个时候 rAF 就出现了，采用系统时间间隔(大多浏览器刷新频率是 60Hz，相当于 1000ms/60≈16.6ms)，保持最佳绘制效率，不会因为间隔时间过短，造成过度绘制，增加开销；也不会因为间隔时间太长，使用动画卡顿不流畅，让各种网页动画效果能够有一个统一的刷新机制。并且 rAF 会把每一帧中的所有 DOM 操作集中起来，在一次重绘或回流中就完成。
详情：CSS3动画那么强，requestAnimationFrame还有毛线用？
回到顶部
javascript 的垃圾回收机制讲一下
定义：指一块被分配的内存既不能使用，又不能回收，直到浏览器进程结束。
像 C 这样的编程语言，具有低级内存管理原语，如 malloc()和 free()。开发人员使用这些原语显式地对操作系统的内存进行分配和释放。 而 JavaScript 在创建对象(对象、字符串等)时会为它们分配内存，不再使用对时会“自动”释放内存，这个过程称为垃圾收集。
内存生命周期中的每一个阶段:
分配内存 —  内存是由操作系统分配的，它允许您的程序使用它。在低级语言(例如 C 语言)中，这是一个开发人员需要自己处理的显式执行的操作。然而，在高级语言中，系统会自动为你分配内在。 使用内存 — 这是程序实际使用之前分配的内存，在代码中使用分配的变量时，就会发生读和写操作。 释放内存 — 释放所有不再使用的内存,使之成为自由内存,并可以被重利用。与分配内存操作一样,这一操作在低级语言中也是需要显式地执行。
四种常见的内存泄漏：全局变量，未清除的定时器，闭包，以及 dom 的引用
1. 全局变量 不用 var 声明的变量，相当于挂载到 window 对象上。如：b=1; 解决：使用严格模式
2. 被遗忘的定时器和回调函数
3. 闭包
4. 没有清理的 DOM 元素引用
回到顶部
对前端性能优化有什么了解？一般都通过那几个方面去优化的？
前端性能优化的七大手段
1. 减少请求数量
2. 减小资源大小
3. 优化网络连接
4. 优化资源加载
5. 减少重绘回流
6. 性能更好的API
7. webpack优化


防抖与节流
https://www.jianshu.com/p/dc1f70ed3221


2020大前端有哪些技术热点和趋势？
https://zhuanlan.zhihu.com/p/99598195


MVC，MVP 和 MVVM
http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html

SEO 
https://www.cnblogs.com/baimeishaoxia/p/12672191.html

