# flex

### 什么是flex布局

- flex--->flexible box，即flex布局

- **任何一个容器都可以指定为flex布局**

  ```css
  /* 块级元素 */
  div {
      display: flex;
  }
  /* 行内元素 */
  行内元素 {
      display: inline-flex;
  }
  ```

- **元素设置flex后，其子元素的float、clear、vertical-align等属性都将失效**

- `flex`元素的兼容性如下图

  ![flex兼容性](http://p7e35vgip.bkt.clouddn.com/flex-c.png)

### flex布局基本概念

- 基本概念如下图所示

  ![flex概念图](http://p7e35vgip.bkt.clouddn.com/flexbox.png)

- 通过设置`display`属性的值为`flex`或者`inline-flex`元素称为flex container，即flex 容器；flex container的所有子元素自动成为其成员--flex item，即flex 项目

- flex container默认两根轴：

  - flex item沿其依次排列的那根轴称为**主轴**；垂直于**主轴**的那根轴称为**侧轴**
  - 主轴开始的位置为main start，结束的位置main end
  - 侧轴开始的位置cross start，结束的位置cross end

- **项目默认沿着主轴排列**，在主轴上占据的空间为main size，在侧轴上占据的空间为cross size

### flex container 的属性

- **flex-direction**

  - **`flex-direction`指定了flex 项目是如何在flex 容器中布局的，它决定主轴的方向，即flex 项目的排列方向**

  - `flex-direction`语法如下

    ```
    flex-direction: row(默认) | row-reverse | column | column-reverse
    
    row--->默认值，主轴为水平方向，从左到右
    
    row-reverse--->主轴为水平方向，从右到左
    
    column--->主轴为垂直方向，从上到下
    
    column-reverse--->主轴为垂直方向，从下到上
    ```

  - 示例一，flex 容器设置`flex-direction: row` ，效果如下图所示

    ![fd-r](http://p7e35vgip.bkt.clouddn.com/fd-r.png)

  - 示例二，flex 容器设置`flex-direction: row-reverse`，效果如下图所示

    ![fd-rv](http://p7e35vgip.bkt.clouddn.com/fd-rv.png)

  - 示例三，flex 容器设置`flex-direction: column`，效果如下图所示

    ![fd-c](http://p7e35vgip.bkt.clouddn.com/fd-c.png)

  - 示例四，flex 容器设置`flex-direction: column-reverse`，效果如下图所示

    ![fd-cr](http://p7e35vgip.bkt.clouddn.com/fd-cr.png)

- **flex-wrap**

  - **`flex-wrap`指定 flex 项目在一行显示不下时是否可以换行；如果允许换行，可控制行的堆叠方向**

  - `flex-wrap`语法如下

    ```
    flex-wrap: nowrap(默认) | wrap | wrap-reverse
    
    nowrap--->默认值，不换行
    
    wrap--->换行，第一行在上面
    
    wrap-reverse--->换行，第一行在下面
    ```

  - 示例一，flex 容器设置`flex-wrap: nowrap`，则flex 项目都在一行且有可能会溢出，效果如下图所示

    ![fw-n](http://p7e35vgip.bkt.clouddn.com/fw-n.png)

  - 示例二，flex 容器设置`flex-wrap: wrap`，则flex 项目可以换行且第一行在上面，效果如下图所示

    ![fw-w](http://p7e35vgip.bkt.clouddn.com/fw-w.png)

  - 示例三，flex 容器设置`flex-wrap: wrap-reverse`，则flex 项目可以换行且第一行在下面，效果如下图所示

    ![fw-wr](http://p7e35vgip.bkt.clouddn.com/fw-wr.png)

- **flex-flow**

  - **`flex-flow`是flex-direction和flex-wrap的简写**

  - `flex-flow`语法如下

    ```
    flex-flow: <flex-direction> | <flex-wrap>
    ```

- **justify-content**

  - **`justify-content`定义如何分配在主轴方向上的flex 项目之间及其周围的空间**

  - `justify-content`语法如下

    ```
    justify-content: flex-start | flex-end | center | space-betwwen |space-around | space-evenly
    
    flex-start--->flex 项目从行首开始排列
    
    flex-end--->flex 项目从行尾开始排列
    
    center--->flex 项目居中排列
    
    space-betwwen--->每行第一个元素与行首对齐，每行最后一个元素与行尾对齐(即flex 项目头尾
                     两端紧挨容器)；flex 项目之间距离都相等
                     
    space-around--->每行第一个元素到行首的距离和每行最后一个元素到行尾的距离
                    将会是相邻元素之间距离的一半；flex 项目之间距离都相等
                    
    space-evenly--->flex 项目都沿着主轴均匀分布在指定的对齐容器中；flex 项目左右两边距离都相等
    ```

  - 示例一，flex 容器设置`justify-content: flex-start`，效果如下图所示

    ![jc-fs](http://p7e35vgip.bkt.clouddn.com/jc-fs.png)

  - 示例二，flex 容器设置`justify-content: flex-end`，效果如下图所示

    ![jc-fe](http://p7e35vgip.bkt.clouddn.com/jc-fe.png)

  - 示例三，flex 容器设置`justify-content: center`，效果如下图所示

    ![jc-c](http://p7e35vgip.bkt.clouddn.com/jc-c.png)

  - 示例四，flex 容器设置`justify-content: space-betwwen`，效果如下图所示

    ![jc-sb](http://p7e35vgip.bkt.clouddn.com/jc-sb.png)

  - 示例五，flex 容器设置`justify-content: space-around`，效果如下图所示

    ![jc-sa](http://p7e35vgip.bkt.clouddn.com/jc-sa.png)

  - 示例六，flex 容器设置`justify-content: space-evenly`，效果如下图所示

    ![jc-se](http://p7e35vgip.bkt.clouddn.com/jc-se.png)

- **align-items**

  - **`align-items`指定了当前行flex 项目自身在侧轴上的对齐方式**

  - `align-items`语法如下

    ```
    align-items: stretch(默认) | flex-start | flex-end  | center | baseline
    
    stretch--->默认值，如果flex 项目未设置高度或设为auto，则flex 项目占满整个容器高度
    
    flex-start--->flex 项目在侧轴的起点对齐
    
    flex-end--->flex 项目在侧轴的终点对齐
    
    center--->flex 项目在侧轴的中点对齐
    
    baseline--->flex 项目在侧轴上对齐位置是其第一行文字的基线位置
    ```

  - 示例一，flex 容器设置`align-items: stretch`，效果如下图所示

    ![ai-s](http://p7e35vgip.bkt.clouddn.com/ai-s.png)

  - 示例二，flex 容器设置`align-items: flex-start`，效果如下图所示

    ![ai-fs](http://p7e35vgip.bkt.clouddn.com/ai-fs.png)

  - 示例三，flex 容器设置`align-items: flex-end`，效果如下图所示

    ![ai-fe](http://p7e35vgip.bkt.clouddn.com/ai-fe.png)

  - 示例四，flex 容器设置`align-items: center`，效果如下图所示

    ![ai-c](http://p7e35vgip.bkt.clouddn.com/ai-c.png)

  - 示例五，flex 容器设置`align-items: baseline`，效果如下图所示

    ![ai-b](http://p7e35vgip.bkt.clouddn.com/ai-b.png)

- **align-content**

  - **`align-content`定义了多根轴线存在时在侧轴上的对齐方式；如果只有一根轴线，则无效；一行为一根轴线**

  - `align-content`语法如下

    ```
    align-content: stretch | flex-start | flex-end | center | space-betwwen | space-around
    
    stretch--->拉伸所有行来填满剩余空间；剩余空间平均地分配给每一行
    
    flex-start--->所有行从侧轴起点开始填充；
                  第一行的顶部和容器的侧轴起点边对齐；
                  接下来的每一行紧跟前一行
                  
    flex-end--->所有行从侧轴终点开始填充；
                最后一行的底部和容器的侧轴终点对齐；
                同时所有后续行都紧跟前一行
    
    center--->所有行朝向容器的中心填充；
              每行互相紧挨，相对于容器居中对齐；
              容器的侧轴起点边和第一行的距离相等于容器的侧轴终点边和最后一行的距离
    
    space-between--->容器的侧轴起点边和终点边分别与第一行和最后一行的边对齐；
                     所有行在容器中侧轴方向平均分布，相邻两行间距相等
    
    space-around--->容器的侧轴起点边和终点边分别与第一行和最后一行的距离是相邻两行间距的一半；
                    所有行在容器中平均分布，相邻两行间距相等
    ```

  - 示例一，flex 容器设置`align-content: stretch`，效果如下图所示

    ![ac-s](http://p7e35vgip.bkt.clouddn.com/ac-s.png)

  - 示例二，flex 容器设置`align-content: flex-start`，效果如下图所示

    ![ac-fs](http://p7e35vgip.bkt.clouddn.com/ac-fs.png)

  - 示例三，flex 容器设置`align-content: flex-end`，效果如下图所示

    ![ac-fe](http://p7e35vgip.bkt.clouddn.com/ac-fe.png)

  - 示例四，flex 容器设置`align-content: center`，效果如下图所示

    ![ac-c](http://p7e35vgip.bkt.clouddn.com/ac-c.png)

  - 示例五，flex 容器设置`align-content: space-betwwen`，效果如下图所示

    ![ac-sb](http://p7e35vgip.bkt.clouddn.com/ac-sb.png)

  - 示例六，flex 容器设置`align-content: space-around`，效果如下图所示

    ![ac-sa](http://p7e35vgip.bkt.clouddn.com/ac-sa.png)

### flex item 的属性

- **`order`**

  - **`order`定义flex 项目的排列顺序**

  - **`order`属性的值为整数值，数值越小，排列越靠前，默认值为0**

  - `order` 仅仅对元素的视觉顺序产生作用，并不会影响元素的逻辑或 tab 顺序

  - 示例一，当元素都为`order: 0`的情况，元素按照在HTML中的顺序排列，如下图所示

    ![order-0](http://p7e35vgip.bkt.clouddn.com/order-0.png)

  - 示例二，当同级元素为`order: 0`，而目标元素设置为`order: 1`的情况，则目标元素排在最后，如下图所示

    ![order-1](http://p7e35vgip.bkt.clouddn.com/order-1.png)

  - 示例三，当同级元素为`order: 0`，而目标元素设置为`order: -1`的情况，则目标元素排在最前，如下图所示

    ![order--1](http://p7e35vgip.bkt.clouddn.com/order--1.png)

  - 示例四，当`order`属性的值都设置成不同的整数值的情况，则按照值越小越靠前排列，如下图所示

    ![order](http://p7e35vgip.bkt.clouddn.com/order-s.png)

- **flex-grow**

  - **`flex-grow`定义flex 容器在有多余空间时每个flex 项目的放大比例**

  - **`flex-grow`属性的值为大于等于0的number；默认值为0，表示即使存在剩余空间也不会放大**

  - 示例一，当元素都为`flex-grow: 0`的情况，则每个元素的仅占用自身空间，如下图所示

    ![flex-grow为默认值](http://p7e35vgip.bkt.clouddn.com/fg-0.png)

  - 示例二，目标元素为`flex-grow: 1`，而同级元素为`flex-grow: 0`，则目标元素会占据flex 容器的剩余空间，如下图所示

    ![目标元素flex-grow为1](http://p7e35vgip.bkt.clouddn.com/fg-1.png)

  - 示例三，第一个元素为`flex-grow: 1`，第二个元素为`flex-grow: 2`，第三个元素为`flex-grow: 0`，则flex 容器存在的剩余空间会被分成三份，其中第一个元素占1/3，第二个元素占2/3，第三个元素不会分得剩余空间，如下图所示

    ![flex-grow为不同的值](http://p7e35vgip.bkt.clouddn.com/fg-s.png)

- **flex-shrink**

  - **`flex-shrink`定义了每个flex 项目的缩小比例；flex 项目仅在默认宽度之和大于flex 容器的宽度时才会发生收缩，其收缩的大小是依据 flex-shrink 的值**

  - **`flex-shrink`属性的值是大于等于0的number，默认值为1表示若空间不足时则该flex 项目会缩小**

  - **若将flex-shrink设置为0，当空间不足时该flex 项目也不会缩小**

  - 示例一，flex 容器宽度为300px；第一个flex 项目宽度为100px，第二个flex 项目宽度为150px，第三个flex 项目宽度为80px；三个flex 项目皆为`flex-shrink：1`。此时，flex 项目的宽度之和为330px大于了flex 容器的宽度300px，则每个flex 项目都要按照flex-shrink值缩小。flex 项目的实际宽度 = flex 项目的初始宽度 - flex 项目要缩小的宽度。则第一个flex 项目的实际宽度 = 100 - (100 + 150 + 80 - 300) × (100 × 1 / (100 × 1 + 150 × 1 + 80 × 1)) = 100 - 9.091= 90.909px。同理，第二个flex 项目的实际宽度 = 150 - (100 + 150 + 80 - 300) × (150 × 1 / (100 × 1 + 150 × 1 + 80 × 1)) = 150 -  13.636 = 136.364px。同理，第三个flex 项目实际宽度 = 80px - (100 + 150 + 80 - 300) × (80 × 1 / (100 × 1 + 150 × 1 + 80 × 1)) = 80 - 7.273 = 72.727px。效果如下图所示

    ![fx-1](http://p7e35vgip.bkt.clouddn.com/fs-11.png)

  - 示例二，在示例一中同等的情况下只把第三个项目的`flex-shrink`设置为0。则此时，第三个flex 项目的宽度是不会缩小的；而第一个flex 项目的实际宽度 = 100 - (100 + 150 + 80 - 300) c (100 × 1 / (100 × 1 + 150 × 1 + 80 × 0)) = 100 -12 = 88px；第二个flex 项目的实际宽度 = 100 - (100 + 150 + 80 - 300) ×  (150 × 1 / (100 × 1 + 150 × 1 + 80 × 0)) = 100 - 18 = 132px。效果如下图所示

    ![fs-0](http://p7e35vgip.bkt.clouddn.com/fs-0.png)

- **flex-basis**

  - **`flex-basis`指定了 flex 项目在主轴方向上的初始大小**

  - 示例一，通过给元素设置`flex-basis: 200px`来指定了元素的初始宽度为200px。效果如下图所示

    ![fb](http://p7e35vgip.bkt.clouddn.com/fb.png)

- **flex**

  - `flex`是flex-grow、flex-shrink、flex-basis的简写，后两个属性可选，默认值为0 1 auto

- **align-self**

  - **`aligin-self`允许单个flex 项目有与其主轴所在方向的其它flex 项目在侧轴上有不一样的对齐方式，可覆盖align-items属性**

  - **`align-self`默认值为auto，表示继承父元素的`align-item`属性，如果没有父元素，则等同于stretch；其余属性如下**

    ```
    .item {
        align-self: auto | flex-start | flex-end | center | baseline | stretch;
    }
    ```

  - 示例一，目标元素为`align-self: auto`，则它会使用父元素的`align-items: center`，如下图所示

    ![as-a](http://p7e35vgip.bkt.clouddn.com/as-a.png)

  - 示例二，目标元素为`align-self: flex-start`且其父元素为`align-items: center`，效果如下图所示

    ![as-fs](http://p7e35vgip.bkt.clouddn.com/as-fs.png)

  - 示例三，目标元素为`align-self: flex-end`且其父元素为`align-items: center`，效果如下图所示

    ![as-fe](http://p7e35vgip.bkt.clouddn.com/as-fe.png)

  - 示例四，目标元素为`align-self: center`且其父元素为`align-items: flex-start`，效果如下图所示

    ![as-c](http://p7e35vgip.bkt.clouddn.com/as-c.png)

  - 示例五，目标元素为`align-self: baseline`且其父元素为`align-items: center`，效果如下图所示

    ![as-b](http://p7e35vgip.bkt.clouddn.com/as-b.png)

  - 示例六，目标元素为`align-self: stretch`且其父元素为`align-items: center`，效果如下图所示

    ![as-s](http://p7e35vgip.bkt.clouddn.com/as-s.png)

### flex 实现圣杯布局

- html 如下

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<meta charset="utf-8">
  		<title>flex 实现圣杯布局</title>
  	</head>
  	<body>
  		<header>header</header>
  		<main>
  			<div class="content">content</div>
  			<nav>nav</nav>
  			<aside>aside</aside>
  		</main>
  		<footer>footer</footer>
  	</body>
  </html>
  ```

- css 如下

  ```css
  /* reset css */
  body {
  	margin: 0;
  }
  
  /* 圣杯布局css */
  header {
  	min-height: 100px;
  	background-color: green;
  }
  main {
  	min-height: 500px;
  	display: flex;
  }
  main > .content {
  	background-color: pink;
  	order: 2;
  	flex: 3 1 60%;
  }
  main > nav {
  	background-color: orange;
  	order: 1;
  	flex: 1 6 20%;
  }
  main > aside {
  	background-color: #F47378;
  	order: 3;
  	flex: 1 6 20%;
  }
  footer {
  	min-height: 70px;
  	background-color: #60C5F1;
  }
  /* 当页面宽度窄到已不足以支持三栏 */
  @media all and (max-width: 640px) {
  	header {
  		min-height: 80px;
  	}
  	main {
  		flex-direction: column;
  	}
  	/* 恢复到文档内的自然顺序 */
  	main > .content, main > nav, main > aside {
  		order: 0;
  	}
  	main > nav, main > aside, footer {
  		min-height: 50px;
  		max-height: 50px;
  	}
  }
  ```

- 效果图如下

  - 正常情况

    ![](http://p7e35vgip.bkt.clouddn.com/flex1.png)

  - 当页面宽度小于640时

    ![](http://p7e35vgip.bkt.clouddn.com/flex2.png)
### flex的踩坑点

- **flex-grow的坑**

  - **flex-grow的宽度分配问题**

    ```css
    /*
     *<div class="parent">
     *	<div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *</div>
     */
    
    /* 没有设置 flex-grow 时的css，此时 .child 的宽度由自身决定，大小是56px */
    div {
      box-sizing: border-box;
    }
    .parent {
        width: 500px;
        display: flex;
        background-color: pink;
    }
    .child {
        height: 50px;
        line-height: 50px;
        text-align: center;
        border: 1px solid white;
        background-color: green;
    }
    
    /* 设置 flex-grow 时的css，此时 .child 的宽度大小是100px */
    div {
      box-sizing: border-box;
    }
    .parent {
        width: 500px;
        display: flex;
        background-color: pink;
    }
    .child {
        height: 50px;
        line-height: 50px;
        text-align: center;
        border: 1px solid white;
        background-color: green;
        flex-grow: 1;
    }
    
    /*
     *当把html换成如下时，css不变
     *<div class="parent">
     *	<div class="child">我是第一个子元素但我要大一些</div>
     *	<div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *  <div class="child">子元素</div>
     *</div>
     */
    
    /* 在不管flex item的自身实际宽度的情况下实现都是等比宽高 */
    div {
      box-sizing: border-box;
    }
    .parent {
        width: 500px;
        display: flex;
        background-color: pink;
    }
    .child {
        height: 50px;
        line-height: 50px;
        text-align: center;
        border: 1px solid white;
        background-color: green;
        flex-grow: 1;
        flex-basis: 0; /* 该属性会使每个flex 项目的初始大小一样 */
        overflow: hidden; /* 隐藏超出的部分文本 */
    }
    ```
    - 没有设置 `flex-grow `时 .child 的宽度是56px，如下图1

      ![没有设置flex-grow时，此时.child宽度56px](https://i.loli.net/2018/04/10/5acb982f7df2a.png)

    - 设置了`flex-grow:1`时 .child 的宽度为100px，如下图2

      ![设置flex-grow时，此时.child宽度100px](https://i.loli.net/2018/04/10/5acb998f4f5ba.png)

    - 将`.child:nth-child(1)`的文本内容写多一点(此时它的自身宽度为254px)，如下图3

      ![将.child:nth-child(1)的文本内容写多一点](https://i.loli.net/2018/04/10/5acb9b323ab93.png)

    - **flex-grow利用的的是基于flex container的宽度 - 每个flex item默认宽度后的空间。**
      - 上面的例子中，图二中flex container剩余的多余空间 =  .parent宽度 - .child宽度 * 5 = 500 - 56 * 5 = 220px。此时每个.child都设置了flex-grow:1，则这5个.child均分220px，则每个.child增加了44px，所以最终.child的宽度为100px
      - 在图三中，.child:nth-child(1)的自身宽度是254px，那么flex container剩余的多余空间 = .parent宽度 - (.child:nth-child(1)宽度 + .child宽度 * 4) = 500px - (254 + 56 * 4) = 22px。而此时每个.child都设置了flex-grow:1，则这5个.child均分22px，则每个.child增加了4.4px，所以最终.child:nth-child(1)的宽度为258.4px，其余四个为60.4px

    - **在不管flex item的自身实际宽度的情况下实现都是等比宽高，则可以设置flex-basis:0来实现**，如下图4

      ![](https://i.loli.net/2018/04/10/5acba03f1201a.png)

  - **设置flex-grow的元素的后代撑破flex container**

    ```css
    /*
     *<div class="parent">
     *  <div class="small">我是small</div>
     *  <div class="big">
     *		<div class="content">
     *  		.big是设置了flex-grow的元素，作为.big的后代元素我将要撑破flex container
     *		</div>
     *	</div>
     *</div>
     */
    
    .parent {
        padding: 50px 20px;
        background-color: pink;
        display: flex;
    }
    .small {
        width: 100px;
        height: 60px;
        line-height: 60px;
        text-align: center;
        background-color: orange;
        flex-shrink: 0; /* 空间不够不会被压缩 */
    }
    .big {
        height: 60px;
        line-height: 60px;
        background-color: green;
        flex-grow: 1;
    }
    /* 设置来不允许.content里面的内容换行显示，当超过时出现省略号 */
    .content {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }
    ```

    - 当要给 .content 里面的内容设置单行省略号时，因为不允许换行撑破了flex container，如下面两图所示![图5](https://i.loli.net/2018/04/10/5acba75cee955.png)                                                               ![图5](https://i.loli.net/2018/04/10/5acba7e44622c.png)

    - **解决方法一：在设置了flex属性的.parent元素下设置`width: 0`**

      ```css
      .big {
          width: 0; /* 在设置了flex-grow属性.big上设置width: 0 */
          height: 60px;
          line-height: 60px;
          background-color: green;
          flex-grow: 1;
      }
      ```

    - **解决方法二：在都设置了flex-grow的元素身上设置`overflow:  hidden`**

      ```
      .big {
          height: 60px;
          line-height: 60px;
          background-color: green;
          flex-grow: 1;
          overflow: hidden;
      }
      ```

    - 以上两种方法都可以达到如下效果

      ![实现省略号](https://i.loli.net/2018/04/10/5acba975411dc.png)

