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
* 1. 尽量使用字面量和局部变量 避免使用数组和对象
* 2. 


