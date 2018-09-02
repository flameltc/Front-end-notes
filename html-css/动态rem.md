# 动态rem

- 动态rem--->作为手机端专用的一个自适应方案

- **屏幕宽度是重要的信息，尽量让所有的单位都以宽度作为基准**

- 怎样让rem跟页面宽度产生联系：

  - **rem--->是根元素html的font-size的大小；利用js使font-size的大小为页面的宽度；这样rem间接地和页面宽度产生联系**

-   实现下面的简单手机页面![简单的手机页面](https://i.loli.net/2018/04/04/5ac4a515108d4.png)

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="utf-8">
          <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
          <title>JS Bin</title>
          <style>
              * {
                  margin: 0;
                  padding: 0;
              }

              .clearfix::after {
                  content: '';
                  display: block;
                  clear: both;
              }

              body {
                  font-size: 16px;
              }

              .ct {
                  background-color: #ddd;
              }

              .box {
                  width: 0.4rem;
                  height: 0.2rem;
                  margin: 0.04rem;
                  background-color: pink;
                  float: left;
              }
          </style>
          <script>
              let pageWidth = window.innerWidth;
              document.write('<style>html{font-size:' + pageWidth + 'px;}</style>')
          </script>
      </head>
      <body>
          <div class="ct clearfix">
              <div class="box">1</div>
              <div class="box">2</div>
              <div class="box">3</div>
              <div class="box">4</div>
          </div>
      </body>
  </html>
  ```

  ​

  ​