# 抽象类



# 练习1
 - [分析](#分析)
 - [抽象函数-员工基本信息](#员工基本信息)
 - [程序员类](#程序员类)
 - [经理类](#经理类)
 - [主函数](#主函数)
 
 ### 分析
 ```java
/*
1.两个对象，程序员和项目经理
2.共有内容:姓名、工号、薪水、工作内容，项目经理还有奖金
3.父类应具有姓名、工号、薪水、工作内容，其中工作内容不同，所以应为抽象的
*/
```

 ### 员工基本信息
 ```java
abstract class Person
{
	private String name;
	private String id;
	private double salar;
	Person(String name,String id,double salar)
	{
		this.name=name;
		this.id=id;
		this.salar=salar;
	}

	public String getName()
	{
		return name;
	}

	public String getId()
	{
		return id;
	}

	public double getSalar()
	{
		return salar;
	}

	public abstract void work();
}
```

### 程序员类
```java
class Program extends Person
{
	Program(String name,String id,double salar)
	{
		super(name,id,salar);
	}

	public void work()
	{
		System.out.println("programer:");
	}
}
```
### 经理类
```java

class Manager extends Person
{
	Manager(String name,String id,double salar)
	{
		super(name,id,salar);
	}

	public void work()
	{
		System.out.println("manage");
	}
}
```
### 主函数
```java
public class Information
{
	public static void main(String[] args) 
	{
		Program p1=new Program("WH","1201710142",300000);
		p1.work();
		System.out.println(p1.getName()+","+p1.getId()+","+p1.getSalar());
		Manager p2=new Manager("ryh","20131002638",500000);
		p2.work();
		System.out.println(p2.getName()+","+p2.getId()+","+p2.getSalar());
	}
}
```

