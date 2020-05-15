# HTML标签

### 第一部分

标题：h1~h6

段内换行：br

段内分组：span

段落：p

预留格式：pre

水平线：hr

习题：

①	利用h1、p、hr等标签完成“web前端开发“页面

### 第二部分

- 超链接：a
  - 链到本站点其他网页
  - 链接到其他站点
  - 虚拟超链接

习题：

①	跳转页面

### 第三部分

- 图像格式
  - GIF：简单动画、背景透明
  - PNG：无损压缩、透明、交错、动画
  - JPG：

插入图像：img

绝对路径、相对路径

习题：

①	点击图片，跳转页面

### 第四部分

区域：div

列表：ul、ol、li

表格：table、tr、td

习题：

①	使用div、table创建一个排行榜

### 第五部分

表单：是一个区域，采集用户信息

表单元素：文本框、按钮、单选、复选、下拉列表、文本域

表单标签：form

文本框：text

密码框：password

提交按钮：submit	value（按钮上显示文字）

重置按钮：reset	value

单选框：radio

复选框：checkbox

下拉列表：select option

文本域：textarea

- 表单中
  - id是唯一的，任何元素都可以有id。它可以唯一直接定位到这个元素，主要在js的document.getElementByid（）使用
  - name可以不唯一，不是所有元素都有name，表单元素有name。它主要用来提交表单数据给后端。
  - value就是要传给后端的那个值