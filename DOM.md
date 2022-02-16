### 1. DOM是什么

DOM的全称是 Document Object Model（文档对象模型），它是由W3C定义的一个标准

**而DOM操作，可以简单理解成“元素操作”。**



#### 1.1 页面的呈现经历了什么

输入网址 解析网址 发送请求 建立连接 返回代码 **解析代码**

（1）DOM构造

浏览器接受到服务器传递回来的代码，通过 html 解析器解析成一颗 DOM 树，而 dom 树像是一颗倒着的树，这棵树决定了某些节点之间特殊的关联，例如父子关系、性兄弟关系等；

接着接收 css 代码，通过css解析器构建出样式表规则，将这些规则分别应用到对应的DOM树节点上，得到一颗带有样式属性的DOM树



（2）布局

**浏览器按从上到下，从左到右的顺序，读取DOM树的文档节点**，顺序存放到一条虚拟的传送带上。

传送带上的盒子就是节点，而这条流动的传送带就是文档流。

如果我们读取到的节点是属于另一个节点下的子节点，那么在放入传送带的时候，就应该按顺序放到该节点盒子的内部。如果子节点下还有子节点，在传送带上的时候就继续套到子一级的盒子内部。

**文档流排完之后，开始获取计算节点的坐标和大小等CSS属性，作为盒子的包装说明**
然后把盒子在仓库里一一摆放，这就将节点布局到了页面。



（3）绘制页面

**布局完成之后**，我们在页面上其实是看不到任何内容的
浏览器只是计算出了每一个节点对象应该被放到页面的哪个位置上，但**并没有可视化**。
因此**最后一步就是将所有内容绘制出来，完成整个页面的渲染。**这里又涉及到 ui 线程的工作







### 2. 节点类型

#### 2.0 简介

网页中的所有内容都是节点（标签、属性、文本、注释等），在DOM 中，节点使用 node 来表示

HTML DOM 树中的所有节点均可通过 JavaScript 进行访问，所有 HTML 元素（节点）均可被修改，也可以创建或删除

一般地，节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性



DOM中包括有 12 中节点类型，常见的只有 3 种，分别是

1. 元素节点	nodeType = 1
2. 文本节点    nodeType = 3【包含文字、空格、换行等】
3. 属性节点    nodeType = 2

```html
<div id="wrapper">hello</div>
```



 元素节点：整个 div 元素称为

文本节点："hello" 

属性节点：id="wrapper"



#### 2.1 父级节点

node.parentNode

（1）父节点 parentNode【最近一个父节点】

（2）如果指定的节点没有父节点则返回 null



#### 2.2 子节点

##### childNodes

parentNode.childNodes

（1）标准属性

（2）包含所有子节点的集合，该集合实时更新

（3）集合包含所有子节点，包含元素系节点，文本节点，属性节点



parentNode.firstChild

（1）返回第一个子节点



parentNode.lastChild

（1）返回最后一个子节点



##### children

parentNode.children

（1）非标准属性【各个浏览器也支持】

（2）只读属性，只返回子元素节点



parentNode.firstElementChild

（1）返回第一个子元素节点

（2）IE 9才支持



parentNode.lastElementChild

（1）返回最后一个子元素节点

（2）IE 9才支持



##### 实际开发的写法  既没有兼容性问题又返回第一个子元素，最后一个子元素

parentNode.children[0]

parentNode.children[ol.children.length - 1]



#### 2.3 兄弟节点

node.nextSibling

（1）下一个兄弟节点



node.previousSibling

（2）上一个兄弟节点



node.nextElementSibling

（1）下一个兄弟元素节点【IE9才支持】



node.previousElementSibling

（2）上一个兄弟元素节点【IE9才支持】



解决兼容

```js
   function getNextElementSibling(element) {
      var el = element;
       // 与当前元素相等的兄弟元素
      while (el = el.nextSibling) {
        if (el.nodeType === 1) {
            return el;
        }
      }
      return null;
    } 
```



#### 节点操作

#### 2.4 创建节点

（1）document.write（）

​	如果页面文档流加载完毕，再调用这句话会导致页面重绘



（2）innerHTML 

​	创建多个元素效率高，不使用拼接字符串，使用数组形式拼接

​	不会导致页面全部重绘

​	

（3）doucment.createElement（' tagName '）【标签名】

​	创建元素效率稍低，但结构清晰，易于理解

​	因为这些元素都不存在，所以我们也称动态创建节点



**innerHTML字符串拼接方式（效率低）**

```js
<script>
    function fn() {
        var d1 = +new Date();
        var str = '';
        for (var i = 0; i < 1000; i++) {
            document.body.innerHTML += '<div style="width:100px; height:2px; border:1px solid blue;"></div>';
        }
        var d2 = +new Date();
        console.log(d2 - d1);
    }
    fn();
</script>
```



**createElement方式（效率一般）**

```js
<script>
    function fn() {
        var d1 = +new Date();

        for (var i = 0; i < 1000; i++) {
            var div = document.createElement('div');
            div.style.width = '100px';
            div.style.height = '2px';
            div.style.border = '1px solid red';
            document.body.appendChild(div);
        }
        var d2 = +new Date();
        console.log(d2 - d1);
    }
    fn();
</script>
```



**innerHTML数组方式（效率高）**

```js
<script>
    function fn() {
        var d1 = +new Date();
        var array = [];
        for (var i = 0; i < 1000; i++) {
            array.push('<div style="width:100px; height:2px; border:1px solid blue;"></div>');
        }
        document.body.innerHTML = array.join('');
        var d2 = +new Date();
        console.log(d2 - d1);
    }
    fn();
</script>
```





#### 2.5 添加节点【插入节点】

（1）node.appenChild（child）

node 父级  

child 是子级 后面追加元素

类似于 css 里面的 after 伪元素





（2）node.insertBefore(child, 指定元素);

ul.insertBefore(lili, ul.children[0]);

将一个节点添加到父节点的指定节点前面



#### 2.5 删除节点

（1）node.removeChild() 

从 node节点中删除一个子节点，返回删除的节点。



#### 2.6 克隆节点

（1）node.cloneNode();

返回该结点的一个副本，称 克隆节点 / 拷贝节点

括号为空或者里面是false 浅拷贝 只复制标签不复制里面的内容

括号为true 深拷贝 复制标签复制里面的内容

var lili = ul.children[0].cloneNode(true);





### 3. 获取元素的方法

#### 1. 获取常规元素的方法

**1. 根据ID获取**

let timer = document.getElementById('time');

```javascript
<body>
    <div id="time">2019-9-9</div>
    <script>
        // 因为我们文档页面从上往下加载，所以先得有标签 所以我们script写到标签的下面
        let timer = document.getElementById('time');
        console.log(timer);
        console.log(typeof timer); // object

        // console.dir 打印我们返回的元素对象 更好的查看里面的属性和方法
        console.dir(timer);
    </script>
</body>
```



**2. 根据标签名获取元素**

document.getElementsByTagName('标签名') 

​	或者 

element.getElementsByTagName('标签名') 

```js
        // 1.返回的是 获取过来元素对象的集合 以伪数组的形式存储的
        var lis = document.getElementsByTagName('li');
        console.log(lis);
        console.log(lis[0]); // 可以直接使用下标来获取元素

        var nav = document.getElementById('nav'); // 这个获得nav 元素
        var navLis = nav.getElementsByTagName('li');
```

注意：getElementsByTagName ( ) 获取到是动态集合，即：当页面增加了标签，这个集合中也就增加了元素



 **3. H5新增获取元素方式**【冷知识：html5 08年提出，14 年标准规范才被完全制订】

var boxs = document.getElementsByClassName('box');



var firstBox = document.querySelector('.box');

​	querySelector 返回指定选择器的第一个元素对象  切记里面的选择器需要加符号 ( .box or #nav)，. 是通过而类名获取，# 是通过 id 获取



var allBox = document.querySelectorAll('.box');

​	querySelectorAll()返回指定选择器的所有元素对象集合



#### 2. 获取特殊元素的方法

body

​	document.body

html

​	document.documentElement





### 4. 常见的元素属性操作

1. ##### 获取或设置元素的内容

   ```
   // 获取元素的内容，去除标签以及空格和换行
   element.innerText
   
   // 不去除标签，保留空格和换行
   element.innerHTML
   ```

   

   innerText 和 innerHTML 的区别

   1. 在获取内容时，innerText 去除标签以及空格和换行，innerHTML不去除标签，同时保留空格和换行

   2. 设置内容时，innerText不会识别html，而innerHTML会识别

      

2. ##### 常用元素的属性操作

   1. src、herf
   2. id、alt、title



3. ##### 表单元素的属性操作

   type，value，checked，disabled，selected

   后三个元素对象的属性的值是布尔型



4.  ##### 样式属性操作
  
   1. element.style
   2. element.className

element.style 中注意：

- 元素对象的 style 属性也是一个对象
- JS 里面使用驼峰命名法，例如 fontSize、backgroundColor，在获取或者设置时需注意
- JS 修改 style 里面的样式产生的是内联样式，注意权重



element.className 中注意：

- 因为class是关键字，所有使用className。
- 修改样式过多使用类名操作的方式更好
- className 直接修改类名，会覆盖原来的类名



5. ##### 自定义属性操作

   （1）element.属性

   （2）element.getAttribute(' 属性 ')

   

element.属性    获取内置的属性

element.getAttribute(' 属性 ')   获取自定义的属性

此处插入一个小细节

```
// 小细节
//这样才能正确输出
console.log(div.getAttribute('class'));
console.log(div.className);
```



6.  ##### 设置属性值

(1) element.属性= '值'

(2) element.setAttribute('属性', '值');  主要针对于自定义属性

​	div.setAttribute('index', 2);



7. ##### 删除属性

element.removeAttribute(属性) ; 



8. ##### HTML5 自定义属性操作

自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中

自定义属性获取是通过getAttribute(‘属性’) 获取



H5 规定自定义属性以 data 开头，例如

```
<div data-index="2"></ div>
```





设置 H5 自定义属性

（1）直接向上面那样设置

（2）element.setAttribute('属性', '值')



获取自定义属性

（1）element.getAttribute(' 属性 ')

（2）【H5新增加	ie11开始支持】element.dataset.index 或者 element.dataset.[ ' index ' ]



注意	

如果自定义属性里面有多个-链接的单词，我们获取的时候采取 驼峰命名法

```
<div getTime="20" data-index="2" data-list-name="andy"></div>

// 如果自定义属性里面有多个-链接的单词，我们获取的时候采取 驼峰命名法
console.log(div.dataset.listName);
console.log(div.dataset['listName']);
```





### 5. 事件

#### 1. 事件概述

JavaScript 使我们有能力创建动态页面，而事件是可以被 JavaScript 侦测到的行为。

**触发——响应机制**

网页中的每个元素都可以产生某些可以触发 JavaScript 的事件



#### 2. 事件三要素

- 事件源（谁）：触发事件的元素
- 事件类型（什么事件）： 例如 click 点击事件
- 事件处理程序（做啥）：事件触发后要执行的代码(函数形式)，事件处理函数



#### 3. 执行事件的步骤

1. 获取事件源
2. 注册事件
3. 添加事件处理程序（函数赋值形式）



#### 4.  注册事件

传统方式

eventTarget.onclick = function() {
        alert('hi');
}



监听注册方式

addEventListener（type【事件类型】，listener【事件处理函数】，useCaptrue【是否使用事件捕获】）【IE9之后支持】

（1）同一个元素 同一个事件可以添加多个侦听器（事件处理程序）

    // 注意 click 加双引号
    eventTarget.addEventListener('click', function() {
        alert(22);
    })



attacheEvent（type【事件类型，需要带上on】，listener【事件处理函数】）【IE6 7 8支持】



#### 5.解绑事件

（1）eventTarget.onclick = null;

​	传统注册方式解绑



（2）eventTarget.removeEventListener( type ,  fn );

​	监听注册方式解绑【IE9】

（3）eventTarget.detachEvent( type 【带上on】, fn );

​	监听注册方式解绑【IE 678】



#### 6. DOM 事件流

**事件流**描述的是从页面中接收事件的顺序

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程叫做 DOM 事件流



DOM 事件流会经历3个阶段： 

1. 捕获阶段

2. 当前目标阶段

3. 冒泡阶段 



（1）事件冒泡

​	事件从具体的元素传播到不具体的元素

![](https://pic.imgdb.cn/item/61643f1f2ab3f51d91b74bad.png)



（2）事件捕获

​	事件从不具体的元素传播到具体的元素



#### 7. 事件对象

事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象



##### 事件对象的兼容性处理

事件对象本身的获取存在兼容问题：

1. 标准浏览器中是浏览器给方法传递的参数，只需要定义形参 e 就可以获取到。
2. 在 IE6~8 中，浏览器不会给方法传递参数，如果需要的话，需要到 window.event 中获取查找。



```js
        div.onclick = function(e) {
                // console.log(e);
                // console.log(window.event);
                // e = e || window.event;
                console.log(e);
        }
```



##### 时间对象的属性和方法

| 属性              | 说明                                        |
| :---------------- | ------------------------------------------- |
| e.target          | 返回触发事件的对象【标准属性】              |
| e.srcElement      | 返回触发事件的对象【非标准属性，ie6 - ie8】 |
| e.type            | 返回事件的类型，例如 click，不带on          |
| e.returnValue     | 阻止默认行为【非标准属性，ie6 - ie8】       |
| e.preventDefault  | 阻止默认行为【标准属性】，例如不让链接跳转  |
| e.cancelBubble    | 阻止冒泡【非标准属性，ie6 - ie8】           |
| e.stopPropagation | 阻止冒泡【标准属性】                        |



##### e.target 和 this 的区别

-  this 是事件绑定的元素（绑定这个事件处理函数的元素） 
-  e.target 是事件触发的元素



> 常情况下terget 和 this是一致的，
> 但有一种情况不同，那就是在事件冒泡时（父子元素有相同事件，单击子元素，父元素的事件处理函数也会被触发执行），
> 这时候this指向的是父元素，因为它是绑定事件的元素对象，
> 而target指向的是子元素，因为他是触发事件的那个具体元素对象



```
    <ul>
        <li>abc</li>
        <li>abc</li>
        <li>abc</li>
    </ul>
    <script>
        var ul = document.querySelector('ul');
        ul.addEventListener('click', function(e) {
              // 我们给ul 绑定了事件  那么this 就指向ul  
              console.log(this); // ul

              // e.target 触发了事件的对象 我们点击的是li e.target 指向的就是li
              console.log(e.target); // li
        });
    </script>
```



##### 阻止事件默认行为和事件冒泡

**阻止事件默认行为**

```
    <a href="http://www.baidu.com">百度</a>
    <script>
        // 2. 阻止默认行为 让链接不跳转 
        var a = document.querySelector('a');
        a.addEventListener('click', function(e) {
             e.preventDefault(); //  dom 标准写法
        });
        // 3. 传统的注册方式
        a.onclick = function(e) {
            // 普通浏览器 e.preventDefault();  方法
            e.preventDefault();
            
            // 低版本浏览器 ie678  returnValue  属性
            e.returnValue = false;
            
            // 我们可以利用return false 也能阻止默认行为 没有兼容性问题
            return false;
        }
    </script>
```



**阻止事件冒泡**

```js
    <div class="father">
        <div class="son">son儿子</div>
	</div>
	
    <script>
        var son = document.querySelector('.son');
		// 给son注册单击事件
        son.addEventListener('click', function(e) {
            alert('son');
            e.stopPropagation(); // stop 停止  Propagation 传播
            window.event.cancelBubble = true; // 非标准 cancel 取消 bubble 泡泡
        }, false);

        var father = document.querySelector('.father');
		// 给father注册单击事件
        father.addEventListener('click', function() {
            alert('father');
        }, false);
        
		// 给document注册单击事件
        document.addEventListener('click', function() {
            alert('document');
        })
    </script>
```



这里使用的是监听注册的方式注册事件，阻止事件冒泡也是在监听注册里面，使用传统方式注册事件里面阻止冒泡？

同样有效

```js
  <div class="father">
    <div class="son">son儿子</div>
  </div>

  <script>
    var son = document.querySelector('.son');
    son.onclick = function (e) {
      alert('son')
      e.stopPropagation();
    }

    var father = document.querySelector('.father');
    // 给father注册单击事件
    father.addEventListener('click', function () {
      alert('father');
    }, false);


    // 给document注册单击事件
    document.addEventListener('click', function () {
      alert('document');
    })
  </script>
```



**阻止事件冒泡的兼容性处理**

```
if(e && e.stopPropagation){
	e.stopPropagation()
}else{
	window.event.cancelBubble = true
}
```



#### 8. 事件委托

事件委托也称为事件代理

在 jQuery 中称为事件委派



不给子元素添加事件处理程序而给父元素添加，

在点击子元素的同时冒泡到父元素身上触发事件处理程序，对子元素进行操作



```js
 	<ul>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
        <li>知否知否，点我应有弹框在手！</li>
    </ul>
 
    <script>
        // 事件委托的核心原理：给父节点添加侦听器， 利用事件冒泡影响每一个子节点
        var ul = document.querySelector('ul');
        ul.addEventListener('click', function(e) {
            // e.target 这个可以得到我们点击的对象
            e.target.style.backgroundColor = 'pink';
        })
    </script>
```



事件委托的好处

- 我们只操作了一次 DOM ，提高了程序的性能

- 动态新创建的子元素，也拥有事件



#### 9. 常用的鼠标事件

| 鼠标事件     | 触发条件         |
| ------------ | ---------------- |
| onclick      | 鼠标点击左键触发 |
| onmouseover  | 鼠标经过触发     |
| onmouseout   | 鼠标离开触发     |
| onmouseenter | 鼠标进入触发     |
| onmouseleave | 鼠标离开触发     |
| onfocus      | 获得鼠标焦点出发 |
| onblur       | 失去鼠标焦点触发 |
| onmousemove  | 鼠标移动触发     |
| onmuoseup    | 鼠标弹起触发     |
| onmousedown  | 鼠标按下触发     |



**onmouseover，onmouseout与onmouseenter，onmouseleave的区别**

onmouseover、onmouseout：鼠标移动到自身时候会触发事件，同时移动到其子元素身上也会触发事件

onmouseenter、onmouseleave：鼠标移动到自身是会触发事件，但是移动到其子元素身上不会触发事件

onmouseenter、onmouseleave  事件不支持冒泡 



##### 禁止选中以及禁止右键

给 document 添加事件监听器

```js
<body>
    我是一段不愿意分享的文字
    <script>
        // 1. contextmenu 我们可以禁用右键菜单
        document.addEventListener('contextmenu', function(e) {
                e.preventDefault();
        })
        // 2. 禁止选中文字 selectstart
        document.addEventListener('selectstart', function(e) {
            e.preventDefault();
        })
    </script>
</body>
```



鼠标事件对象

| 属性      | 说明                                 |
| --------- | ------------------------------------ |
| e.clientX | 相对于浏览器窗口可视区的 X 坐标      |
| e.clientY | 相对于浏览器窗口可视区的 Y 坐标      |
| e.pageX   | 相对于文档页面的 X 坐标【IE9后支持】 |
| e.pageY   | 相对于文档页面的 Y 坐标【IE9后支持】 |
| e.screenX | 相对于电脑屏幕的 X 坐标              |
| e.screenY | 相对于电脑屏幕的 Y 坐标              |



（1）pageX 和 pageY 是相对于整个 body 来说的坐标，比如 向上滚动 a 个像素，

那么 pageY  就因该是加上滚动的距离 a 的



（2）输出的都是纯数字，没有带 px





#### 10. 常用的键盘事件，键盘事件对象

| 事件       | 说明                 |
| ---------- | -------------------- |
| onkeyup    | 某个按键松开时触发   |
| onkeydown  | 某个按键被按下时触发 |
| onkeypress | 某个按键被按下时触发 |



（1）onkeypress不识别功能键，例如 ctrl，shift 键等等，而 onkeydown 会识别功能键

（2）三个事件的执行顺序是

keydown ----》keypress ----》keyup



```js
// 
document.addEventListener('keyup', function() {
    console.log('我弹起了');
})
```





| 属性    | 说明                |
| ------- | ------------------- |
| keyCode | 返回该键的 ASCII 值 |

（1）onkeyup，onkeydown不区分大小写，onkeypress区分大小写

```js
document.addEventListener('keypress', function(e) {
    // console.log(e);
    console.log('press:' + e.keyCode);

})
```

