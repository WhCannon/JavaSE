# 继承，接口
- [继承示例](https://github.com/WhCannon/JavaSE/blob/master/Extends/继承1.MD)
- [接口示例](https://github.com/WhCannon/JavaSE/blob/master/Extends/接口1.MD)
- [练习](#练习)




## 练习
### 1.写出程序结果 [答案1](#答案1)
```java
interface A{}
class B implements A
{
	public String func() 
	{
		return "func";
	}
}

class Demo
{
	public static void main(String[] args)
	{
		A a=new B();
		System.out.println(a.func());
	}
}
```

### 2.写出程序结果 [答案2](#答案2)
```java
interface A{}
class B implements A
{
	public String test()
	{
		return "yes";
	}
}
class Demo
{
	static A get()
	{
		return new B();
	}
	public static void main(String[] args)
	{
		A a=get();
		System.out,println(a.test());
	}
}
```


### 3.选择题 [答案3](#答案3)
```java
class Demo
{
	int show(int a,int b){return 0;}
}
```
下面哪些函数可以存在于Demo的子类中：  
A.public int show(int a,int b){return 0;}  
B.private int show(int a,int b){return 0;}  
C.private int show(int a,long b){return 0;}  
D.public short show(int a,int b){return 0;}  
E.static int show(int a,int b){return 0;}  



### 4.写出程序结果 [答案4](#答案4)
```java
interface A
{
	void show();
}
interface B
{
	void add(int a,int b);
}
class C implements A,B;
{
	//写出程序代码
	
}
class D
{
	public static void main(String[] args)
	{
		C c=new C();
		c.add(4,2);
		c.show(); //通过该函数打印两个数的和
	}
}
```



### 5.写出程序结果 [答案5](#答案5)
```java
class Super
{
	int i=0;
	public Super(String a)
	{
		i=1;
	}
}
class Demo extends Super
{
	public Demo(String a)
	{
		//super();父类中没有空构造函数，编译失败
		i=2;
	}
	public static void main(String[] args)
	{
		Demo d=new Demo("yes");
		System.out.println(d.i);
	}
}
```



### 6.写出程序结果 [答案6](#答案6)
建立一个图形接口，声明一个面积函数。圆形和矩形都实现这个接口，并得出两个图形的面积。  
体现面向对象的特征，对数值进行判断，用异常处理，不合法的数值要出现“这个数值是非法的”提示，不再进行运算。


### [练习7](#答案7)
```java
写出运行结果
class Fu {
	static {
		System.out.println("静态代码块Fu");
	}

	{
		System.out.println("构造代码块Fu");
	}

	public Fu() {
		System.out.println("构造方法Fu");
	}
}

class Zi extends Fu {
	static {
		System.out.println("静态代码块Zi");
	}

	{
		System.out.println("构造代码块Zi");
	}

	public Zi() {
		System.out.println("构造方法Zi");
	}
}

class ExtendsTest2 {
	public static void main(String[] args) {
		Zi z = new Zi();
	}
}

```










### 答案1
编译失败，a所属的A接口没有定义func方法

### 答案2
编译失败，A接口中没有定义test方法

### 答案3
A.public int show(int a,int b){return 0;} //可以，覆盖  
B.private int show(int a,int b){return 0;}//不可以，子类权限必须大于等于父类  
C.private int show(int a,long b){return 0;}//可以，子类的特有方法  
D.public short show(int a,int b){return 0;}//不可以，调用的不确定性  
E.static int show(int a,int b){return 0;}//不可以，静态只能覆盖静态  

### 答案4
```java
private int c;
	public void add(int a,int b)
	{
		c=a+b;
	}
	public void show()
	{
		System.out.println(c);
	}
  ```
  
  ### 答案5
```java
class Demo extends Super
{
	public Demo(String a)
	{
		//super();父类中没有空构造函数，编译失败
		i=2;
	}
	public static void main(String[] args)
	{
		Demo d=new Demo("yes");
		System.out.println(d.i);
	}
}
```


### 答案6
```java
interface Areable
{
	public double getArea();
}

class NoValueException extends RuntimeException //异常
{
	NoValueException()
	{
		super();
	}
	NoValueException(String message)
	{
		super(message);
	}
}

class Rec implements Areable  //矩形面积
{
	private int length;
	private int width;
	Rec(int length,int width)
	{
		if(length<=0||width<=0)
			throw new NoValueException("数值非法");
		this.length=length;
		this.width=width;
	}
	public double getArea()
	{
		return length*width;
	}
}

class Circle implements Areable//圆形面积
{
	private int radius;
	public static final double PI=3.14;
	Circle(int radius)
	{
		if(radius<=0)
			throw new NoValueException("半径非法");
		this.radius=radius;
	}
	public double getArea()
	{
		return radius*radius*PI;
	}
}


class Test21
{
	public static void main(String[] args) 
	{
		Rec r=new Rec(3,5);
		double a=r.getArea();
		System.out.println("area:"+a);

	}
}
```

## 答案7
```java
		静态代码块Fu
		静态代码块Zi
		构造代码块Fu
		构造方法Fu
		构造代码块Zi
		构造方法Zi
```
