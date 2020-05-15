# CSS布局与定位概述

[![YDglRK.png](https://s1.ax1x.com/2020/05/14/YDglRK.png)](https://imgchr.com/i/YDglRK)

- 盒子模型——元素什么样
  - 页面元素的大小
  - 边框
  - 与其他元素的距离
- 定位机制——元素放在哪
  - 文档流
  - 浮动定位
  - 层定位

# 盒子模型

页面中的所有元素都可以看成一个盒子，占据着一定页面空间。

- 盒子模型的组成
  - content——内容
  - height——高度
  - width——宽度
  - border——边框
  - padding——内边距
  - margin——外边距

一个盒子的实际宽度、高度：content+padding+border+margin

- overflow属性
  - hidden——超出部分不可见
  - scroll——显示滚动条
  - auto——如果有超出部分，显示滚动条
- border属性
  - border-width
  - border-style
  - border-color
  - border：width style color

**水平线案例：**

```html
.line {
	height:1px;
	width:500px;
	border-top:1px solid #e5e5e5;
}
```

- padding属性
  - padding
  - padding-top
  - padding-right
  - padding-bottom
  - padding-left
- margin属性
  - margin
  - margin-top
  - margin-right
  - margin-bottom
  - margin-left
  - margin的合并：垂直方向合并，水平方向不合并
  - 水平居中
    - 图片、文字水平居中：text-align
    - div水平居中：margin：0 auto；浏览器自动计算

# CSS定位机制

- CSS布局——定位机制
  - 文档流（flow
    - 元素分类
      - block
      - inline
      - inline-block
    - 元素类型转换
      - display
  - 浮动（float
  - 层（layer

### 文档流

- block元素
  - 独占一行
  - 元素的height、width、margin、padding都可以设置
  - 常见元素：div、p、h1、ul、ol、li、table、from
- inline
  - 不单独占用一行
  - width、height不可设置
  - width就是它包含的文字或图片的宽度，不可改变
  - inline元素之间有一个间距问题
  - 常见元素：a、span
- inline-block
  - 就是同时具备inline元素、block元素的特点
  - 不单独占用一行
  - 元素的height、width、margin、padding都可以设置
  - 常见元素：img
- 相互转换
  - display：none——元素不会被显示
  - display：block——显示为block元素
  - display：inline——显示为inline元素
  - display：inline-block——显示为inline-block元素

### 浮动定位

- float属性
  - left——左浮动
  - right——右浮动
  - none——不浮动
- cleat属性
  - both——清除左右两边的浮动
  - left和right只能清除一个方向的浮动
  - none是默认值，只在需要移除已指定的清除值时用到

### 层定位

- position属性
  - static——没有定位
  - fixed——固定定位（相对于**浏览器窗口**进行定位
  - relative——相对定位（相对于其**直接父元素**进行定位（**原位置依然存在**
  - absolute——绝对定位（相对于**static定位以外的第一个父元素**
- left属性
- right属性
- top属性
- bottom属性
- z-index属性——值大在上面

# CSS3

- 圆角边框与阴影
  - border-radius属性
    - border-top-left——左上角的形状
    - border-top-right——右上角的形状
    - border-bottom-left——左下角形状
    - border-bottom-right——右下角形状
  - box-shadow属性
    - insert——内部阴影
    - outset——外部阴影
    - 水平偏移 垂直偏移 模糊距离 颜色
- 文字与文本
  - text-shadow属性：水平偏移 垂直偏移 阴影大小 颜色
  - word-wrap属性
    - normal
    - break-word
  - @font-face规则
- 2D转换——对元素进行旋转、缩放、移动、拉伸
  - 旋转 transform：rotate（）
  - 缩放 transform：scale（）
- 3D变换
  - 3D
    - transform-style：preserve-3d
  - 旋转
    - transform属性
      - rotateX（）
      - rotateY（）
      - rotateZ（）
    - 透视
      - perspective属性

