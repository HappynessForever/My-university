[TOC]

# JDBC数据库

JDBC操作不同的数据库仅仅是连接方式上有差异而已，使用JDBC的应用程序一旦和数据库建立连接，就可以使用JDBC提供的API操作数据库。以下是使用JDBC操作数据库：

![YEHhQJ.png](https://s1.ax1x.com/2020/05/06/YEHhQJ.png)

程序经常使用JDBC进行如下的操作：

- 与一个数据库建立连接
- 向已连接的数据库发送SQL语句
- 处理SQL语句返回的结果

## 连接数据库的工具类

以下是工具类的笔记，涉及的知识点如下：

- 数据库的连接
  - 加载驱动
  - 利用property来加载连接数据库的相关信息

- 运行时异常类的编写
- 数据库的查询操作	

### 数据库的连接

驱动需要从网上下载。加载驱动涉及到的方法：

```java
Class.forName(DRIVER);	//DRIVER是数据库相关驱动
```

使用到

```java
Class.getResourceAsStream(String path);
```

方法，该方法简单介绍：path 不以’/'开头时默认是从此类所在的包下取资源，以’/'开头则是从ClassPath根下获取。其只是通过path构造一个绝对路径。

使用到**Properties**类来加载数据使用到的方法：

```java
load(InputStream inStream);	//加载数据流
getProperty(String ley);	//获取键值所对应的值
```

运行时异常类的编写，目的是让人更清楚程序有问题时是在哪里，方便查找问题。过程中使用了枚举。

直接上工具类：

配置文件名称：jdbcUtil.properties	配置文件内容：

```
user=Value
password=Value
url=Value
driver=Value
```



```java
import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

import com.util.SMSJdbcException.ExceptionCategory;

public class JdbcUtil {
		//定义访问数据库的地址
		private  static  String URL;
		
		//定义数据库用户名
		private  static  String USER;
		
		//定义数据库密码
		private  static  String PASSWORD;
		
		//定义数据库驱动信息
		private  static  String DRIVER;
		
		//定义数据库连接
		private Connection conn = null;
		
		//定义sql语句执行对象
		private PreparedStatement ps = null;
		
		//定义查询返回结果集合
		ResultSet rs = null;	
		
		static {
			loadConfig();
		}
		
		/**
		 * 加载配置信息，并给相关属性赋值
		 */
		public static void loadConfig() {
			InputStream inStream = 		 JdbcUtil.class.getResourceAsStream("/jdbcutil.properties");   //从ClassPath根下获取         
			Properties prop = new Properties();
			try {
				prop.load(inStream);
				USER = prop.getProperty("user");
				PASSWORD = prop.getProperty("password");
				URL = prop.getProperty("url");
				DRIVER = prop.getProperty("driver");
			} catch (IOException e) {
				throw new SMSJdbcException(ExceptionCategory.Properties);
			}	
		}
		
		public JdbcUtil() {}
		/**
		 *  获取数据库连接
		 * @return 数据库连接
		 */
		public  Connection getConnection() {
			try {
				Class.forName(DRIVER);	//加载数据库驱动
				conn = DriverManager.getConnection(URL,USER,PASSWORD);	//建立数据库连接
			} catch (Exception e) {
				throw new SMSJdbcException(ExceptionCategory.ConnectFail);
			}
			return conn;
		}
		/**
		 *  释放资源
		 */
		public  void free() {
			try {
				if(rs != null) {
					rs.close();
				}
			} catch (SQLException e) {
				e.printStackTrace();
			} finally {
				try {
					if(ps != null) {
						ps.close();
					}
				} catch (SQLException e2) {
					e2.printStackTrace();
				} finally {
					try {
						if(conn != null) {
							conn.close();
						}
					} catch (SQLException e3) {
						e3.printStackTrace();
					}
				}
			}
		}
		
		/**
		 *   执行更新操作
		 * @param sql sql语句
		 * @param params 执行参数
		 * @return SQLException
		 */
		public ResultSet executeQuery(String sql, Object[] params) {
			ResultSet rs = null;
			try {
				ps = conn.prepareStatement(sql);
				if(params != null && params.length > 0) {
					for(int i=0; i<params.length; i++) {
						ps.setObject(i+1, params[i]);
					}
				}
				rs = ps.executeQuery();
			} catch (SQLException e) {
				throw new SMSJdbcException(ExceptionCategory.SQLFail);
			}
			return rs;
		}
		/**
		 * 执行更新操作
		 * @param sql sql语句
		 * @param params 执行参数
		 * @return 更新结果条数
		 */
		public int executeUpdate(String sql, Object[] params) {
			int count = 0;
			try {		
				ps = conn.prepareStatement(sql);
				if(params != null && params.length > 0) {
					for(int i=0; i<params.length; i++) {
						ps.setObject(i+1, params[i]);
					}
				}
				count = ps.executeUpdate();
			} catch (SQLException e) {
				throw new SMSJdbcException(ExceptionCategory.SQLFail);
			}
			return count;
		}
}

```

异常类的编写：

```java
public class SMSJdbcException extends RuntimeException{
	private static final long serialVersionUID = 1L;
	public SMSJdbcException(ExceptionCategory exceptionCategory) {
		super(exceptionCategory.getMessage());
	}
	
	public enum ExceptionCategory {
		ClassNotFound("未找到驱动类"),ConnectFail("连接失败"),
		SQLFail("SQL命令失败"),ResultSetFail("ResultSet处理失败"),Properties("配置文件异常");
		private String message;
		public String getMessage() {
			return message;
		}
		ExceptionCategory(String message) {
			this.message = message;
		}
	}
}

```



