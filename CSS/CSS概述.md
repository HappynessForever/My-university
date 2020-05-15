# CSS概述

CSS：Cascading Style Sheets层叠样式表

CSS语法：选择器、属性名、属性值、分号、注释

- 最后一条声明可以没有分号，但是为了以后修改方便，一般也加上分号
- 为了使用样式更加容易阅读，可以将每条代码写写在一个新行内

# CSS添加方法

- CSS添加方法——行内
- CSS添加方法——内嵌样式
  - 即使有公共CSS代码，也是每个页面都要定义的
  - 适合文件很少，CSS代码也不多的情况
  - 如果一个网站有很多页面，每个文件都会变大，后期维护难度也大
- CSS添加方法——单独文件
  - 页面结构HTML代码与样式CSS代码完全分离，维护方便
  - 如果需要改变网站风格，只需要修改公共CSS文件
  - 可以在同一个HTML文档内部引用多个外部样式表
- CSS添加方法——优先级
  - 多重样式可以层叠，可以覆盖
  - 样式的优先级按照“就近原则”
  - 行内样式 > 内嵌样式 > 连接样式 > 浏览器默认样式

# CSS选择器

- 标签选择器
- 类别选择器
- ID选择器
- 嵌套声明
- 集体声明
- 全局声明
- 混合

# CSS样式

### 文本

- 单位
  - px
  - em
  - %
- 颜色
  - red、blue
  - rgb
  - rgba
  - 十六进制
- 文本
  - color——文本颜色
  - letter-spacing——字符间距
  - line-height——行高
  - text-align——对齐
  - text-decoration——装饰线
  - text-indent——首行缩进
- 字体
  - font——在一个声明中设置所有的字体属性
  - font-famliy——字体系列
  - font-size——字号
  - font-style——斜体
  - font-weight——粗体

### 背景

- background——颜色 图片 repeat
- background-color——颜色
- background-image——url（“logo.jpg”）
- background-repeat——设置平铺类型
  - repeat
  - repeat-x
  - repeat-y
  - no-repeat

### 超链接

- a:link——普通的、未被访问的连接
- a:visited——用户已访问的连接
- a:hover——鼠标指针位于连接的上方悬停
- a:active——连接被点击的时刻

### 列表

- list-style——所有用于列表的属性设置于一个声明中
- list-style-image——为列表项标志设置图像
- list-style-position——标志位置
- list-style-type——标志类型

### 表格

- border——表格边框
- width——宽
- height——高
- border-collapse——表格边框

**奇偶选择器**——：nth-child（oddleven）