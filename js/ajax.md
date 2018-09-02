# ajax

###  什么是ajax？

- **`ajax`**--->Asynchronous JavaScript And XML，即**异步的JavaScript和XML**，它是一种技术方案
- **`ajax`** 是一种可以同服务器通信，且可以得到服务器返回的数据并处理的一种技术方案
- **`ajax`** 只能向**同源网址**（协议、域名、端口都相同）发送HTTP请求；如果发出跨域请求，则报错
- **`ajax`** 优点：
  - 在不重新加载页面的情况下发送请求给服务器
  - 可以接受并使用从服务器发来的数据；得到服务器返回的数据后，也不会刷新整个页面，只更新相关部分

### ajax的实现方式

- `XMLHttpRequest` 对象
- 使用 Fetch

### XMLHttpRequest 对象

##### 1.使用 XMLHttpRequest 发送请求

- 如下示例代码

  ```js
  // 创建 XMLHttpRequest 实例对象 xhr
  let xhr = new XMLHttpRequest();
  
  // 设置 xhr 请求超时时间
  xhr.timeout = 5000;
  
  // 创建一个 GET 异步请求
  xhr.open('GET', '/login', true);
  
  // 设置服务器的响应返回的数据格式
  xhr.responseType = 'json';
  
  // 处理服务器响应
  // 1.通过 onreadystatechange 检查请求的状态时调用函数
  xhr.onreadystatechange = function () {
      // 2.当 readyState 的值为4时意味着服务器响应收到了并且是没问题的
      if (xhr.readyState === 4) {
          // 3.检查 HTTP 响应码区别对待成功和不成功的AJAX调用
          if ((xhr.status >= 200 && xhr.status < 300) || (xhr.status === 304)) {
              // 成功，服务器的响应回来了
              console.log('服务器响应OK')
              console.log(xhr.response);
          } else {
              // 失败
              console.log(xhr.statusText);
          }
      }
  };
  // 或者像下面这样写
  xhr.addEventListener('readystatechange', function() {
      if (xhr.readyState === 4) {
          if ((xhr.status >= 200 && xhr.status < 300) || (xhr.status === 304)) {
              console.log(xhr.response);
          } else {
              console.log(xhr.statusText);
          }
      }
  });
  // 也可以这样写，此时load就是xhr.readState === 4的状态
  xhr.addEventListener('load', () => {
      if ((xhr.status >= 200 && xhr.status < 300) || (xhr.status === 304)) {
          console.log(xhr.response);
      } else {
          console.log(xhr.statusText);
      }
  });
  
  // 当请求超时的时候
  xhr.ontimeout = function () {
      console.log('Timeout');
  }
  
  // 当网络异常的时候，比如断网
  xhr.onerror = function () {
      console.log('An error occurred during the transaction');
  }
  
  // 发送请求
  xhr.send();
  ```


##### 2.构造方法

- 通过构造函数`XMLHttpRequest()`初始化一个`XMLHttpRequest`对象

  ```js
  let xhr = new XMLHttpRequest();
  ```

##### 3.属性

- **`XMLHttpRequest.readyState`**

  - 返回一个整数，表示实例对象同服务器“握手”交互通信时所处的状态

  - 通信过程中，每当实例对象发生状态变化，`xhr.readyState`属性的值就会改变；该值每一次变化，都会触发`readyStateChange`事件

  - 各种状态如下图所示

    |  值  |         状态         | 描述                                                         |
    | :--: | :------------------: | :----------------------------------------------------------- |
    | `0`  |      ` UNSENT `      | `xhr`对象被成功构造，但尚未调用 `open()`方法                 |
    | `1`  |      ` OPENED `      | `open()`方法已经被调用。注意：只有`xhr`对象处于`OPENED`状态，才能调用`xhr.setRequestHeader()`和`xhr.send()`，否则会报错 |
    | `2`  | ` HEADERS_RECEIVED ` | `send()` 方法已经被调用，并且头部和状态已经可获得            |
    | `3`  |     ` LOADING `      | 响应体在下载中， `xhr.responseText` 属性已经包含部分数据     |
    | `4`  |       ` DONE `       | 下载操作已完成。不管本次请求是成功还是失败                   |

- **`XMLHttpRequest.onreadystatechange`**

  - 指向一个回调函数，当`xhr.readyState`属性发生变化，就会调用该回调函数

  - 语法如下

    ```js
    XMLHttpRequest.onreadystatechange = callback;
    ```

- **`XMLHttpRequest.status`**

  - 返回一个整数，表示服务器响应的 HTTP 状态码

  - 基本上，只有2xx和304的状态码，表示服务器返回的数据是正常状态

    ```js
    if (xhr.readyState === 4) {
        if ( (xhr.status >= 200 && xhr.status < 300) || (xhr.status === 304) ) {
            // 处理服务器的返回数据
        } else {
            // 出错
        }
    }
    ```

- **`XMLHttpRequest.statusText`**

  - 返回一个字符串，表示服务器返回状态对应的文本信息。比如`OK`、`Not Found`

- **`XMLHttpRequest.response`**

  - 表示服务器返回响应的正文
  - 它可以是 `ArrayBuffer`、`Blob`、 `Document`、JSON对象或一个DOMString类型，具体取决于`xhr.responseType`属性的值
  - 当请求完成时，该属性才有正确的值
  - 当请求未完成或未成功时，具体与 `xhr.responseType`有关：若`xhr.responseType`为`""`或`"text"`时，则响应可以包含部分文本响应，而请求仍处于加载状态；`xhr.responseType`为其他值时，值为 `null` 

- **`XMLHttpRequest.responseText`**

  - 返回从服务器接收到的字符串
  - 只有当 `xhr.responseType` 为`"text"`、`""`时，`XMLHttpRequest`对象上才有此属性，此时才能调用`xhr.responseText`，否则抛错

- **`XMLHttpRequest.responseXML`**

  - 返回从服务器接收到的 HTML 或 XML 文档对象；该属性得到的数据，是直接解析后的文档 DOM 树；如果本次请求没有成功，或者收到的数据不能被解析为 HTML 或 XML，则该属性为`null`
  - 该属性生效的两个前提：
    - 第一，在发送请求前，`xhr.responseType` 要设置为`""`、`"document"`，`XMLHttpRequest`对象上才有`xhr.responseXML`属性
    - 第二，HTTP 回应的`Content-Type`头信息等于`text/xml`或`application/xml`
    - 如果 HTTP 回应的`Content-Type`头信息不等于`text/xml`和`application/xml`，但是想从`xhr.responseXML`拿到数据（即把数据按照 DOM 格式解析），则需要手动调用`xhr.overrideMimeType()`来强制进行 XML 解析

- **`XMLHttpRequest.responseType`**

  - 指定`xhr.response`的数据类型

  - 数据类型如下表所示

    |        值         | `xhr.response`的数据类型 | 描述                                                        |
    | :---------------: | :----------------------: | :---------------------------------------------------------- |
    |       `""`        |      `String`字符串      | 默认值（在不设置`responseType`时），等同于`text`            |
    |     `"text"`      |      `String`字符串      | 表示服务器返回文本数据                                      |
    |   `"document"`    |      `Document`对象      | 表示服务器返回一个文档对象， 适合返回 HTML / XML 文档的情况 |
    |     `"json"`      |        `JSON`对象        |                                                             |
    |    ` "blob" `     |        `Blob`对象        | 表示服务器返回二进制对象，适合读取二进制数据，比如图片文件  |
    | ` "arrayBuffer" ` |    `ArrayBuffer`对象     | 表示服务器返回二进制数组                                    |

- **`XMLHttpRequest.timeout`**

  - 返回一个整数，表示多少毫秒后，如果请求仍然没有得到结果，则自动终止。如果该属性等于0，就表示没有时间限制
  - 请求开始：调用`xhr.send()`方法的时候
  - 请求结束：`xhr.loadend`事件触发的时候 

- **事件监听属性**

  `XMLHttpRequest`对象有以下事件监听属性，都继承自`XMLHttpRequestEventTarget`接口

  - `xhr.onloadstart`--->对应`loadstart`事件，当一个HTTP请求开始加载数据时调用
  - `xhr.onprogress`--->对应`progress`事件，间歇调用该方法用来获取请求过程中的信息
  - `xhr.onabort`--->对应`abort`事件，表示请求中止时会被调用，比如用户调用了`xhr.abort()`方法
  - `xhr.ontimeout`--->对应`timeout`事件，表示请求时间超过了`xhr.timeout`设置的时间时会被调用
  - `xhr.onerror`--->对应`error`事件，当请求发生错误时调用该方法
  - `xhr.onload`--->对应`load`事件，当一个HTTP请求正确加载出内容后返回时调用
  - `xhr.onloadend`--->对应`loadend`事件，当内容加载完成，不管失败与否，都会调用该方法

- **`XMLHttpRequest.withCredentials`**

  - 该属性为一个布尔值，表示**跨域**请求时，用户信息（比如 Cookie 和认证的 HTTP 头信息）是否会包含在请求中；**默认值为`false`**，即向`xxx.com`发送跨域请求时，不会发送`xxx.com`设置在本机上的 Cookie（如果有的话），如下代码

    ```js
    let xhr = new XMLHttpRequest();
    xhr.open('GET', 'http://xxx.com', true);
    xhr.withCredentials = true;
    xhr.send();
    ```

  - 要让这个属性生效，服务器必须显式返回`Access-Control-Allow-Credentials`这个头信息，如下

    ```js
    Access-Control-Allow-Credentials: true
    ```

- **`XMLHttpRequest.upload`**

  - 返回一个 `XMLHttpRequestUpload`对象，用来表示上传的进度

  - 通过`upload`上绑定的事件监听器来获得上传的进度，如下

    |  事件监听器   | 描述                             |
    | :-----------: | -------------------------------- |
    | `onloadstart` | 获取开始                         |
    | `onprogress`  | 数据传输进行中                   |
    |   `onabort`   | 获取操作终止                     |
    |   `onerror`   | 获取失败                         |
    |   `onload`    | 获取成功                         |
    |  `ontimeout`  | 获取操作在用户规定的时间内未完成 |
    |  `onloadend`  | 获取完成（不论成功与否）         |

##### 4.方法

- **`XMLHttpRequest.open()`**

  - 用于初始化一个 HTTP 请求

  - 语法如下

    ```js
    xhr.open(method, url);
    xhr.open(method, url, async);
    xhr.open(method, url ,async, user);
    xhr.open(method, url ,async, user, password);
    ```

    - `method`--->要使用的HTTP方法，比如`GET`、`POST`、`PUT`、`DELETE`、`HEAD`
    - `url`--->表示请求发送的目标URL
    - `async`--->一个可选的布尔参数，表示请求是否为异步。默认值为true，表示请求为异步
    - `user`--->表示用于认证的用户名，默认为空字符串。该参数可选
    - `password`--->表示用于认证的密码，默认为空字符串。该参数可选

  - 注意，如果对使用过`xhr.open()`方法的 AJAX 请求，再次使用这个方法，等同于调用`xhr.abort()`，即终止请求

- **`XMLHttpRequest.send()`**

  - 用于实际发出 HTTP 请求。如果是异步请求（默认为异步请求），则此方法会在请求发送后立即返回；如果是同步请求，则此方法直到响应到达后才会返回

  - 语法如下

    ```js
    xhr.send();
    xhr.send(data);
    ```

    - **`xhr.send()`的参数是可选的，表示发送的数据**

    - 如果**不带参数**，表示 HTTP 请求只包含头信息，即只有一个URL。比如`GET`请求，如下代码

      ```js
      let xhr = new XMLHttpRequest();
      xhr.open('GET', '/login?username=xxx&password=123');
      xhr.send();
      ```

    - 如果**带参数**，HTTP 请求除了头信息外，还有包含具体数据的信息体。比如`POST`请求，如下代码

      ```js
      let xhr = new XMLHttpRequest();
      xhr.open('POST','/login');
      xhr.send('username=xxx&password=123');
      ```

- **`XMLHttpRequest.abort()`**

  - 用来终止已经发出的 HTTP 请求
  - 当一个请求被终止时，`xhr.readyState`属性变为`4`，`xhr.status`属性变为`0`

- **`XMLHttpRequest.setRequestHeader()`**

  - 用于设置 HTTP 请求头部。该方法必须在`xhr.open()`和`xhr.send()`之间调用。如果对同一个请求头多次赋值，只会生成一个合并了多个值的请求头

  - 用法如下

    ```js
    xhr.open('GET','/login');
    
    // 用法
    xhr.setRequestHeader(header, value);
    // 基本例子
    xhr.setRequestHeader('Content-Type', 'application/javascript');
    // 对同一个请求头多次赋值，最终request header中Test为：xxx，yyy
    xhr.setRequestHeader('Test', 'xxx');
    xhr.setRequestHeader('Test', 'yyy');
    
    xhr.send();
    ```

- **`XMLHttpRequest.getResponseHeader()`**

  - 返回包含指定 header 字段的值，参数必须是**限制以外**的header字段，否则就会抛出`Refused to get unsafe header`的错误。如果响应尚未收到，或者响应中不存在标头，则返回null

  - 用法如下

    ```js
    xhr.getResponseHeader(name);
    // 例子
    xhr.getResponseHeader('Last-Modified');
    ```

- **`XMLHttpRequest.getAllResponseHeaders()`**

  - 返回响应的所有限制之外的 header 字段。如果没有收到服务器响应，则为`null`

  - 用法如下

    ```js
    xhr.getAllResponseHeaders();
    ```

- **`XMLHttpRequest.overrideMimeType()`**

  - 用来指定具体的 MIME 类型去代替有服务器指定的 MIME 类型。该方法必须在`xhr.send()`之前调用
  - 比如，服务器返回的数据类型为`text/html`，但由于各种原因浏览器无法解析。此时拿不到数据，但可以通过把 MIME 类型改成`text/plain`来拿到原始文本

##### 5.事件

- 

##### 6.封装 ajax

##### 7.实例