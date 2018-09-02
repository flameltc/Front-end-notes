### Node 类型

### DOM基本概念

- 什么是DOM？其有什么作用
  - **DOM--->Document Object Model，即文档对象模型，它是一个编程接口**
  - **DOM作用--->DOM是操作网页的接口，它将网页转换成一个JavaScript对象，从而可以使用脚本进行各种操作，比如增删改查**
- DOM节点类型有七种，如下：
  - **Document**--->文档节点，即每个文档的根节点，比如html页面中，document表示整个页面
  - **DocumentFragment**--->文档片段节点
  - **DocumentType**--->文档类型节点，比如`<!doctype html>`
  - **Element**--->元素节点，比如`<div></div>`
  - **Attribute**--->属性节点，比如img的src属性、title属性等
  - **Text**--->文本节点，比如`<p>Text Node</p>`中p元素里的Text Node
  - **Comment**--->注释节点
- DOM中定义了一个Node接口，该接口由DOM中所有节点类型来实现；Node接口在JavaScript中由Node类型来实现，其它节点都继承自Node类型。除了Node接口，还有parentNode接口和childNode接口

### Node 类型

- Node类型的常用属性

  - Node.nodeType--->返回一个整数值，表示当前节点的类型

  - Node.nodeName--->返回当前节点的节点名称

    - Document节点：#document
    - DocumentFragment节点：#document-fragment
    - DocumentType节点：文档的类型，比如`<!doctype html>`返回html
    - Element节点：大写的标签名
    - Attribute节点：属性名称
    - Text节点：#Text
    - Comment节点：#Comment

  - Node.nodeValue--->返回一个字符串，表示当前节点的文本值，可读写。只有Text节点、Comment节点有nodeValue；其它节点返回null

  - **Node.textContent--->返回当前节点和其后代的文本内容**

    ```js
    /*
     *<div class="demo">hello<p>world</p></div>
     *<div class="xxx">
     *	xxx
     *	<p>kkk</p>
     *</div>	
     */
    
    let demo = document.querySelector('.demo');
    let xxx = document.querySelector('.xxx');
    // 获取
    demo.textContent; // helloworld
    xxx.textContent;
    // 设置
    demo.textContent = 'this is a new text';
    
    /*
     *Node.textContent和Node.innerText区别
     *1.textContent会获取所有元素的文本内容，包括script,style和隐藏元素，而innerText不会
     *2.textContent不会受CSS样式的影响，而innerText会，并且会触发重排
     */
    ```

  - **Node.innerText--->返回当前节点和其后代“渲染”的文本内容**

  - **Node.nextSibling--->返回紧跟当前节点后的第一个同级节点（包括元素节点、文本节点、注释节点）；如果没有则返回null**

    ```js
    /*
     *<div class="demo"></div><p>hello world</p>
     *<div class="xxx">xxx</div>	
     *<p>kkk</p>
     */
    let demo = document.querySelector('.demo');
    let xxx = document.querySelector('.xxx');
    demo.nextSibling; // <p>hello world</p>
    xxx.nextSibling; // #text
    
    // 遍历所有紧跟element后的同级节点
    while (element) {
        element = element.nextSibling;
    }
    ```

  - **Node.previousSibling--->返回紧邻当前节点前面的距离最近的同级节点（包括元素节点、文本节点、注释节点）；如果没有则返回null**

  - **Node.parentNode--->返回当前节点的父节点。对于一个节点来说，其父节点只能是文档节点、文档片段节点、元素节点**

  - **Node.parentElement--->返回当前节点的元素父节点**

  - **Node.firstChild--->返回当前节点的第一个子节点，（包括元素节点、文本节点、注释节点）；如果没有则返回null**

  - **Node.lastChild--->返回当前节点的最后一个子节点，（包括元素节点、文本节点、注释节点）；如果没有则返回null**

  - **Node.childNodes--->返回一个类数组对象NodeList，它包含当前节点的所有子节点（包括元素节点、文本节点、注释节点）；如果没有则返回一个空的NodeList**

    ```js
    /*
     *<div class="parent">
     *	child child
     *	<p>hello world</p>
     *	<!--comment -->
     *</div>
     */
    let parent = document.querySelector('.parent');
    parent.childNodes; // NodeList(5) [text, p, text, comment, text]
    ```

- Node 类型常用的方法

  - **Node.appendChild()--->参数是一个节点对象，将其作为当前节点的最后一个子节点插入。返回值是插入文档的子节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    let parent = document.querySelector('.parent');
    let newDiv = document.createElement('div');
    newDiv.textContent = 'hello world';
    parent.appendChild(newDiv);

    /*
     *完成后的html结构
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *	<div>hello world</div>
     *</div>
     */

    // 如果参数是已经存在的DOM节点，则会将其从原来位置移除再插入到新的位置
    // 如果参数是DocumentFragment对象，插入的是DocumentFragment对象的所有子节点，而不是其自身
    ```

  - **Node.insertBefore(newNode,referenceNode)--->在参考节点前面插入一个节点作为当前节点的一个子节点。返回值是新插入的节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    let parent = document.querySelector('.parent');
    let firstChild = document.querySelector('.parent > p');
    let newDiv = document.createElement('div');
    newDiv.textContent = 'hello world';
    parent.insertBefore(newDiv,firstChild);

    /*
     *完成后的html结构
     *<div class="parent">
     *	<div>hello world</div>
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    // 如果参数是DOM中已经存在的节点，则将其从原有位置移除再插入到新的位置
    // 如果省略referenceNode，则newNode被插入到当前节点的子节点的最后一个
    ```

  - **Node.replaceChild(newNode,oldNode)--->用新节点替换掉当前节点的一个子节点，并返回被替换掉的节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    let parent = document.querySelector('.parent');
    let firstChild = document.querySelector('.parent > p');
    let newDiv = document.createElement('div');
    newDiv.textContent = 'hello world';
    parent.replaceChild(newDiv,firstChild);

    /*
     *完成后的html结构
     *<div class="parent">
     *	<div>hello world</div>
     *	here is last child
     *</div>
     */

    // 如果newNode已经存在DOM中，则其将会从原来的位置移除，插入到新的位置
    ```

  - **Node.removeChild(child)--->从当前节点中删除一个子节点，并返回该删除的子节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    let parent = document.querySelector('.parent');
    let firstChild = document.querySelector('.parent > p');
    parent.removeChild(firstChild);

    /*
     *完成后的html结构
     *<div class="parent">
     *	here is last child
     *</div>
     */

    // 被移除的子节点仍然存在于内存中，只是不在DOM上
    ```

  - **Node.cloneNode()--->用于克隆当前节点，参数为布尔值，表示是否同时克隆当前节点的后代节点。返回值是克隆出来的新节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */
    
    let parent = document.querySelector('.parent');
    let P = document.querySelector('.parent > p');
    let oherP = p.cloneNode(true);
    parent.appendChild(otherP);
    
    /*
     *完成后的html结构
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *	<p>this is first child</p>
     *</div>
     */
    
    // 克隆一个元素节点会拷贝其所有属性及属性值，包括其以HTML内联方式的事件处理程序，
    // 比如onclick="console.log(111)"，但是不会拷贝用addEventListener方法和node.onclick = fn 添加的事件处理程序
    // 在没有使用Node.appendChild()等方法将克隆的节点添加到DOM中之前，克隆的节点不再文档中，此时没有父节点
    // 如果参数为false，则不克隆它的任何子节点。比如该节点所包含的所有文本也不会被克隆
    ```

    ​

  - **Node.contains(node)--->返回一个布尔值，表示传入的节点是否是当前节点本身或者当前节点的后代节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    let parent = document.querySelector('.parent');
    let firstChild = document.querySelector('.parent > p');
    parent.contains(parent);//true
    parent.contains(firstChild);//true
    ```

  - **Node.hasChildNodes()--->返回一个布尔值，表示当前节点是否有子节点**

    ```js
    /*
     *<div class="parent">
     *	<p>this is first child</p>
     *	here is last child
     *</div>
     */

    let parent = document.querySelector('.parent');
    parent.hasChildNodes();//true
    ```

###  parentNode 接口

- parentNode接口表示当前节点是一个父节点，它提供处理子节点的属性和方法

- **parentNode.children--->返回一个HTMLCollection实例，表示当前节点的所有元素子节点**

- **parentNode.firstElementChild--->返回当前节点的第一个元素子节点。如果没有则返回null**

- **parentNode.lastElementChild--->返回当前节点的最后一个元素子节点。如果没有则返回null**

- **parentNode.childElementCount--->返回一个整数，表示当前节点的元素子节点的个数**

- **parentNode.append()--->参数是一个或者多个节点对象，将它们作为当前节点的最后一个子节点插入**

  ```js
  /*
   *<div class="parent">
   *	<p>this is first child</p>
   *	here is last child
   *</div>
   */

  let parent = document.querySelector('.parent');
  let newDiv = document.createElement('div');
  newDiv.textContent = 'hello world';
  parent.append(newDiv,'javascript');

  /*
   *完成后的html结构
   *<div class="parent">
   *	<p>this is first child</p>
   *	here is last child
   *	<div>hello world</div>
   *	javascript
   *</div>
   */
  ```

- **parentNode.prepend()--->参数是一个或多个节点对象，插入的位置是当前节点的第一个子节点前面**

  ```js
  /*
   *<div class="parent">
   *	<p>this is first child</p>
   *	here is last child
   *</div>
   */

  let parent = document.querySelector('.parent');
  let newDiv = document.createElement('div');
  newDiv.textContent = 'hello world';
  parent.prepend(newDiv,'javascript');

  /*
   *完成后的html结构
   *<div class="parent">
   *	<div>hello world</div>
   *	javascript
   *	<p>this is first child</p>
   *	here is last child
   *</div>
   */
  ```

### childNode 接口

- childNode 接口表示当前节点是一个子节点，它提供一些方法

- **childNode.after()--->在当前节点的后面插入一个或多个节点对象作为其同级节点，它们具有同一个父节点**

  ```js
  /*
   *<div class="parent xxx">
   *	<p>this is first child</p>
   *	here is last child
   *</div>
   */

  let xxx = document.querySelector('.xxx');
  let newDiv = document.createElement('div');
  newDiv.textContent = 'hello world';
  xxx.after(newDiv,'javascript');

  /*
   *完成后的html结构
   *<div class="parent">
   *	<p>this is first child</p>
   *	here is last child
   *</div>
   *<div>hello world</div>
   *javascript
   */
  ```

- **childNode.before()--->在当前节点的前面插入一个或多个节点对象作为其同级节点，它们具有同一个父节点**

- **childNode.remove()--->用于将当前节点从其父节点移除**

- **childNode.replaceWith()--->使用参数节点替换掉当前节点。参数节点可以是文本节点也可以是元素节点**

  ```js
  /*
   *<div class="parent xxx">
   *	<p>this is first child</p>
   *	here is last child
   *</div>
   */

  let p = document.querySelector('.parent > p');
  let newDiv = document.createElement('div');
  newDiv.textContent = 'hello world';
  p.replaceWith(newDiv);

  /*
   *完成后的html结构
   *<div class="parent">
   *	<div>hello world</div>
   *	here is last child
   *</div>
   */
  ```

  ​

### 节点集合

- **DOM中提供两种集合用于容纳多个节点，分别是NodeList接口，HTMLCollection接口。其中NodeList接口返回的是NodeList实例；HTMLCollection接口返回的是HTMLCollection实例**

- **NodeList实例**

  - **NodeList实例是一个类数组对象，其成员是所有的节点对象**
  - **NodeList实例具有forEach方法和length属性**
  - **可以通过`Node.childNodes`和`document.querySelectorAll()`得到NodeList实例**
  - **只有`Node.childNodes`返回的是动态集合，其余都是静态集合**

- **HTMLCollection实例**

  - **HTMLCollection实例是一个类数组对象，其成员只包括元素节点对象**

  - **HTMLCollection实例只有length属性**

  - 可以通过如下方法得到HTMLCollection实例

    ```js
    // 得到HTMLCollection实例的方法
    document.links;
    document.forms;
    document.images;
    document.getElementsByClassName();
    document.getElementsByTagName();
    parentNode.children()
    ```

  - **HTMLCollection实例都是动态集合**