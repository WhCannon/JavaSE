# 面向对象
  - [内存加载过程](#内存加载过程)
  - [构造代码块](#构造代码块)
  - [练习](#练习)
  

## 内存加载过程
```java
class Student {
	private String name="林青霞";
	private int age=27;
	public Student() {
		name="刘意";
		age=30;
	}
}
class StudentDemo{
	public static void main(String[] args) {
		Student s=new Student();
	}
}
①加载StudentDemo.class；
②加载main方法；
③加载Student.class；
④在栈内存内给s变量开辟一个空间；
⑤在堆内存为学生对象申请一个空间；
⑥给成员变量进行默认初始化；
⑦给成员变量进行显式初始化；
⑧通过构造方法给成员变量进行初始化；
⑨数据初始化完毕，然后把堆内存的地址值赋值给栈内存的s变量。
```


## 构造代码块
```java
class Code {
	{
		int x = 100;
		System.out.println(x);
	}
	public Code(){
		System.out.println("code");
	}
	{
		int y = 200;
		System.out.println(y);
	}
}
class Test0 {
	public static void main(String[] args) {
		Code c = new Code();	
	}
}
输出结果：100 200 code
```

## 练习
### [练习1](#答案1)
```java
写出运行结果
class Student {
	{
		System.out.println("Student 构造代码块");
	}
	
	public Student() {
		System.out.println("Student 构造方法");
	}
	
	static {
		System.out.println("Student 静态代码块");
	}
}

class StudentDemo {
	static {
		System.out.println("林青霞都60了，我很伤心");
	}
	
	public static void main(String[] args) {
		System.out.println("我是main方法");
		
		Student s1 = new Student();
		Student s2 = new Student();
	}
}
```


## 答案1
```java
	林青霞都60了，我很伤心
	我是main方法
	Student 静态代码块
	Student 构造代码块
	Student 构造方法
	Student 构造代码块
	Student 构造方法
```
