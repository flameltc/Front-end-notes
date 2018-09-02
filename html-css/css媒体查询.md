# CSS媒体查询

### 什么是媒体查询

- **`@media`包含一个可选的媒体类型和根据表示媒体特性的零个或多个表达式是否为true来应用样式**，这些表达式最终会被解析为true或false。如果媒体查询中指定的媒体类型匹配展示文档所使用的设备类型，并且所有的表达式的值都是true，那么该媒体查询的结果为true，则会应用样式
- **`@media`媒体查询**的媒体类型用于描述设备的一般类别，当没有选择媒体类型时默认使用`all`类型
  - `all`--->适用于所有设备
  - `screen`--->主要适用于彩色的电脑屏幕 
  - `print`--->用于打印机
- **`@media`媒体查询**的media feature用于针对每个浏览器和不同设备都有各自的媒体特性 ，常见如下
  - `width`--->可视宽度 
  - `height`--->可视高度 
  - `aspect-ratio`--->定义输出设备中的页面可见区域宽度与高度的比率 
  - `orientation`--->定义输出设备中的页面可见区域高度是否大于或等于宽度
  - `resolution`--->定义设备的分辨率
  - `scan`--->定义电视类设备的扫描工序 
  - `color`--->定义输出设备每一组彩色原件的个数。如果不是彩色设备，则值等于0 
  - `color-index`--->定义在输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则值等于0
  - `grid`--->用来查询输出设备是否使用栅格或点阵 

### 媒体查询基本语法

- **`link`标签中使用媒体查询，如下代码**

  ```html
  <!--link 标签中css媒体查询-->
  <link rel="stylesheet" media="screen and (max-width:900px)" href="./css/xxx.css">
  ```

- **css 里使用媒体查询，如下代码**

  ```css
  /* 样式表中css媒体查询 */
  @media (max-width: 320px) { /* 0 ~ 320px */
      body {
          background-color: pink;
      }
  }
  @media (min-width: 321px) and (max-width: 375px) { /* 321px ~ 375px */
      body {
          background-color: orange;
      }
  }
  @media (min-width: 376px) and (max-width: 425px) { /* 376px ~ 425px */
      body {
          background-color: green;
      }
  }
  @media (min-width: 426px) and (max-width: 640px) { /* 426px ~ 640px */
      body {
          background-color: blue;
      }
  }
  @media (min-width: 641px) { /* 641px ~ ??? */
      body {
          background-color: yellow;
      }
  }
  ```

### 逻辑操作符

- **`and`操作符**--->用于合并多个媒体属性或者合并媒体属性和媒体类型，只有媒体类型和媒体属性都返回true，其包含的css才有效

  ```css
  /* 表示媒体查询类型为 all 且媒体特征最小宽度为900px */
  @media (min-width: 900px) {
      body {
          background-color: pink;
      }
  }
  
  
  /* 表示媒体查询类型为 screen 且媒体特征为宽度321px到375px */
  @media screen and (min-width: 321px) and (max-width: 375px) {
      body {
          background-color: green;
      }
  }
  
  /* 表示媒体查询仅在电视媒体上，可视区域不小于700像素宽度并且是横屏时有效 */
  @meida tv and (min-width: 700px) and (orientation: landscape) {
    
  }
  ```

- **逗号分隔（等同于or操作符）**--->此时，任何一个媒体查询返回真，样式就是有效的

  ```css
  /* 最小宽度为700px或是横屏的手持设备上应用一组样式 */
  @media (min-width: 900px), handheld and (orientation: landscape) {
      
  }
  ```

- **not操作符**--->用来对整个媒体查询的结果进行取反，**必须指定媒体类型**

  ```css
  /* 下面媒体查询其实是对not (all and (monochrome)) 进行查询*/
  @media not all and (monochrome) {
      
  }
  ```

- **only操作符**--->防止老旧的浏览器不支持带媒体属性的查询而应用到给定的样式

  ```css
  <link rel="stylesheet" media="only screen and (color)" href="xxx.css">
  ```
