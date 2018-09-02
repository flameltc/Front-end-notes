# Document 类型

- 在浏览器中，document对象是Document类型的一个实例，用于表示整个html页面

- document对象常用的属性

  - document.doctype--->表示当前文档对象的文档类型，在HTML页面中，比如`<!doctype html>`，返回html

  - document.documentElement--->返回当前文档对象的根节点，在HTML页面中，一般是`<html>`节点

  - document.head--->返回当前文档对象的`<head>`节点

  - document.title--->返回当前文档对象的title

  - document.characterSet--->返回当前文档对象的编码

  - document.body--->返回当前文档对象的`<body>`节点

  - document.links--->返回HTMLCollection的实例，其包含当前文档所有设置了href属性的a节点和area节点

  - document.images--->返回HTMLCollection的实例，其包含当前文档所有img节点

  - document.form--->返回HTMLCollection的实例，其包含当前文档所有form节点

  - document.scripts--->返回HTMLCollection的实例，其包含当前文档所有script节点

  - document.styleSheets--->返回文档内嵌或引入的样式表集合

  - **document.cookie--->获取并设置与当前文档相关联的 cookie**

  - **document.domain--->返回当前文档的原始域部分，不包括protocol和port。如果无法获取域名则返回null。次级域名的网页，可以通过document.domain来设置其对应的上级域名。document.domain相同的网页可以获取对方的资源**

    ```js
    // 比如当前页面网址：https://dengpan.com:4097/index.html
    document.domain;//dengpan.com

    // 一个有次级域名的网页，比如：https://a.b.dengpan.com
    document.domain = 'b.dengpan.com'; // 可以这样设置
    document.domain = 'dengpan.com'; // 也可以这样设置
    ```

  - **document.location--->返回一个Location对象，包含与文档URL相关的信息，并提供改变该URL和加载其他URL的方法**

    ```js
    // 当前页面网址：https://user:passed@dengpan.com:4097/src/index.html?a=1#part1
    document.location.href; // https://dengpan.com:4097/src/index.html?a=1#part1
    document.location.protocol; // https
    document.location.host; // dengpan.com:4097
    document.location.hostname; // dengpan.com
    document.location.port; // 4097
    document.location.pathname; // /src/index.html
    document.location.search; // ?a=1
    document.location.hash; // #part1
    document.location.username; // user
    document.location.password; // passed
    document.location.origin; // https://dengpan.com

    // 跳转到另一个网址
    document.location.assign('http://google.com');
    document.location.href = 'http://google.com';
    document.location = 'http://google.com';
    // 从服务器重新加载
    document.location.reload(true);
    // 从本地缓存重新加载
    document.location.reload();
    ```

- document 对象常用的方法

  - document.write()--->向当前文档写入内容

    ```
    打开一个页面时，浏览器会
    1.调用document.open()来打开文档
    2.document.write(...)将下载到的网页内容写入文档
    3.写完所有内容后，调用document.close()
    4.出发DOM ready事件
    只要在第三步之前使用document.write()，则写入的内容会追加在已有内容后；
    在第三步之后使用document.write()则浏览器会先调用document.open()来打开文档把之前内容清空后再向文档写入内容
    document.write()对写入的内容不会转义，比如document.write('<div>xxx</div>')则会在页面添加一个div元素
    ```

  - document.open()--->打开文档，清除当前文档的所有内容

  - document.close()--->用来关闭document.open()打开的文档

  - **document.getElementById()--->参数是大小写敏感的字符串，代表了要查找元素的唯一ID。返回匹配id属性的元素节点。若没有则返回null。只能在document对象上使用**

    ```js
    // <div id="xxx">xxx</div>
    let elXxx = document.getElementById('xxx');
    ```

  - **document.getElementsByClassName()--->返回HTMLCollection实例，成员是所有class属性满足指定条件的元素节点。可以在任何元素节点上调用**

    ```js
    /*
     *<div class="demo xxx">
     *	<p class="demo"><p>
     *</div>
     *<h2 class="demo xxx"></h2>
     *<span class="demo"></span>
     */
    let demos = document.getElementsByClassName('demo'); // 返回所有具有demo的class的元素
    let specialDemo = document.getElementsByClassName('demo xxx'); // 返回同时具有demo和xxx的class的元素
    ```

  - **document.getElementsByTagName()--->返回HTMLCollection实例，成员是满足指定标签名的元素节点。可以在任何元素节点上调用**

    ```js
    /*
     *<div class="demo xxx">
     *	<p class="demo"><p>
     *</div>
     *<div class="demo xxx"></div>
     *<span class="demo"></span>
     */

    let divs = document.getElementsByTagName('div'); // 返回所有标签名为div的元素
    ```

  - **document.querySelector()--->参数是css选择器，返回当前文档中匹配该选择器的第一个元素。可以在任何元素节点上调用**

    ```js
    /*
     *<div class="demo xxx">
     *	<p class="demo"><p>
     *</div>
     *<div class="demo xxx"></div>
     *<span class="demo sss"></span>
     */
    let xxx = document.querySelector('.xxx'); // 返回第一个class为xxx的元素
    ```

  - **document.querySelectorAll()--->参数是css选择器，返回一个NodeList实例，成员是所有满足指定条件的元素。可以在任何元素节点上调用**

    ```js
    /*
     *<div class="demo xxx">
     *	<p class="demo"><p>
     *</div>
     *<div class="demo xxx"></div>
     *<span class="demo sss"></span>
     */

    let demos = document.querySelectorAll('.demo');
    let aaa = document.querySelector('.xxx,.sss'); // 返回class属性为xxx或者sss的元素
    let bbb = document.querySelector('div,h2,p'); // 返回div，h2，p这三类元素
    ```

  - **document.createElement()--->参数为元素标签名，用来生成一个元素节点**

    ```js
    let newDiv = document.createElement('div');
    // 参数可以是自己定义的
    let foo = document.createElement('foo');
    ```

  - document.createAttribute()--->用来创建并返回一个新的属性节点，参数是属性名

    ```js
    // <div class="xxx">xxx</div>
    let elDiv = document.querySelector('.xxx');
    let newAttribute = document.createAttribute('myattribute');
    newAttribute.nodeValue = '属性值';
    elDiv.setAttribute(newAttribute);
    ```

    ​

  - **document.createDocumentFragment()--->生成一个空的文档片段对象**

    ```js
    let docfragment = document.createDocumentFragment();
    /*
     *文档片段对象只存在于内存中，不属于当前文档； 
     *文档片段对象常用来生成较复杂的DOM结构，然后再插入当前文档； 
     *好处是文档片段对象不属于DOM，对它的任何改动都不会引发网页的重新渲染，
     *比直接修改当前文档的DOM有更好的性能表现
     */
    ```


  ​