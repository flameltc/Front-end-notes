#  HTTP

### 一、理解HTTTP

- HTTP重要性

  - HTTP是前后端合作的重要方式
  - HTTP能帮你从本质上理解HTML、CSS、JavaScript、JSON、JSONP、图片等不同形式的资源
  - Web性能优化基本等价于对HTTP传输效率优化
  - 前端工程化需要对HTTP缓存有深入了解

- 什么是HTTP？

  - 四个概念--->client、server、request、response

  - client--->用户代理，即user agent，代理的英文是proxy，所以两者不是一样

    - 一般来讲，proxy是满足特定功能的agent，这些功能包括加速访问、内容缓存、内容过滤、信息加密等
    - 而user agent的功能比较广泛，比如chorme，能满足广泛的上网需求，curl、后端开发者的爬虫、搜索引擎的爬虫也是user agent

  - server--->服务端

  - request--->请求包括四部分，如下图所示

      ![](http://p7e35vgip.bkt.clouddn.com/request.png)

      - 第一部分：动词（请求的方式）  路径 协议/版本号
      - 第二部分：一堆key:value，用回车分隔
      - 第三部分：回车，作用只有一个：分隔开第二部分和第四部分
      - 第四部分：随便什么内容都可以，内容的格式必须要第二部分里用Content-Type来说明

  - response--->响应包括四个部分，如下图所示

      ![](http://p7e35vgip.bkt.clouddn.com/response.png)

      - 第一部分：协议/版本号 状态码 状态信息
      - 第二部分：一堆key:value，用回车分隔
      - 第三部分：回车，作用只有一个：分隔开第二部分和第四部分
      - 第四部分：随便什么内容都可以，内容的格式必须要第二部分里用Content-Type来说明

- Content-Type的常见类型

  - text/plain
  - text/html
  - text/css
  - application/javascript
  - application/json
  - application/xml
  - image/png
  - image/jpg
  - image/gif

- **HTML、CSS、JavaScript的本质**

  - 首先它们都是**字符串**，只不过**Content-Type**不同

  - 其次，**URL的后缀都是废话，毫无意义**，比如

    ```
    http://localhost:9999/index.html
    浏览器不是根据上面url的后缀来判断这是一个html的，而是根据response里的Content-Type里的类型来确定的
    ```

  - **浏览器通过地址栏、iFrame来请求HTML**

  - **浏览器通过link标签来获取CSS，然后渲染**

  - **浏览器通过script标签来获取JavaScript，然后执行**

  - **浏览器通过img标签来获取图片，然后展示**

  - 一些特殊情况，比如当在response里返回的是css，但是在响应头里设置Content-Type：application/javascript浏览器会怎么样，代码如下

    ```javascript
    // 服务端为server.js
    if (path === '/1') {
      	response.setHeader('Content-Type','text/html; charset=utf-8')
      	response.end(`
      		<!doctype html>
    		<html>
    			
              	<head>
                	<title>xxx</title>
                	<link rel="stylesheet" href="/2">
              	</head>
              	<body>
                	<h1>this is a demo</h1>
              	</body>
            </html>
          `)
    } else if (path === '/2') {
        // 返回的是css，但是设置Content-Type:application/javascript
      	response.setHeader('Content-Type','application/javascript; charset=utf-8')
      	response.end('body{bacground: pink;color: #fff}')
    }
    ```

    这个时候浏览器会有一个警告，如下图所示

    ![](http://p7e35vgip.bkt.clouddn.com/http-warn.png)

- **Json的本质**

  - Json本质也是**字符串**，只不过response里的**Content-Type**为**application/json**
  - 可以通过ajax来请求得到json

- **Jsonp的本质**

  - Jsonp的本质也是**字符串**，只不过response里的**Content-Type**为**application/javascript**
  - Jsonp内容格式：`functionName({"name": "xxx"})`

- **HTTP缓存**

  - > 什么是缓存--->缓存是一种保存资源副本并在下次请求时直接使用该副本的技术
    >
    > 实现方式--->当 web 缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去源服务器重新下载
    >
    > 好处--->缓解服务器端压力，提升性能(获取资源的耗时更短了)

  - **通过设置`Cache-Control`来达到HTTP缓存的目的，它可被用于在http 请求和响应中通过指定指令来实现缓存机制**

    - **缓存请求指令**，如下代码

      ```
      // max-age=<seconds>--->设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)
      Cache-Control: max-age=<seconds>
      
      // max-stale[=<seconds>]--->表明客户端愿意接收一个已经过期的资源。
      //                          可选的设置一个时间(单位秒)，表示响应不能超过的过时时间
      Cache-Control: max-stale[=<seconds>]
      
      // min-fresh=<seconds>--->表示客户端希望在指定的时间内获取最新的响应
      Cache-Control: min-fresh=<seconds>
      
      // no-cache--->在释放缓存副本之前，强制高速缓存将请求提交给原始服务器进行验证
      Cache-control: no-cache 
      
      // no-store--->缓存不应存储有关客户端请求或服务器响应的任何内容
      Cache-control: no-store
      
      // no-transform--->不得对资源进行转换或转变
      Cache-control: no-transform
      
      // only-if-cached--->表明客户端只接受已缓存的响应，并且不要向原始服务器检查是否有更新的拷贝
      Cache-control: only-if-cached
      ```

    - **缓存响应指令**，如下代码

      ```
      // must-revalidate--->缓存必须在使用之前验证旧资源的状态，并且不可使用过期资源
      Cache-control: must-revalidate
      
      // no-cache--->在释放缓存副本之前，强制高速缓存将请求提交给原始服务器进行验证
      Cache-control: no-cache
      
      // no-store--->缓存不应存储有关客户端请求或服务器响应的任何内容
      Cache-control: no-store
      
      // no-transform--->不得对资源进行转换或转变
      Cache-control: no-transform
      
      // public--->表明响应可以被任何对象（包括：发送请求的客户端，代理服务器，等等）缓存
      Cache-control: public
      
      // private--->表明响应只能被单个用户缓存，不能作为共享缓存（即代理服务器不能缓存它）
      Cache-control: private
      
      // proxy-revalidate--->与must-revalidate作用相同，但它仅适用于共享缓存（例如代理），并被私有缓存忽略
      Cache-control: proxy-revalidate
      
      // max-age=<seconds>--->设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)
      Cache-Control: max-age=<seconds>
      
      // s-maxage=<seconds>--->覆盖max-age 或者 Expires 头，但是仅适用于共享缓存，
      //                       比如说各个代理，并且私有缓存中它被忽略
      Cache-control: s-maxage=<seconds>
      ```

  - 一个缓存的例子，如下代码

    ```javascript
    // 服务端为server.js中
    if (path === '/index.html') {
    	response.statusCode = 200
      	response.setHeader('Content-Type','text/html;charset=utf-8')
        response.end(`
    		<!doctype html>
    		<html>
    			<head>
    				<link rel="stylesheet" href="/main.css">
    			</head>
    			<body>
    				<script src="/tab.js"></script>
    			</body>
    		</html>
    	`)
    } else if (path === '/main.css') {
        response.setHeader('Content-Type','text/css;charset=utf-8')
      
        // 下面这一句设置缓存，将最大存储周期设置为315360000/24/3600/365 = 十年
        // 在这个最大存储周期没有过期之前，以后访问main.css都会从disk中访问
        // 如果想要更新文件内容则应该通过文件名的更改来达到更新内容的目的
        // md5sum main.css，当前文件改动的时候，md5sum main.css这个值会改变
        // 当文件有改动的时候，通过在文件后面加上md5sum当前文件的值的改变来更新页面
        // 比如，md5sum main.css--->90bebe2fd80ef25a3a6da88c3012bc93
        // main-90bebe2fd80ef25a3a6da88c3012bc93.css
        // 对于入口文件，不要添加缓存，比如这里的/index.html；但是就算是加了，有些浏览器也不会不接受入口文件的缓存
        response.setHeader('Cache-Control','max-age=315360000')
      
        response.end('body{background: pink;}')
    } else if (path === '/tab.js') {
        response.setHeader('Content-Type','application/javascript;charset=utf-8')
      
        // 下面这一句设置缓存
        response.setHeader('Cache-Control','max-age=315360000')
      
        response.end('alert(111)')
    } else {
        console.log('你请求的资源不存在')
    }

    ```

    结果如下图所示：

    ![](http://p7e35vgip.bkt.clouddn.com/cache.png)

  - **Etag用于缓存验证**

    - Etag的用法，如下代码

      ```javascript
      // 服务端为server.js中，简单设置Etag
      if (path === '/index.html') {
      	response.statusCode = 200
        	response.setHeader('Content-Type','text/html;charset=utf-8')
          response.end(`
      		<!doctype html>
      		<html>
      			<head>
      				<link rel="stylesheet" href="/main.css">
      			</head>
      			<body>
      				<h1>这是Etag</h1>
      			</body>
      		</html>
      	`)
      } else if (path === '/main.css') {
          response.setHeader('Content-Type','text/css;charset=utf-8')
          
          // 这里设置Etag
          response.setHeader('Etag','xxx')
          
          response.end('body{background: pink;}')
      } else {
          console.log('你请求的资源不存在')
      }
      
      /* 
      1.第一次访问main.css时，除了response里面多了一个Etag: xxx外没有变化
      2.在第二次再次访问时，main.css文件会在request中带上If-None-Match:xxx
      */
      
      // 对Etag进行判断
      if (path === '/index.html') {
      	response.statusCode = 200
        	response.setHeader('Content-Type','text/html;charset=utf-8')
          response.end(`
      		<!doctype html>
      		<html>
      			<head>
      				<link rel="stylesheet" href="/main.css">
      			</head>
      			<body>
      				<h1>这是Etag</h1>
      			</body>
      		</html>
      	`)
      } else if (path === '/main.css') {
          let ifNoneMatch = request.headers['if-none-match']
          let css = 'body{background: pink}'
          let etag = md5sum(css) // 这里的md5sum()是伪代码
          
          // 这里对Etag进行判断
          if (ifNoneMatch === etag) {
              response.statusCode = 304
              response.end()
          } else {
              response.statusCode = 200
              response.setHeader('Content-Type','text/css;charset=utf-8')
              
              // 第一次对时候设置Etag
              response.setHeader('Etag',etag)
              
              response.write(css)
              response.end()
          }
      } else {
          console.log('你请求的资源不存在')
      }
      ```

    - **Cache-Control和Etag两者的区别：在于是否有请求服务器。Cache-Control是不会请求服务器的；而Etag依然会请求服务器，当服务器判断请求里带的Etag与自己的一致时则不会从服务器下载资源**

- **Cookie**

  - 首先明确HTTP是无状态的协议

  - > 什么是Cookie--->HTTP Cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上，它的大小一般不超过4KB
    >
    > 用处是什么--->它用于告知服务端两个请求是否来自同一浏览器
    >
    > 应用场景--->1.会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
    >
    > ​                     2.个性化设置（如用户自定义设置、主题等）
    >
    > ​                     3.浏览器行为跟踪（如跟踪分析用户行为）

  - **设置Cookie--->服务器在响应头部用Set-Cookie向用户代理发送Cookie信息**

    ```javascript
    // Set-Cookie语法
    // 简单使用
    Set-Cookie: <cookie名>=<cookie值>
        
    // Expires=<date>--->cookie的最长有效时间，形式为符合HTTP-date规范的时间戳
    //                   时间戳格式为格林尼治标准时间
    // let time = new Date(); 
    // time.toGMTString(); "Fri, 08 Jun 2018 06:05:34 GMT"    
    Set-Cookie: <cookie名>=<cookie值>; Expires=<date>
        
    // Max-Age=<non-zero-digit>--->在 cookie 失效之前需要经过的秒数
    Set-Cookie: <cookie名>=<cookie值>;  Max-Age=<non-zero-digit>
        
    // Domain=<domain-value>--->指定cookie可以送达的主机名。假如没有指定，
    //                          那么默认值为当前文档访问地址中的主机部分（但是不包含子域名） 
    Set-Cookie: <cookie名>=<cookie值>; Domain=<domain-value>    
        
    // Path=<path-value>--->指定一个URL路径，这个路径必须出现在要请求的资源的路径中才可以发送 Cookie首部    
    // 比如Path=/xxx，那么/xxx、/xxx/yyy、/xxx/ccc/ddd都满足匹配的条件
    Set-Cookie: <cookie名>=<cookie值>; Path=<path-value>  
        
    // Secure--->一个带有安全属性的cookie只有在请求使用SSL和HTTPS协议的时候才会被发送到服务器
    //           整个机制从本质上来说都是不安全的
    Set-Cookie: <cookie名>=<cookie值>; Secure
    
    // HttpOnly--->设置了HttpOnly属性的cookie不能使用JavaScript经由Document.cookie属性、
    //             XMLHttpRequest和Request APIs进行访问，以防范跨站脚本攻击（XSS）
    Set-Cookie: <cookie名>=<cookie值>; HttpOnly
    
    // 多个指令的例子
    Set-Cookie: <cookie名>=<cookie值>; Expires=<date>; Secure
    
    
    // 比如简单地在服务端server.js中种Cookie
    if (path === '/index.html') {
      	let cookie = request.headers['cookie']
      	let html = `
      		<!doctype html>
    		<html>
    			<head>
    				<link rel="stylesheet" href="/main.css">
    			</head>
    			<body>
    				<h1>__hi__</h1>
    				<form action="/login" method="get">
    					<input type="password" name="password" placeholder="测试密码是xxx">
    					<input type="submit" value="提交">
    				</form>
    			</body>
    		</html>
    
      	`
    	response.statusCode = 200
      	response.setHeader('Content-Type','text/html;charset=utf-8')
        // 判断Cookie能否对应
      	if (cookie === 'login=true') {
      		html = html.replace('__hi__','你好，登录用户')
      	} else {
      		html = html.replace('__hi__','你好')
      	}
        response.end(html)
    } else if (path === '/main.css') {
        response.setHeader('Content-Type','text/css;charset=utf-8')
        response.end('body{background: pink;}')
    } else if (path === '/login') {
    	response.setHeader('Content-Type','text/html;charset=utf-8')
        // 密码正确则种一个cookie
    	if (query.password === 'xxx') { // query.password这里是伪代码
    		response.setHeader('Set-Cookie','login=true')
    		response.end()
    	} else {
    		response.end('<h1>密码错了，滚</h1>')
    	}
    } else if (path === '/logout') {
    	// 当进入/logout时，删除Cookie
        let time = new Date(0)
        response.setHeader('Set-Cookie',`login=true; Expires=${time.toGMTString()}`)
    } else {
        console.log('你请求的资源不存在')
    }
    ```

- **Session**

  - session是client和server一对一交互的一种会话机制，它是存储在服务端，可以放在文件、数据库、内存里。

  - 当需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否已包含了一个session标识（称为session id），如果已包含则说明以前已经为此客户端创建过session，服务器就按照session id把这个session检索出来使用（检索不到，会新建一个），如果客户端请求不包含session id，则为此客户端创建一个session并且生成一个与此session相关联的session id，session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个session id将被在本次响应中返回给客户端保存

  - session一般是基于Cookie的，但是也可以通过localStorage、url接参数来实现。基于Cookie的范例如下

    ```javascript
    // 一个简单的session的使用
    
    const session = {}
    
    if (path === '/index.html') {
        let login = false
      	let cookie = request.headers['cookie'] // 此时cookie是sessionid=?????
        if (cookie) {
            let sessionid = cookie.split('=')[1]
            if (session[sessionid] && session[sessionid].login) {
                login = true
            }
        }
      	let html = `
      		<!doctype html>
    		<html>
    			<head>
    				<link rel="stylesheet" href="/main.css">
    			</head>
    			<body>
    				<h1>__hi__</h1>
    				<form action="/login" method="get">
    					<input type="password" name="password" placeholder="测试密码是xxx">
    					<input type="submit" value="提交">
    				</form>
    			</body>
    		</html>
    
      	`
    	response.statusCode = 200
      	response.setHeader('Content-Type','text/html;charset=utf-8')
        // 判断login是否存在
      	if (login) {
      		html = html.replace('__hi__','你好，登录用户')
      	} else {
      		html = html.replace('__hi__','你好')
      	}
        response.end(html)
    } else if (path === '/main.css') {
        response.setHeader('Content-Type','text/css;charset=utf-8')
        response.end('body{background: pink;}')
    } else if (path === '/login') {
    	response.setHeader('Content-Type','text/html;charset=utf-8')
        // 密码正确则种一个cookie，并设置sessionid
    	if (query.password === 'xxx') { // query.password这里是伪代码
             let randomNum = Math.random()
    		response.setHeader('Set-Cookie',`sessionid=${randomNum}`)
             session[randomNum] = {login:true}
    		response.end()
    	} else {
    		response.end('<h1>密码错了，滚</h1>')
    	}
    } else {
        console.log('你请求的资源不存在')
    }
    
    ```

    

- 反向代理

  - 正向代理代理客户端，反向代理代理服务器 ，如下图所示

    ![](http://p7e35vgip.bkt.clouddn.com/proxy.png)
### 二、HTTP面试题

- HTTP状态码知道哪些？
- 301和302的区别是什么？
- HTTP缓存怎么做？
- Cache-Control和Etag的区别是什么？
- Cookie是什么？Session是什么？两者的区别是什么？
  - Cookie和Session的区别：
    - Cookie用于标识用户；Session用于记录用户敏感信息（比如用户是否登录、用户密码、是否是VIP等）
    - 从实现机制上Cookie是属于HTTP协议层面的，它存储在浏览器上；而Session不属于HTTP协议层面，它是存储在服务器上的（可以放在文件、数据库、内存里），不同的后端框架有不同的Session处理方法
- LocalStorage 和 Cookie 的区别是什么？
- （must）GET 和 POST 的区别是什么？
- （must）怎么跨域？JSONP 是什么？CORS 是什么？postMessage 是什么？