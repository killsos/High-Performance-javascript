## 高性能javascript

#### 1. script position 

#### 2. group script 打包 CDN

#### 3. Noblocking Scripts 
* 减少js文件大小并限制http请求次数仅仅是第一步
* window.onload


#### 4. Deferred Scripts 延迟脚本
* script标签的defer属性指明本元素所含的脚本不会修改DOM,因此代码可以延迟执行

#### 5. Async 
* async和defer用于异步加载脚本
* async和defer相同点并行下载,在下载过程不会产生阻塞
* async和defer区别执行时机:async是加载完成后自动执行  defer是等待页面解析完成后执行


#### 6. Dynamic Scripts Elements
* 被添加到页面中就是开始执行
* 重点在于:无论何时启动下载,文件的下载和执行过程不会阻塞页面其他进程
* 使用动态脚本节点下载文件时,返回代码通常会立刻执行（Firefox Opera会等此前所有动态脚本执行完毕）
* script onlaod  readystatechange readystate : uninitialized loading  loaded interactive complete


#### 7. XMLHttpRequest Script Injection
* 局限性是同源

#### 8. Recommended NonBlocking Pattern

#### 9. Data Access 数据存取
1. 尽量使用字面量和局部变量 避免使用数组和对象
2. 管理作用域
	* Scope chains and Identifier Resolution
	* 每一个函数都有一个内部属性[[scope]],scope属性包含了一个被创建的作用域中对象的集合,这个集合被称为函数的作用域链,它决定了那些数据能被函数访问
	* 函数作用域中的每个对象被称之为一个可变对象,每个可变对象都以键值对形式存在.
	* 当一个函数创建后,它的作用域链会被创建此函数的作用域中可访问的数据对象所填充
3. 执行环境
	* 一个函数被调用时会创一个称为执行环境(execution context)的内部对象
	* 一个执行环境定义了一个函数执行时的环境,函数每次执行时对应的执行环境是独一无二的,所以多次调用同一个函数就会导致创建多个执行环境。
	* 函数执行完毕,执行环境就会被销毁
	* 每个执行环境都有自己的作用域链,用于解析标识符.当执行环境被创建时,它的作用域链初始化为当前运行函数的scope属性.这些值按照它们出现在函数中的顺序,  被复制到执行环境的作用域链中。这个过程一旦完成，一个被称为“活动对象”（activaion object）的新对象就为执行环境创建好了.
	* 活动对象作为函数运行时的变量对象,包含了所有局部变量 形参 实参集合 以及this.然后此对象被推入作用域链的最前端。当执行环境被销毁,活动对象也随之销毁
	
4. 标识符解析的性能
	* 标识符解析是有代价的 全局变量存在执行环境作用域链的最末端
	* 建议尽量使用局部变量 如果某个跨作用域的值在函数中被引用一次以上,就可以存储到局部变量 例如 document对象
	
5. 改变作用域链 scope chain Augmentation
	* 一个执行环境的作用域链是不会改变的 但是有两个语句可以改变：with trycatch
	* 最好避免使用with
	* trycatch 把异常对象推一个变量对象并置于作用域的首位  推荐做法将错误委托给一个函数来处理
6. 闭包 closures scope and memory
	* 闭包是允许函数访问局部作用域之外的数据 内存消耗加大
	* IE使用非原生Javascript对象来实现DOM对象,因此闭包会导致内存泄露
	* 搜索自有属性比字面量或局部变量中读取数据代价更高  解析自有属性可能对调用toString()导致性能不高
	* 嵌套成员 每次遇到点操作符,嵌套成员会导致javascript引擎搜索所有对象成员 locaction.href比window.location.href
	
	

