### 1. BOM

#### 1.1 什么是BOM

BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window。

BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。

BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是Netscape 浏览器标准的一部分。

bom 是浏览器厂商在各自的浏览器上定义的，兼容性较差



#### 1.2 bom 的构成

bom 的顶级对象是 window

window下包含，

（1）document【dom的顶级对象】

（2）location

（3）navigation

（4）screen

（5）history



#### 1.3 window对象

（1）window对象是浏览器的顶级对象

（2）js 访问浏览器窗口的一个接口

（3）是一个全局对象，定义在全局作用域中的方法、属性都会成为 window 对象的属性，或者方法

（4）window 下有一个特殊的属性 window.name 



##### 1.3.1 window对象常见事件

###### （1）窗口加载事件

1. window.onload 事件

window.onload 是窗口 (页面）加载事件，**当文档内容完全加载完成**会触发该事件(加载内容包括图像、脚本文件、CSS 文件等), 就调用的处理函数。

可以借用此事件，把 js 代码写在元素上方，等待页面内容全部加载完毕，再去执行处理函数



2. document.addEventListener( 'DOMContentLoaded' , function() { } )

DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等。

​	IE9以上才支持！！！

​	如果页面的图片很多的话, 从用户访问到onload触发可能需要较长的时间, 交互效果就不能实现，必然影响用户的体验，此时用 DOMContentLoaded 事件比较合适。



###### （2）调整窗口大小事件

window.onresize 是调整窗口大小加载事件,  当触发时就调用的处理函数。

注意：

1. 只要窗口大小发生像素变化，就会触发这个事件。

2. 我们经常利用这个事件完成响应式布局。 window.innerWidth 当前屏幕的宽度



###### （3）定时器

1. setTimeout() 

   



2. setInterval()  





#### 1.4 location 对象

##### 1.4.1 什么是 location 对象

用于获取或设置窗体的URL，并且可用于解析URL，该属性返回一个对象，称为 location 对象



##### 1.4.2 URL

统一资源定位符

互联网上的每一个资源都有一个唯一的 url ，它包含的信息指示文件的位置，以及浏览器应该怎么处理它



一般组成为

protocal : // host [:port] / path / [ ? query ] # fragment

http://121.41.113.77:9000/#/goods?id=1

| 组成     | 说明                                     |
| -------- | ---------------------------------------- |
| protocal | 通信协议【http，https，ftp】             |
| host     | 主机名，域名                             |
| port     | 端口号                                   |
| path     | 路径，用来表示主机上的一个目录或问价地址 |
| query    | 参数                                     |
| fragment | 片段，# 后面的内容，常用于链接锚点       |



| location对象属性  | 说明       |
| ----------------- | ---------- |
| location.href     | 整个 url   |
| location.host     | 返回主机名 |
| location.port     | 返回端口号 |
| location.pathname | 返回路径   |
| location.search   | 返回参数   |
| location.hash     | 返回片段   |



注意：在 vue 中使用 location.href 来跳转链接，需要加上主机名，例如 http://www.baidu.com，否则会识别成其他内容拼接在 url 里面

