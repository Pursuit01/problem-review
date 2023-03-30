### 问题：
当设置父元素设置`justify-content: space-between;`使子元素两端对齐时，最后一行如果个数不足，会出现偏移。<br />如图所示：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/23035706/1680055401897-f51da0ce-8769-417e-81e7-dbbae4450c4e.png#averageHue=%23fefefd&clientId=u0775e681-681f-4&from=paste&height=351&id=u17cdbb9e&name=image.png&originHeight=351&originWidth=1631&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14007&status=done&style=shadow&taskId=u7c9efc25-3faa-4306-b96d-e2be049ecb9&title=&width=1631)<br />**尝试：**

1. 动态添加空元素。比如每行显示5个，最后一行只有3个时，动态push2个空元素并设置标识符isAdd:true，当遍历时再将isAdd为true的元素设置隐藏。这种方法的缺陷在于，如果调整屏幕分辨率，使每行显示的个数发生变化时（展示6个或4个，此时15 / 6又会有余数），仍然会出现同样的问题。
2. 遍历元素时，每五个分一组，每组单独使用一个div作为弹性盒子，然后把最后一个弹性盒子的`justify-content`属性设备`start`，前面的弹性盒子依旧是`space-between`。缺点是生成的DOM树不够清晰。

可见以上两种方法都存在问题，最后发现ElementUI组件库的`<el-row :gutter='20'>`搭配`<el-col :span='4'>`，可以满足需求。于是查看了el组件的源码，发现了它的实现方式：

1. 首先，在子元素中，首先获取父元素设置的gutter属性，将它除以2之后作为子元素的padding-left和padding-right，如下图：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/23035706/1680056041276-0db96aa3-189b-4984-bb65-ab1e874c0a0e.png#averageHue=%23fefefd&clientId=u0775e681-681f-4&from=paste&height=98&id=u01f6aef8&name=image.png&originHeight=98&originWidth=353&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4693&status=done&style=shadow&taskId=uba68e47f-7396-49bc-b963-0d41c1c0a5b&title=&width=353)

2. 接着再将父元素的`margin-left`设为负的gutter的一半，这样相当于将第一个子元素的`padding-left`抵消掉了，以此保证两边依然是对齐的，垂直方向原理也是一样的。如下图： 

![image.png](https://cdn.nlark.com/yuque/0/2023/png/23035706/1680056449311-c59b02f4-d699-412e-8c4e-1b158b15f78f.png#averageHue=%23fefefd&clientId=u0775e681-681f-4&from=paste&height=114&id=uf0cad288&name=image.png&originHeight=96&originWidth=318&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4637&status=done&style=shadow&taskId=ub50e080a-1ea0-4181-b94f-6fa1e7505d8&title=&width=377)

