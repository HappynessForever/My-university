# DOM

- 什么是DOM（document object model）
- DOM是W3C（万维网联盟）的标准，是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。
- 对网页进行增删改查操作。

### 常用DOM操作

- 查找节点
- 读取节点信息
- 修改节点信息
- 创建新节点
- 删除节点

### DOM查找

- getElementByld()——按id属性，精确查找一个元素对象
- getElementByTagName()——按标签名找或者按name属性查找
  - 可以用在任意父元素上
  - 查找相关所有子代节点
  - 返回一个动态集合
    - 即使只找到一个元素，想拿到元素也必须使用【0】来取出元素
- getElementByClassName()——通过class属性查找
- appendChild()
- removeChild()
- insertBefore()
- createAttribute()
- createElement()
- createTextNode()
- getAttribute()
- setAttribute()

### DOM修改

- 读取属性值
  - 先获取属性节点对象，再获得节点对象的值
  - 直接获得属性值
- 修改属性值
- 判断是否包含指定属性
- 移除属性

- 修改样式
  - 属性名：去横线，变驼峰。

### DOM添加

- 添加元素的步骤
  - 创建空元素
  - 设置关键属性
  - 将元素添加到DOM