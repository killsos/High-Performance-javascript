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
7. 缓存对象成员值 Caching object member values
	* 函数中如果要多次读取同一个对象的属性 最佳做法是将属性值保存局部变量
	
#### 10. DOM编程 DOM Script
* 浏览器通常会把DOM和Javascript独立实现 
* 例如IE JScript位于jscript.dll文件中 DOM位于mshtml.dll 
* safair DOM webkit  javascript squirrelFish
* chrome DOM webkit  javascript v8
* firefox DOM Gecko javascript spiderMonkey
* inherently slow 天生慢 两个相互独立的功能只要通过节口彼此链接,就会产生消耗 减少交互次数
* DOM访问与修改 
	1. 减少DOM访问
	2. innerHTML 比 document.createElement 快
* 节点克隆 Cloning Nodes
	1 使用element.clondeNode() 替代 document.createElement()
	
* HTML集合  HTML Collections
	1. HTML集合是包含了DOM节点引用的类数组对象
	2. document.getElementByName() document.getElementByClassName()  document.getElementByTagName()
	3. document.images document.links document.forms document.forms[0].elements
	4. HTML集合是“假定实时态” 会自动更新
	5. 最好办法将复制到一个真正的数组中 
	6. 少用集合尽量少用length属性  原因:读取length会引起集合进行更新
	7. 最好使用一个局部变量缓存HTML集合 第一个优化原则是把集合存储到局部变量,并把length缓存到循环外部,然后使用局部变量替代读取元素

* 遍历DOM walking the DOM
	1. childNodes nextSibling
	2. DOM元素属性诸如childNodes firstChild nextSibling并不区分元素节点与其他节点（注释节点 文本节点）通过浏览器提供API已经自行过滤的
|属性名|被替代属性名|
|:-----|-----------:|
|children|childNodes|
|childElementCount|childNodes.length|
|firstElementChild|firstChild|
|lastElementChild|lastChild|
|nextElementSibling|nextSibling|
|previouslementChild|previousSibling|

* 选择器API 使用querySelectorAll() 使用css选择器为参数 返回一个NodeList---包含着匹配节点的类数组对象,这个方法不会返回HTML集合  而返回的节点不会对应实时的文档结构,避免了性能问题

##### 重绘与重构 repaints and reflows
* 浏览器下载完成页面中所有组件html css js image 之后解析成两个内部数据结构  
* DOM树 表示页面机构  render树 表示DOM节点如何显示
* DOM树中每一个需要显示的节点在渲染树中至少存在一个对应的节点(隐藏的DOM元素在渲染树中没有对应节点)。渲染树中的节点称为帧frames或盒boxes,符合css模 型的定义,理解页面元素为一个具有内边距 外边距 边框 和position的盒子。一旦DOM和渲染树构建完成.浏览器就开始显示(绘制 repaint)页面元素
* 当DOM元素的变化影响了元素的几何属性（width height),比如改变边框的宽度或给段落增加文字,导致行数增加---浏览器需要重新计算元素的几何属性---reflows
* 在完成重排之后,浏览器会重新绘制受影响的部分到屏幕中---重绘---repaints
* 重排
	* 添加/删除DOM元素
	* 元素位置改变
	* 元素尺寸改变
	* 内容改变
	* 页面渲染器初始化
	* 浏览器尺寸改变
* 渲染树变化的排队和刷新 queuing and flushing render tree changes
* 由于每次重排会产生计算消耗,浏览器通过队列优化修改并批量执行来优化重排过程
* 强制刷新队列并要求计划任务立刻执行,获取布局信息的操作会导致刷新
	1. offsetTop/left offsetWidth/Height
	2. scrollTop....
	3. clientTop...
	4. getComputedStyle()---IE
	
* 在修改样式的过程中,最好避免使用上面列出的属性,它们都会刷新渲染队列,即使你是在获取最近未发生改变的或者最新改变无关的布局信息
* 最有效率的方法不要改变样式信息的时候 获取上面的属性 触发强制刷新渲染队列

			document.body.currentStyle // IE Opera
			document.defaultView.getComputedStyle // W3C

* 最小化重绘和重排 Minimzing repaints and reflows
	1. 使用style.cssText
	2. 使用修改类名称
	3. 批量修改DOM
		* 当需要对DOM元素进行一系列操作时候,下面步骤会减少重绘和重排的次数
			1. 使元素脱离文档流
			2. 对其应用多重改变
			3. 把元素带回文档中
			
			###### 相对应操作如下：
			
			1. 隐藏元素,应用修改,重新显示
			2. 使用文档片段document.fragment在当前DOM之外构建一个子树,再把它拷贝回文档
			3. 将原始元素拷贝到一个脱离文档,修改副本,完成后再替换原始元素
			



	
	

