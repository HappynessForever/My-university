[TOC]

# GUI概述

- GUI
  - 用图形的方式，来显示计算机操作的界面，这样更方便直观。
- CLI
  - 就是常见的Dos命令行操作，操作不直观。

# awt和swing包的描述

- java.awt：Abstract Window ToolKit（抽象窗口工具包），需要调用**本地系统方法**实现功能。属于重量级控件。
- javax.swing：在AWT的基础上，建立一套图形界面系统，其中提供了更多的组件，而且**完全由Java实现**，增强了移植性，属于轻量级控件。

常用方法：窗体、大小、可见、基础操作

```java
JFrame();
JFrame(String s);
public void setSize(int width, int heigh);
public void setLocation(int x, int y);
public void setBounds(int a, int b, int width, int heigth);
public void setVisible(boolean b);
public void dispose();
public void setDefaultCloseOperation(int operation);
```

**两个封装类**：Dimension（封装宽度和高度）、Point（封装位置）

# 事件监听

事件源：事件发生的地方

事件：就是要发生的事情

事件处理：就是针对发生的事情做出的处理方案

事件监听：事件源和事件关联起来

举例：人受伤事件

​		事件源：人（具体的对象）

​				Person p1= new Person("张三")；

​				Person p2 = new Person("李四")；

​		事件：受伤（接口）

​				interface 受伤接口 {

​						一拳（）；一脚（）；一屁股（）；

​				}

​		事件处理：

​				受伤处理类 implements 受伤接口 {

​						一拳（）{}	一脚（）{}	一屁股（）{}

​				}

​		事件监听

​				p1.注册监听（受伤接口）

## 适配器模式

​		当一个接口中方法很多，具体类实现接口就会显得很复杂。这是，可以使用一个抽象类实现接口，然后在用一个具体类区实现抽象类，然后抽象类只实现那些对自己有用的方法就好了。

**接口**

```java
package com.e01;

public interface UserDao {
	//增加用户
	public abstract void addUser();
	
	//更新用户
	public abstract void updateUser();
	
	//删除用户
	public abstract void deleteUser();
	
	//查找用户
	public abstract void findUser();
}
```

**抽象类**

```java
package com.e01;

public abstract class UserAdapter implements UserDao {

	@Override
	public void addUser() {

	}

	@Override
	public void updateUser() {

	}

	@Override
	public void deleteUser() {

	}

	@Override
	public void findUser() {

	}

}
```

**具体类**

```java
package com.e01;

public class UserDaoImpl extends UserAdapter {
	public void addUser() {
		System.out.println("添加用户");
	}
}
```

## 窗体关闭案例

```java
package com.e01;

import java.awt.Frame;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

public class FrameDemo {
	public static void main(String[] args) {
		//创建窗体对象
		Frame f = new Frame("窗体关闭案例");
		
		f.setBounds(800, 200, 400, 300);
		
		//让窗体关闭。事件源：窗体；事件：对窗体的处理；事件处理：关闭窗体
		//事件监听
		f.addWindowListener(new WindowAdapter() {//适配器类
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		f.setVisible(true);
		
		
		
	}
}
```

## 按钮点击事件

```java
package com.e01;

import java.awt.Button;
import java.awt.FlowLayout;
import java.awt.Frame;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

/*
 * 创建窗体对象
 * 创建按钮对象
 * 把按钮添加到窗体
 * 窗体显示
 */
public class FrameDemo {
	public static void main(String[] args) {
		Frame f = new Frame("添加按钮");
		
		f.setBounds(800, 200, 400, 300);
		
		f.setLayout(new FlowLayout());
		
		Button bu = new Button("按钮");
		
		
		f.add(bu);
		
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		bu.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println("你在点试试");
				
			}
		});
		
		f.setVisible(true);
	}
}

```

## 文本框转移案例

```java
package com.e01;

import java.awt.Button;
import java.awt.FlowLayout;
import java.awt.Frame;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

/*
 * 创建窗体对象
 * 创建按钮对象
 * 把按钮添加到窗体
 * 窗体显示
 */
public class FrameDemo {
	public static void main(String[] args) {
		//创建一个窗体
		Frame f = new Frame("数据转移");
		//设置一个窗体大小和初始位置
		f.setBounds(800, 200, 400, 300);
		//设置布局
		f.setLayout(new FlowLayout());
		//设置文本框
		TextField tf = new TextField(20);
		//设置按钮
		Button bu = new Button("数据转移");
		//设置文本域
		TextArea ta = new TextArea(10, 40);
		//加入组件，有顺序
		f.add(tf);
		f.add(bu);
		f.add(ta);
		
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		bu.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				//获取文本框的值
				String tf_str = tf.getText().trim();	//去空格
				
				//设置给文本域
				//ta.setText(tf_str);
				ta.append(tf_str + "\n");//追加输入
				
				//获取光标
				tf.requestFocus();
			}
		});
		
		f.setVisible(true);
	}
}

```

## 移动到按钮上更换背景颜色案例

```java
package com.e01;

import java.awt.Button;
import java.awt.Color;
import java.awt.FlowLayout;
import java.awt.Frame;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

/*
 * 创建窗体对象
 * 创建按钮对象
 * 把按钮添加到窗体
 * 窗体显示
 */
public class FrameDemo {
	public static void main(String[] args) {
		//创建一个窗体
		Frame f = new Frame("更改背景颜色");
		//设置一个窗体大小和初始位置
		f.setBounds(800, 200, 400, 300);
		//设置布局
		f.setLayout(new FlowLayout());
		
		Button redButton = new Button("红色");
		Button greenButton = new Button("绿色");
		Button blueButton = new Button("蓝色");
		//添加按钮
		f.add(redButton);
		f.add(greenButton);
		f.add(blueButton);
		
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
//		//对按钮添加动作事件
//		redButton.addActionListener(new ActionListener() {
//			
//			@Override
//			public void actionPerformed(ActionEvent e) {
//				f.setBackground(Color.RED);				
//			}
//		});
		
		//对按钮添加鼠标事件
		redButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {//进入
				f.setBackground(Color.RED);
			}
			
			@Override
			public void mouseExited(MouseEvent e) {//离开
				f.setBackground(Color.WHITE);
			}
		});
		greenButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {//进入
				f.setBackground(Color.GREEN);
			}
			
			@Override
			public void mouseExited(MouseEvent e) {//离开
				f.setBackground(Color.WHITE);
			}
		});
		blueButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseEntered(MouseEvent e) {//进入
				f.setBackground(Color.BLUE);
			}
			
			@Override
			public void mouseExited(MouseEvent e) {//离开
				f.setBackground(Color.WHITE);
			}
		});
		
		
		f.setVisible(true);
	}
}

```

## 控制文本框只能输入数字字符案例

```java
package com.e01;

import java.awt.FlowLayout;
import java.awt.Frame;
import java.awt.Label;
import java.awt.TextField;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

/*
 * 创建窗体对象
 * 创建按钮对象
 * 把按钮添加到窗体
 * 窗体显示
 */
public class FrameDemo {
	public static void main(String[] args) {
		//创建一个窗体
		Frame f = new Frame("更改背景颜色");
		//设置一个窗体大小和初始位置
		f.setBounds(800, 200, 400, 300);
		//设置布局
		f.setLayout(new FlowLayout());
		
		//创建Label对象
		Label lable = new Label("QQ号码");
		TextField tf = new TextField(40);
		
		f.add(lable);
		f.add(tf);
		
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		//给文本框添加事件
		tf.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent e) {
				//如果不是数字字符就取消事件
				//思路：获取字符，判断字符，取消事件
				char ch = e.getKeyChar();
				if(!('0' <= ch && ch <= '9')) {
					e.consume();
				}
			}
		});
		
		f.setVisible(true);
	}
}

```

## 一级菜单案例

```java
package 菜单;

import java.awt.FlowLayout;
import java.awt.Frame;
import java.awt.Menu;
import java.awt.MenuBar;
import java.awt.MenuItem;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class FrameDemo {
	public static void main(String[] args) {
		//创建一个窗体
		Frame f = new Frame("更改背景颜色");
		//设置一个窗体大小和初始位置
		f.setBounds(800, 200, 400, 300);
		//设置布局
		f.setLayout(new FlowLayout());
			
		//创建菜单栏
		MenuBar mb = new MenuBar();
		
		//创建菜单
		Menu m = new Menu("文件");
		
		//创建菜单项
		MenuItem mi = new MenuItem("退出系统");
		
		//设置菜单栏
		f.setMenuBar(mb);
		m.add(mi);
		mb.add(m);
		
		mi.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.exit(0);
			}
		});
		
		
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		f.setVisible(true);
	}
}

```

## 多级菜单案例

```java
package 菜单;

import java.awt.FlowLayout;
import java.awt.Frame;
import java.awt.Menu;
import java.awt.MenuBar;
import java.awt.MenuItem;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.IOException;

public class FrameDemo {
	public static void main(String[] args) {
		//创建一个窗体
		Frame f = new Frame("更改背景颜色");
		//设置一个窗体大小和初始位置
		f.setBounds(800, 200, 400, 300);
		//设置布局
		f.setLayout(new FlowLayout());
			
		//创建菜单栏
		MenuBar mb = new MenuBar();
		
		//创建菜单
		Menu m = new Menu("文件");
		Menu m2 = new Menu("文件2");
		
		//创建菜单项
		MenuItem mi = new MenuItem("退出系统");
		MenuItem mi2 = new MenuItem("打开记事本");
		
		//设置菜单栏
		f.setMenuBar(mb);
		
		m.add(m2);
		m.add(mi);
		m2.add(mi2);
		mb.add(m);
		
		mi.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.exit(0);
			}
		});
		
		mi2.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				Runtime r = Runtime.getRuntime();
				try {
					r.exec("notepad");
				} catch (IOException e1) {
					e1.printStackTrace();
				}
				
			}
		});
		
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
		f.setVisible(true);
	}
}

```



# 布局方式

窗体布局，窗体中组件的排列方式

流式布局（FlowLayout）、边界布局（BorderLayout）、网格布局（GridLayout）、网格包布局、卡片布局（CardLayout）

# 总结

1. GUI（了解）
   1. 用户图形界面
      1. GUI方便直观
      2. CLT：需要记忆一些命令
   2. 两个包
      1. java.awt
      2. javax.swing
   3. GUI继承关系
      1. 组件：对象
         1. 容器组件：是可以存储基本组件和容器组件的组件
         2. 基本组件：是可以使用的组件，但是必须依赖容器
   4. 事件监听机制（理解）
      1. 事件源
      2. 事件
      3. 时间处理
      4. 事件监听
   5. 适配器模式
      1. 接口
      2. 抽象适配器类
      3. 实现类
   6. 案例：
      1. 创建窗体案例
      2. 窗体关闭案例
      3. 窗体添加按钮并对按钮添加事件案例
         1. 界面中组件布局
      4. 把文本框里面的数据转移到文本域
      5. 更改背景颜色
      6. 设置文本框里面不能输入非数字字符
      7. 一级菜单
      8. 多级菜单
   7. Netbeans的概述和使用
      1. 是可以开发做Java的另一个IDE工具
      2. 使用
         1. 四则运算
            1. 修改图标
            2. 设置皮肤
            3. 设置居中
            4. 数据校验
         2. 登录注册