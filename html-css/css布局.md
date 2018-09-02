# CSS 布局

### CSS 布局套路

![css-layout](http://p7e35vgip.bkt.clouddn.com/css-layout.png)

### float 布局套路

- 儿子全加`float: left`

- 老子加一个class为`clearfix`

- `clearfix`写法如下

  ````css
  .clearfix::after {
      content: '';
      display: block;
      clear: both;
  }
  ````

- 示例如下

  - 代码如下

    ```css
    /*
     *<div class="dad clearfix">
     *	<div class="son">first son</div>
     *	<div class="son">second son</div>
     *</div>
     */
    
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    .clearfix:after { /* hack IE8 */
        content: '';
        display: block;
        clear: both;
    }
    
    .clearfix {
        zoom: 1; /* hack IE6 IE7 */
    }
    
    .dad {
        border: 1px solid green;
    }
    
    .son {
        float: left;  
    }
    
    .son:nth-child(1) {
        width: 30%;
        background-color: pink;
    }
    
    .son:nth-child(2) {
        width: 70%;
        background-color: orange;
    }
    ```

  - 效果图如下

    ![float-layout](http://p7e35vgip.bkt.clouddn.com/float-layout.png)

  - **注意：这两个`div.son`只用作布局，要添加内容在其里面添加**。比如，要给两个儿子间有个间距，如下图所示

    ![](http://p7e35vgip.bkt.clouddn.com/float1.png)
### 平均布局

##### 实现如下图所示的平均布局

![average-layout](http://p7e35vgip.bkt.clouddn.com/average-layout.png)

##### 元素宽度固定

- **方法一，使用float+ 将第一、第五的`margin-left`设置为0 + 将第四、第八的`margin-right`设置为0且元素宽度固定（可以兼容到 IE5），如下**

  - html代码如下

    ```html
    <div class="pictures clearfix">
        <div class="picture p1"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture p4"></div>
        <div class="picture p5"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture p8"></div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    .clearfix:after { /* hack IE8 */
        content: '';
        display: table;
        clear: both;
    }
    
    .clearfix {
        zoom: 1;  /* hack IE6 IE7 */
    }
    
    .pictures {
        width: 800px; /* .pictures的width是固定宽度 */
        background-color: pink;
    }
    
    .picture {
        float: left;
        width: 194px; /* .picture的width是固定宽度 */
        height: 194px;
        margin: 0 4px 5px;
        background-color: #ddd;
    }
    
    .p1,.p5 {
        margin-left: 0;
    }
    
    .p4,.p8 {
        margin-right: 0;
    }
    
    /* 可以使用:nth-child选择器来简化，此时可兼容到 IE9 */
    /* 将第一个、第五个的左边距设置为0 */
    .picture:nth-child(4n + 1) {
        margin-left: 0;
    }
    
    /* 将第四个、第五个的右外边距设置为0 */
    .picture:nth-child(4n) {
        margin-right: 0;
    }
    ```

  - IE5 下效果如下图所示

    ![ie5](http://p7e35vgip.bkt.clouddn.com/afs-ie5.png)

  - 使用`:nth-child()`后 IE9 下效果如下图所示

    ![ie9](http://p7e35vgip.bkt.clouddn.com/afs-ie9.png)

  - 现代浏览器下效果如下图所示

    ![现代浏览器](http://p7e35vgip.bkt.clouddn.com/afs.png)

- **方法二，使用float + 负margin且元素宽度固定（可以兼容到 IE5），如下**

  - html代码如下

    ```html
    <div class="pictures">
        <!-- 在所有picture的外边新增了一层 -->
        <div class="xxx clearfix">
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
        </div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    .clearfix:after { /* hack IE8 */
        content: '';
        display: table;
        clear: both;
    }
    
    .clearfix {
        zoom: 1; /* hack IE6 IE7 */
    }
    
    .pictures {
        width: 800px; /* .pictures的width是固定宽度 */
        background-color: pink;
    }
    
    /* 在新增的.xxx上将左右margin设置为负值，其值的绝对值等于.picture的左右外边距 */
    .xxx {
        margin-left: -4px;
        margin-right: -4px;
    }
    .picture {
        float: left;
        width: 194px; /* .picture的width是固定宽度 */
        height: 194px;
        margin: 0 4px 5px;
        background-color: #ddd;
    }
    ```

  - IE5 下效果如下图所示

    ![ie5](http://p7e35vgip.bkt.clouddn.com/afm-ie5.png)

  - 现代浏览器下效果如下图所示

    ![现代浏览器](http://p7e35vgip.bkt.clouddn.com/afm-m.png)

- **方法三，使用flex布局且元素宽度固定（可以兼容到 IE10），如下**

  - html代码如下

    ```html
    <div class="pictures">
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    /* 在.pictures上设置flex 布局 */
    .pictures {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        width: 800px; /*.pictures为固定宽度*/
        margin: 0 auto;
        background-color: pink;
    }
    
    .picture {
        width: 194px; /* .picture为固定宽度 */
        height: 200px;
        margin-bottom: 5px;
        background-color: #ddd;
    }
    ```

  - IE10 下效果如下图所示

    ![ie10](http://p7e35vgip.bkt.clouddn.com/afxs-ie10.png)

  - 现代浏览器下效果如下图所示

    ![现代浏览器](http://p7e35vgip.bkt.clouddn.com/afxs.png)

  - 当.pircture的个数在这里不为4的倍数时，会出现如下bug（可以用下一个方法来解决），效果如下图所示

    ![有bug](http://p7e35vgip.bkt.clouddn.com/afxs-b.png)

- **方法四，使用flex布局 + 负margin且元素宽度固定（可以兼容到 IE10），如下**

  - html代码如下

    ```html
    <div class="pictures">
        <!-- 在所有picture的外边新增了一层 -->
        <div class="xxx">
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
        </div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    .pictures {
        width: 800px; /* .pictures为固定宽度 */
        margin: 0 auto;
        background-color: pink;
    }
    
    /* 在.xxx上设置flex 布局并设置负外边距 */
    /* 在新增的.xxx上将左右margin设置为负值，其值的绝对值等于.picture的左右外边距 */
    .xxx {
        display: flex;
        flex-wrap: wrap;
        margin-left: -4px;
        margin-right: -4px;
    }
    
    .picture {
        width: 194px; /* .picture为固定宽度 */
        height: 200px;
        margin: 0 4px 5px;
        background-color: #ddd;
    }
    ```

  - IE10 下效果如下图所示

    ![ie10](http://p7e35vgip.bkt.clouddn.com/afxms-ie10.png)

  - 现代浏览器下效果如下图所示

    ![现代浏览器下](http://p7e35vgip.bkt.clouddn.com/afxm-m.png)

##### 元素宽度为百分数

- **方法一，使用float+ 将第一、第五的`margin-left`设置为0 + 将第四、第八的`margin-right`设置为0且元素宽度为百分数（可以兼容到 IE5），如下**

  - html代码如下

    ```html
    <div class="pictures clearfix">
        <div class="picture p1"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture p4"></div>
        <div class="picture p5"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture p8"></div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: table;
        clear: both;
    }
    
    .clearfix:after { /* hack IE8 */
        content: '';
        display: table;
        clear: both;
    }
    
    .clearfix {
        zoom: 1; /* hack IE6 IE7 */
    }
    
    .pictures {
        width: 80%;
        background-color: pink;
    }
    
    .picture {
        float: left;
        width: 23.95%;
        height: 200px;
        margin: 0 0.7% 5px;
        background-color: #ddd;
    }
    
    .p1,.p5 {
        margin-left: 0;
    }
    
    .p4,.p8 {
        margin-right: 0;
    }
    
    /* 可以使用:nth-child选择器来简化，此时可兼容到 IE9 */
    /* 将第一个、第五个的左边距设置为0 */
    .picture:nth-child(4n + 1) {
        margin-left: 0;
    }
    
    /* 将第四个、第五个的右外边距设置为0 */
    .picture:nth-child(4n) {
        margin-right: 0;
    }
    ```

  - IE5 下效果如下图所示

    ![ie5](http://p7e35vgip.bkt.clouddn.com/afe-ie5.png)

  - 使用`:nth-child()`后 IE9 下效果如下图所示

    ![ie9](http://p7e35vgip.bkt.clouddn.com/afe-ie9.png)

  - 现代浏览器下效果如下图所示

    ![现代浏览器](http://p7e35vgip.bkt.clouddn.com/afe-m.png)

- **方法二，使用float + 负margin且元素宽度为百分数及外边距也是百分数（可以兼容到IE5），如下**

  - html代码如下

    ```html
    <div class="pictures">
        <!-- 在所有picture的外边新增了一层 -->
        <div class="xxx clearfix">
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
        </div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    .clearfix:after { /* hack IE8 */
        content: '';
        display: table;
        clear: both;
    }
    
    .clearfix {
        zoom: 1; /* hack IE6 IE7 */
    }
    
    /* .pictures占整个页面的80% */
    .pictures {
        width: 80%;
        background-color: pink;
    }
    
    /* 在新增的.xxx上将左右margin设置为负值，其值的绝对值等于.picture的左右外边距 */
    .xxx {
        margin-left: -0.5%;
        margin-right: -0.5%;
    }
    
    /* 每个.picture占24% */
    /* 又由于每行有8个.picture，则每个.picture的左右外边距为0.5% */
    .picture {
        float: left;
        width: 24%;
        height: 200px;
        margin: 0 0.5% 5px;
        background-color: #ddd;
    }
    
    
    /* 还可以通过calc()来计算每个.picture需要占用的宽度，可以兼容到IE9 */
    /* 这样就不用计算，一行四个.pircture，每个.picture占25% */
    /* 又由于每个.picture还有左右margin共8px */
    /* 则每个.picture的实际width: calc(25% - 8px) */
    .picture {
        float: left;
        width: calc(25% - 8px); /* 使用calc来计算值 */
        height: 200px;
        margin: 0 4px 5px;
        background-color: #ddd;
    }
    ```

  - IE5 下效果如下图所示

    ![ie5](http://p7e35vgip.bkt.clouddn.com/afme-ie5.png)

  - 效果如下图所示

    ![现代浏览器](http://p7e35vgip.bkt.clouddn.com/afm-6.png)

- **方法三，使用flex布局且元素宽度为百分数（可以兼容到 IE10），如下**

  - html代码如下

    ```html
    <div class="pictures">
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
        <div class="picture"></div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    /* 在.pictures上设置flex 布局 */
    .pictures {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        width: 80%;
        margin: 0 auto;
        background-color: pink;
    }
    
    .picture {
        width: calc(25% - 5px);
        height: 200px;
        margin-bottom: 5px;
        background-color: #ddd;
    }
    ```

  - IE10 下效果如下图所示

    ![ie10](http://p7e35vgip.bkt.clouddn.com/afxe-ie10.png)

  - 现代浏览器下效果如下图所示

    ![现代浏览器](http://p7e35vgip.bkt.clouddn.com/aflex.png)

  - 但是这个有一个bug，比如当不是8个图片时，是六个时，效果如下图所示（解决办法可以使用下一个方法）

    ![](http://p7e35vgip.bkt.clouddn.com/aflex-b.png)

- **方法四，使用flex布局 + 负外边距且元素宽度为百分数（可以兼容到 IE10），如下**

  - html代码如下

    ```html
    <div class="pictures">
        <!-- 在所有picture的外边新增了一层 -->
        <div class="xxx">
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
            <div class="picture"></div>
        </div>
    </div>
    ```

  - css代码如下

    ```css
    .clearfix::after {
        content: '';
        display: block;
        clear: both;
    }
    
    .pictures {
        width: 80%;
        margin: 0 auto;
        background-color: pink;
    }
    
    /* 在.xxx上设置flex 布局并设置负外边距 */
    /* 在新增的.xxx上将左右margin设置为负值，其值的绝对值等于.picture的左右外边距 */
    .xxx {
        display: flex;
        flex-wrap: wrap;
        margin-left: -4px;
        margin-right: -4px;
    }
    
    .picture {
        width: calc(25% - 8px); /* .picture的实际宽度 = 25% - 左右外边距之和8px */
        height: 200px;
        margin: 0 4px 5px;
        background-color: #ddd;
    }
    
    /* 也可以将.picture设置为24%，则左右外边距平分4%，如下 */
    .xxx {
        display: flex;
        flex-wrap: wrap;
        margin-left: -0.5%;
        margin-right: -0.5%;
    }
    
    .picture {
        width: 24%;
        height: 200px;
        margin: 0 0.5% 5px;
        background-color: #ddd;
    }
    ```

  - 现代浏览器下效果如下图所示

    ![](http://p7e35vgip.bkt.clouddn.com/aflex-m.png)

### 实现如下PC页面布局

##### PC页面如下图所示

![pc-page](http://p7e35vgip.bkt.clouddn.com/pc-page.png)

##### 要支持 IE8

- 直接宽度固定，如下代码（其实可以兼容到 IE6）

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="utf-8">
          <title>CSS PC页面模型-固定-可兼容到 IE8</title>
          <style>
              /* reset css */
              html {
                  min-width: 1000px;
              }
              body {
                  margin: 0;
                  background-color: #F7F7F7;
                  font-size: 16px;
              }
              ul {
                  margin: 0;
                  padding-left: 0;
              }
              li {
                  list-style-type: none;
              }
  
              /* common css */
              .layout {
                  width: 900px;
                  margin: 0 auto;
              }
              .clearfix {
                  zoom: 1; /* hack IE6 IE7 */
              }
              .clearfix:after { /* hack IE8*/
                  content: '';
                  display: block;
                  clear: both;
              }
              .clearfix::after {
                  content: '';
                  display: block;
                  clear: both;
              }
  
              /* header css */
              .header {
                  background-color: #ddd;
              }
              .header .logo {
                  float: left;
                  width: 100px;
                  height: 40px;
                  line-height: 40px;
                  text-align: center;
                  background-color: #333;
                  color: #fff;
              }
              .header .nav {
                  float: right;
              }
              .header .nav .nav-item {
                  float: left;
                  margin-right: 30px;
                  margin-left: 30px;
                  padding: 8px 0 8px 0;
                  line-height: 24px;
                  cursor: pointer;
              }
  
              /* content css */
              .content .banner-content {
                  width: 900px; /* .bannner-content宽度固定 */
                  height: 300px;
                  margin-top: 5px;
                  line-height: 300px;
                  text-align: center;
                  background-color: #888;
                  color: #fff;
                  font-size: 24px;
              }
              .content .pictures {
                  width: 900px; /* .pictures宽度固定 */
                  margin-top: 5px;
              }
              .content .pictures .pic-list {
                  margin-left: -4px;
                  margin-right: -4px;
              }
              .content .pictures .pic-item {
                  float: left;
                  width: 219px; /* .pic-item宽度固定 */
                  height: 200px;
                  margin: 0 4px 5px;
                  line-height: 200px;
                  text-align: center;
                  font-size: 18px;
                  background-color: #DADADA;
              }
              .content .art {
                  width: 900px;
                  color: #fff;
              }
              .content .art .small-part {
                  float: left;
                  width: 300px;
              }
              .content .art .small-part > .sp-content {
                  height: 200px;
                  margin-right: 25px;
                  background-color: #B0B0B0;
                  line-height: 200px;
                  text-align: center;
              }
              .content .art .big-part {
                  float: left;
                  width: 600px;
              }
              .content .art .big-part > .bp-content {
                  height: 200px;
                  background-color: #808080;
                  line-height: 200px;
                  text-align: center;
              }
  
              /* footer css */
              .footer {
                  height: 30px;
                  margin-top: 5px;
                  line-height: 30px;
                  background-color: #ddd;
                  text-align: center;
                  font-size: 14px;
              }
          </style>
      </head>
      <body>
          <div class="header clearfix">
              <div class="logo">logo</div>
              <div class="nav">
                  <ul class="nav-list clearfix">
                      <li class="nav-item">nav1</li>
                      <li class="nav-item">nav2</li>
                      <li class="nav-item">nav3</li>
                      <li class="nav-item">nav4</li>
                      <li class="nav-item">nav5</li>
                  </ul>
              </div>
          </div>
  
          <div class="content">
              <div class="layout">
                      <div class="banner">
                          <div class="banner-content">banner</div>
                      </div>
          
                      <div class="pictures">
                          <ul class="pic-list clearfix">
                              <!-- 所有的.pic-item都是用于做布局， 实际内容在其里面填充-->
                              <li class="pic-item">picture 1</li>
                              <li class="pic-item">picture 2</li>
                              <li class="pic-item">picture 3</li>
                              <li class="pic-item">picture 4</li>
                              <li class="pic-item">picture 5</li>
                              <li class="pic-item">picture 6</li>
                              <li class="pic-item">picture 7</li>
                              <li class="pic-item">picture 8</li>
                          </ul>
                      </div>
          
                      <div class="art clearfix">
                          <!-- .small-par和.big-part都是用于布局，内容在其里面填充，比如.sp-content -->
                          <div class="small-part">
                              <div class="sp-content">AD 1</div>
                          </div>
                          <div class="big-part">
                              <div class="bp-content">AD 2</div>
                          </div>
                      </div>
              </div>
          </div>
  
          <div class="footer">website from xxx to yyy</div>
      </body>
  </html>
  ```

- 元素宽度使用百分数弹性布局也能支持 IE8，如下

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="utf-8">
          <title>CSS PC页面模型-弹性-可兼容到 IE6</title>
          <style>
              /* reset css */
              html {
                  min-width: 640px; /* 当页面缩小时有个最小宽度避免让.header和.footer错乱，同时可以让.content中的.layout可以居中显示 */
              }
              body {
                  margin: 0;
                  background-color: #F7F7F7;
                  font-size: 16px;
              }
              ul {
                  margin: 0;
                  padding-left: 0;
              }
              li {
                  list-style-type: none;
              }
  
              /* common css */
              .layout {
                  width: 80%;
                  min-width: 512px;
                  margin: 0 auto;
              }
              .clearfix {
                  zoom: 1; /* hack IE6 IE7 */
              }
              .clearfix:after { /* hack IE8*/
                  content: '';
                  display: block;
                  clear: both;
              }
              .clearfix::after {
                  content: '';
                  display: block;
                  clear: both;
              }
  
              /* header css */
              .header {
                  background-color: #ddd;
              }
              .header .logo {
                  float: left;
                  width: 100px;
                  height: 40px;
                  line-height: 40px;
                  text-align: center;
                  background-color: #333;
                  color: #fff;
              }
              .header .nav {
                  float: right;
              }
              .header .nav .nav-item {
                  float: left;
                  padding: 8px 30px;
                  line-height: 24px;
                  cursor: pointer;
              }
  
              /* content css */
              .content .banner {
                  margin-top: 5px;
              }
              .content .banner-content {
                  height: 300px;
                  line-height: 300px;
                  text-align: center;
                  background-color: #888;
                  color: #fff;
                  font-size: 24px;
              }
              .content .pictures {
                  margin-top: 5px;
              }
              .content .pictures .pic-list {
                  margin-left: -0.5%;
                  margin-right: -0.5%;
              }
              .content .pictures .pic-item {
                  float: left;
                  width: 24%;
                  height: 200px;
                  margin: 0 0.5% 5px;
                  background-color: #DADADA;
                  line-height: 200px;
                  text-align: center;
                  font-size: 18px;
              }
              .content .art {
                  color: #fff;
              }
              .content .art .small-part {
                  float: left;
                  width: 33.333333%;
              }
              .content .art .small-part > .sp-content {
                  height: 200px;
                  margin-right: 20px;
                  background-color: #B0B0B0;
                  line-height: 200px;
                  text-align: center;
              }
              .content .art .big-part {
                  float: left;
                  width: 66.666666%;
              }
              .content .art .big-part > .bp-content {
                  height: 200px;
                  background-color: #808080;
                  line-height: 200px;
                  text-align: center;
              }
  
              /* footer css */
              .footer {
                  height: 30px;
                  margin-top: 5px;
                  line-height: 30px;
                  background-color: #ddd;
                  text-align: center;
                  font-size: 14px;
              }
          </style>
      </head>
      <body>
          <div class="header clearfix">
              <div class="logo">logo</div>
              <div class="nav">
                  <ul class="nav-list clearfix">
                      <li class="nav-item">nav1</li>
                      <li class="nav-item">nav2</li>
                      <li class="nav-item">nav3</li>
                      <li class="nav-item">nav4</li>
                      <li class="nav-item">nav5</li>
                  </ul>
              </div>
          </div>
  
          <div class="content">
              <div class="layout">
                  <div class="banner">
                      <div class="banner-content">banner</div>
                  </div>
      
                  <div class="pictures">
                      <ul class="pic-list clearfix">
                          <!-- 所有的.pic-item都是用于做布局， 实际内容在其里面填充-->
                          <li class="pic-item">picture 1</li>
                          <li class="pic-item">picture 2</li>
                          <li class="pic-item">picture 3</li>
                          <li class="pic-item">picture 4</li>
                          <li class="pic-item">picture 5</li>
                          <li class="pic-item">picture 6</li>
                          <li class="pic-item">picture 7</li>
                          <li class="pic-item">picture 8</li>
                      </ul>
                  </div>
      
                  <div class="art clearfix">
                      <!-- .small-par和.big-part都是用于布局，内容在其里面填充，比如.sp-content -->
                      <div class="small-part">
                          <div class="sp-content">AD 1</div>
                      </div>
                      <div class="big-part">
                          <div class="bp-content">AD 2</div>
                      </div>
                  </div>
              </div>
          </div>
  
          <div class="footer">website from xxx to yyy</div>
      </body>
  </html>
  ```

##### 不需要支持 IE8

- 直接flex布局走起，如下

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="utf-8">
          <title>CSS 布局-弹性-flex</title>
          <style>
              /* reset css */
              html {
                  min-width: 640px; /* 当页面缩小时有个最小宽度避免让.header和.footer错乱，同时可以让.content中的.layout可以居中显示 */
              }
              body {
                  margin: 0;
                  background-color: #F7F7F7;
                  font-size: 16px;
              }
              ul {
                  margin: 0;
                  padding-left: 0;
              }
              li {
                  list-style-type: none;
              }
  
              /* common css */
              .flex-center {
                  display: flex;
                  justify-content: center;
                  align-items: center;
              }
              .layout {
                  width: 80%;
                  min-width: 512px;
                  margin: 0 auto;
              }
  
              /* header css */
              .header {
                  display: flex;
                  justify-content: space-between;
                  background-color: #ddd;
              }
              .header .logo {
                  width: 100px;
                  background-color: #333;
                  color: #fff;
              }
              .header .nav-list {
                  display: flex;
              }
              .header .nav-list .nav-item {
                  padding: 8px 30px;
                  line-height: 24px;
                  cursor: pointer;
              }
  
              /* content css */
              .content .banner {
                  margin-top: 5px;
              }
              .content .banner .banner-content{
                  height: 300px;
                  background-color: #888;
                  font-size: 24px;
                  color: #fff;
              }
              .content .pictures {
                  margin-top: 5px;
              }
              .content .pictures .xxx{
                  display: flex;
                  flex-wrap: wrap;
                  margin-left: -0.5%;
                  margin-right: -0.5%;
              }
              .content .pictures .pic-item {
                  width: 24%;
                  height: 200px;
                  margin: 0 0.5% 5px;
                  background-color: #dadada;
              }
              .content .art {
                  display: flex;
                  justify-content: space-between;
              }
              .content .art .small-part {
                  width: 32%;
              }
              .content .art .small-part > .sp-content {
                  height: 200px;
                  background-color: #B0B0B0;
                  color: #fff;
              }
              .content .art .big-part {
                  width: 66%;
              }
              .content .art .big-part > .bp-content {
                  height: 200px;
                  background-color: #808080;
                  color: #fff;
              }
  
              /* footer css */
              .footer {
                  height: 30px;
                  background-color: #ddd;
                  font-size: 14px;
              }
          </style>
      </head>
      <body>
          <header class="header">
              <div class="logo flex-center">logo</div>
              <nav class="nav">
                  <ul class="nav-list">
                      <li class="nav-item">nav1</li>
                      <li class="nav-item">nav2</li>
                      <li class="nav-item">nav3</li>
                      <li class="nav-item">nav4</li>
                      <li class="nav-item">nav5</li>
                  </ul>
              </nav>
          </header>
  
          <main class="content">
              <div class="layout">
                  <section class="banner">
                      <div class="banner-content flex-center">banner</div>
                  </section>
  
                  <section class="pictures">
                      <ul class="pic-list">
                          <div class="xxx">
                              <li class="pic-item flex-center">picture 1</li>
                              <li class="pic-item flex-center">picture 2</li>
                              <li class="pic-item flex-center">picture 3</li>
                              <li class="pic-item flex-center">picture 4</li>
                              <li class="pic-item flex-center">picture 5</li>
                              <li class="pic-item flex-center">picture 6</li>
                              <li class="pic-item flex-center">picture 7</li>
                          </div>
                      </ul>
                  </section>
  
                  <section class="art">
                      <div class="small-part">
                          <div class="sp-content flex-center">AD 1</div>
                      </div>
                      <div class="big-part">
                          <div class="bp-content flex-center">AD 2</div>
                      </div>
                  </section>
              </div>
          </main>
  
          <footer class="footer flex-center">website from xxx to yyy</footer>
      </body>
  </html>
  ```

##### 实现如下手机页面布局

