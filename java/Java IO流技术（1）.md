[TOC]

# 文档

## File类的概述和构造方法

- File类的概述
  - 文件和目录路径的抽象表示形式

- 构造方法

```java
//根据一个路径得到File对象
public File(String pathname);
//父路径名字字符串和子路径字符串创建新的File实例
public File(String parent, String child);
//从父抽象路径名和子路径名字字符串创建新的File实例
public File(File parent, String child);
```

使用构造方法：

```java
//把D:\\demo\\a.txt封装成一个File对象
//File(String pathname);
File file = new File("D:\\demo\\a.txt");

//public File(String parent, String child);
File file2 = new File("D:\\demo", "a.txt");

//public File(File parent, String child);
File file3 = new File("D:\\demo");
File file4 = new File(file3, "a.txt");

//以上三种方式效果一样
```

## File类的成员方法

- **创建功能**
  - public boolean createNewFile() 创建文件
  - public boolean mkdir() 创建文件夹（目录）
  - public boolean mkdirs() 递归创建目录（一次性创建多个）

- **删除功能**
  - public boolean delete() 既可以删除文件，也可以删除文件夹（目录）

- **重命名功能**
  - public boolean renameTo(File dest) 路径名相同：就是改名，否则，就是改名并剪切

注意：

A：要想在某个目录下创建文件，这个目录必须存在。

B：Java中的删除不走回收站。

C：删除目录下有内容时，不能用delete方法直接删除目录。

- **判断功能**
  - public boolean isDirectory() 判断文档是否是一个目录
  - public boolean isFile() 判断文档是否是一个文件，而非目录
  - public boolean exists() 判断文档是否存在
  - public boolean canRead() 判断文件是否可读
  - public boolean canWrite() 判断文件是否可写
  - public boolean isHidden() 判断文件是否是隐藏文件

- **获取功能**
  - 基本获取功能
    - public String getAbsolutePath() 获取文件的绝对路径
    - public String getPath() 获取相对路径
    - public String getName() 获取文件的名字
    - public long length() 获取文件的长度（单位是字节）
    - public long lastModified() 获取文件最后修改时间（时间是从1970年午夜至文件最后修改时刻的毫秒数），配合Date，SimpleDateFormat来是毫秒数有意义。
  - 高级获取功能
    - public String[] list() 获取指定目录下的所有文件或者文件夹的名称数组
    - public File[] listFile() 获取指定目录下的所有文件或者文件夹的File数组

案例：判断C盘目录下是否有后缀名为.jpg的文件，如果有，就输出此文件名称

```java
public class FileDemo {
	public static void main(String[] args) {
		File file = new File("d:\\");
		//方法一
		File[] fileArray = file.listFiles();
		
		for(File f : fileArray) {
			if(f.isFile()) {
				if(f.getName().endsWith(".exe")) {
					System.out.println(f.getName());
				}
			}
		}
		//方法二
		String[] strArray = file.list(new FilenameFilter() {

			@Override
			public boolean accept(File dir, String name) {   
                File file = new File(dir, name);
				boolean flag1 = file.isFile();
				boolean flag2 = name.endsWith(".exe");
				return flag1 && flag2;
                
                //或者直接写	return File(dir, name).isFiel() && name.endWith(".exe");
			}
			
		});
		
		for(String s : strArray) {
			System.out.println(s);
		}
	}
}
```

**文件过滤器**

高级获取功能里面的方法参数FilenameFilter是一个接口，这个接口可以看做文件过滤器，可以来实现这个接口从源头选择我们需要的文档。说明：该接口有一个accept(File dir, String name)方法，参数dir为调用list的当前目录dirFile，参数name被实例化为dirFile目录中的一个文件名，当接口方法返回true时，list方法就将名字为name的文件存放到返回的数组中。