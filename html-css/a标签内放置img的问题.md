# a元素内放置img时，a元素比img高一点的问题

- 在a元素内放置img时，会出现如下情况

  ![](http://p7e35vgip.bkt.clouddn.com/a-img.jpg)

- 从上图可以看见a标签的高度要比img稍微高一点，造成的原因如下：

  > a标签下有一个匿名文本，该匿名文本外有一个匿名行级盒子，它有vertical-align属性且是默认值baseline。而往往因为上下文line-height的影响，使它有个line-height，从而使其有了高度。又由于baseline对齐的原因，这个匿名行级盒子要下沉，会往下撑开一些距离，所以把a撑高了

- 解决办法1--->消除掉匿名行级盒子的高度 ，即给a元素设置`display: inline-block;line-height: 0`或者`font-size: 0`。效果如下图所示

  ![](http://p7e35vgip.bkt.clouddn.com/a-imgs1.jpg)

  ![](http://p7e35vgip.bkt.clouddn.com/a-imgs2.jpg)

- 解决办法2--->可以给a元素或者img元素设置`vertical-align: top`，让它们分别是基于`top`对齐。效果如下图所示

  ![](http://p7e35vgip.bkt.clouddn.com/a-imgs3.jpg)

  ![](http://p7e35vgip.bkt.clouddn.com/a-imgs4.jpg)

- 解决办法3--->给`img`元素设置`display:block`，让它和匿名行级盒子不在一个布局上下文中，也就不存在行级盒的对齐问题 。效果如下图所示

  ![](http://p7e35vgip.bkt.clouddn.com/a-imgs5.jpg)