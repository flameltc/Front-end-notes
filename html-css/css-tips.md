# CSS tips

- **有些inline元素（比如img、svg），他们在块级元素下时会留有一定的空隙，解决的办法是在inline元素上设置`vertical-align: top`，**比如如下例子

  ```html
  <style>
      div {
          background-color: pink;
      }
  </style>

  <div>
      <img src="http://p7e35vgip.bkt.clouddn.com/head.jpg" alt="just a test">
  </div>
  ```

  会出现如下的情况，如下图所示![css-tip1](http://p7e35vgip.bkt.clouddn.com/css-tip1.png) 

  这个时候只需要在inline元素即此时的img元素上添加`vertical-align: top`即可，如下图所示![css-tip2](http://p7e35vgip.bkt.clouddn.com/css-tip2.png)

 - **不把width和height写死的情况下保持宽高比相同，**如下代码

   ```css
   /*一个div里面没有其他元素，文本内容都没有*/

   /*
   html 结构

   <div class="demo1"></div>
   */
   //method one--->利用padding-top
   .demo1 {
       width: 20%;
       padding-top: 20%;
       background-color: pink;
   }

   //method two--->利用vw
   .demo1 {
       width: 20vw;
       height: 20vw;
       background-color: pink;
   }

   /*一个div里面有其他元素，比如是一个图片*/

   /*
   html 结构

   <div class="demo2">
   	<img src="" alt="just a test">
   </div>
   */

   .demo2 {
       width: 30%;
       padding-top: 30%;
       background-color: pink;
       position: relative;
   }
   .demo2 > img {
       position: absolute;
       top: 0;
       left: 0;
       width: 100%;
       height: 100%;
   }
   ```

   以上两种情况如下图所示

   ![css-tip3](http://p7e35vgip.bkt.clouddn.com/css-tip3.png)

- **图片作为背景图片时保持图片本身的宽高比例完美放入容器里，**如下代码

  ​

  ​

