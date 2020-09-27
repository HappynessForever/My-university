### 导入版本信息

关键技术：在jsp中可以应用jsp:include标签包含其他文件的内容，被包含的文件可以是jsp或html文件，jsp：include标签语法如下：

```java
<jsp:include page="被包含组件的绝对路径或相对路径"
```

设计过程：

1）创建一个包含版权信息的页面foot.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"%>
<html>
<head>
	<title>版本说明</title>
</head>
<body>
	<div align="center">
		<table>
			<tr align="center">
				<td>编程词典销售服务热线：400-675-1000 网址：www.mingrisoft.com</td>
			</tr>
			<tr align="center">
				<td>Copyright@www.mingrisoft.com All Rights Reserved!</td>
			</tr>
		</table>
	</div>
</body>	
</html>
```

2）创建网站的主页面index.jsp，在改页中应用jsp:include标签将版权信息foot.jsp包含进来

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"%>
<html>
<head>
	<title>版本说明</title>
</head>
	<body>
		<table>
			<tr>
				<td colspan="3">
					<jsp:include page="/jsp/foot.jsp"></jsp:include>
				</td>
			</tr>
		</table>
	</body>	
</html>
```

总结：

在开发网站时，如果多数网页都包含**相同的内容**，可以把这部分内容**单独放倒一个文件**中，其他的jsp文件通过include标签即可将这个文件包含进来。这样做可以**提高代码的重用性**，**避免重复代码**，从而**提高开发网站的效率**。

### 应用Java程序片段动态生成表格

关键技术：

在jsp文件中，可以在 “<%” 和 “%>” 标记之间直接嵌入任何有效的Java代码。

设计过程：

1）创建index.jsp页面，首先在“<%” 和 “%>” 之间中定义一个字符串数组。

```jsp
<%String[] bookName = {"Java","Web","CSS","JavaScrip","HTML5"}; %>
```

2）在index.jsp页面的表格处，循环步骤（1）中创建出的数组，动态生成表格并将数据添加到表格中

```jsp
<table border="1" align="center">
    <tr>
        <td align="center">编号</td>
        <td align="center">书名</td>
    </tr>
    <%for(int i=0; i<bookName.length; i++) { %>
    <tr>
        <td align="center"><%=i %></td>
        <td align="center"><%=bookName[i] %></td>				
    </tr>
    <%} %>
</table>
```

### 应用Java程序片段动态生成下拉列表

数据库中读取数据，然后再添加到下拉列表中。

设计过程：
1）新建index.jsp页，首先在该页中创建一个字符数组，用于保存员工的部门信息

```jsp
<% String[] dept = {"策划部","销售部","研发部","人事部","测试部"}; %>
```

2）在下拉列表中，循环这个数组。

```jsp
<table>
    <tr>
        <td align="center" colspan="4">员工信息查询</td>
    </tr>
    <tr>
        <td>员工姓名：<input type="text" name="name" /></td>
        <td>年龄：<input type="text" name="age" /></td>
        <td>所在部门：
            <select>
                <%for(int i=0; i<dept.length; i++) {%>
                <option value="<%=dept[i] %>"> <%=dept[i] %> </option>
                <%}%>
            </select>

        </td>
        <td><input type="button" value="查询" /></td>
    </tr>
</table>
```

### 获取表单提交信息

关键技术：

jsp的内置对象request封装了由客户端生成的HTTP请求的所有细节，主要包括HTTP头信息、系统信息、请求方式和请求参数等。通过request对象提供的相应方法可以处理客户端浏览器提交的HTTP请求的各项参数。

可以通过request对象的getParameter方法可以获取用户提交的表单信息。

```java
	String name = request.getParameter("name");
```

注意：如果发生乱码问题，需要在应用request获取请求参数之前，调用request对象的setCharacterEncoding方法设置请求编码。

```java
	request.setCharacterEncoding("UTF-8");
```

### 通过Application对象实现网站计数器

关键技术：主要使用setAttribute方法和setAttribute方法

```jsp
<%
	int i = 0;
	synchronized(application) {
		if(application.getAttribute("times") == null) {				i = 1;
		} else {
			i = Integer.parseInt((String)application.getAttribute("times"));
			i++;
		}
		application.setAttribute("times", Integer.toString(i));
	}
%>
		
		<table>
			<tr bgcolor="lightgrey">
				<td align="center">欢迎访问！</td>
			</tr>
			<tr>
				<td align="center"><%=i %>位访问本网站</td>
			</tr>
		</table>
```

