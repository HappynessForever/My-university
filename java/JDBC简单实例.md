# JDBC简单实例

前提条件：存在一张用户（user）表，利用JDBC实现对user的增删改查。

建立一个com.model包写入以下内容：

```java
package com.model;

public class User {
	private String name;		//用户名
    private String password;	//用户密码
    
    public User(){}
    public User(String name, String password){
        this.name = name;
        this.password = password;
    }
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	
	@Override
	public String toString() {
		return name + " " + password;
	}
}

```

建立com.dao包写入以下内容：

```java
package com.dao;

import java.util.Map;
import com.model.*;

public interface userDao {
	//增加user
    public User addUser(String name, String password);
    
    //删除user
    public int deleteUser(String name);
    
    //修改user
    public int alterUser(String name, String rename, String repassword);
    
    //查询user
    public Map<String, String> listUser();
    
    //判断用户是否存在
    public boolean isExistUser(String name);
}

```

建立com.daoImpl包写入以下内容：

```java
package com.daoImpl;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.Map;

import com.dao.userDao;
import com.model.User;
import com.util.JdbcUtil;

public class userDaoImpl implements userDao{
	JdbcUtil ju = new JdbcUtil();
	private User user;	
	
	@Override
	public User addUser(String name, String password) {
		if(!isExistUser(name)) {
			user = new User();
			user.setName(name);
			user.setPassword(password);	
			ju.getConnection();
			String sql = "insert into Users values(?,?)";
			ju.executeUpdate(sql, new Object[] {name, password});
			ju.free();
		}
		return user;
	}

	@Override
	public int deleteUser(String name) {
		ju.getConnection();
		String sql = "delete Users where name = ?";
		int flag = ju.executeUpdate(sql, new Object[] {name});
		ju.free();
		return flag;
	}

	@Override
	public int alterUser(String name, String rename, String repassword) {
		ju.getConnection();
		String sql = "update Users set name = ?, password = ? where name = ?";
		int flag = ju.executeUpdate(sql, new Object[] {rename, repassword, name});
		ju.free();
		return flag;
	}

	@Override
	public Map<String, String> listUser() {
		Map<String, String> map = new HashMap<String, String>();
		ju.getConnection();
		String sql = "select * from Users";
		ResultSet rs = ju.executeQuery(sql, null);
		try {
			user = new User();
			while(rs.next()) {
				user.setName(rs.getString("name"));
				user.setPassword(rs.getString("password"));
				map.put(user.getName(), user.getPassword());
			}
		} catch (SQLException e) {}
		ju.free();
		return map;
	}

	@Override
	public boolean isExistUser(String name) {
		ju.getConnection();
		String sql = "select name from Users where name=?";
		ResultSet rs = ju.executeQuery(sql, new Object[] {name});
		try {
			if(rs.getRow() == 0) {
				return false;
			}
		} catch (SQLException e) {}
		return true;
	}	
}

```

建立com.util包写入以下内容：

```java
package com.util;

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
			InputStream inStream = JdbcUtil.class.getResourceAsStream("jdbc.properties");
			Properties prop = new Properties();
			try {
				if(inStream == null) {
					System.out.println("ssnull");
				}
				prop.load(inStream);
				USER = prop.getProperty("jdbc.user");
				PASSWORD = prop.getProperty("jdbc.password");
				URL = prop.getProperty("jdbc.url");
				DRIVER = prop.getProperty("jdbc.driver");
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
				Class.forName(DRIVER);
				conn = DriverManager.getConnection(URL,USER,PASSWORD);
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
					// TODO: handle exception
					e2.printStackTrace();
				} finally {
					try {
						if(conn != null) {
							conn.close();
						}
					} catch (SQLException e3) {
						// TODO: handle exception
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

```java
package com.util;

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

```
jdbc.user=
jdbc.password=
jdbc.url=
jdbc.driver=

```

建立com.test测试包，可以进行代码测试：

```java
package com.test;

import java.util.Map;

import com.dao.userDao;
import com.daoImpl.userDaoImpl;
import com.model.User;

public class userTest {
	public static void main(String[] args) {
		userDao ud = new userDaoImpl();
		Map<String, String> map = ud.listUser();

			for(String key : map.keySet()) {
				System.out.println(key + " " + map.get(key));
			
		}	
	}
}

```

