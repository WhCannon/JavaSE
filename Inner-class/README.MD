# 内部类
- [定义内部类](https://github.com/WhCannon/JavaSE/tree/master/Inner-class/定义内部类.MD)
- [内部类私有](#内部类私有)
- [匿名内部类](#匿名内部类)
- [练习](#练习)




## 内部类私有
```java
我有一个人(人有身体，身体内有心脏。)
		class Body {
			private class Heart {
				public void operator() {
					System.out.println("心脏搭桥");
				}
			}
			public void method() {
				if(如果你是外科医生) {
					Heart h = new Heart();
					h.operator();
				}
			}
		}
```


## 匿名内部类
```java
interface Inter {
	public abstract void show();
	public abstract void show2();
}

class Outer {
	public void method() {
		//一个方法的时候
		new Inter() {
			public void show() {
				System.out.println("show");
			}
		}.show();
		
		//如果我是很多个方法，就很麻烦了
		//那么，我们有没有改进的方案呢?
		Inter i = new Inter() { //多态
			public void show() {
				System.out.println("show");
			}
			
			public void show2() {
				System.out.println("show2");
			}
		};
		i.show();
		i.show2();
	}
}

class InnerClassDemo6 {
	public static void main(String[] args) {
		Outer o = new Outer();
		o.method();
	}
}
```


## 练习
### 1.补足代码 [答案1](#答案1)
```java
interface Inter
{
	void show(int a,int b);
	void func();
}
class Demo 
{
	public static void main(String[] args)
	{
		//补足代码，调用两个函数，用匿名内部类
		in.show(3,4);
		in.func();
	}
}
```

### 2.写出结果 [答案2](#答案2)
```java
class TD
{
	int y=6;
	class Inner
	{
		static int y=3;
		void show()
		{
			System.out.println(y);
		}
	}
}
class TC
{
	public static void main(String[] args)
	{
		TD.Inner ti=new TD().new Inner();
		ti.show();
	}
}
```


### 3.在空白处填写代码 [答案3](#答案3)
```java
class Demo
{
	public void func()
	{
		//位置1
	}
	class Inner
	{
	}
	public static void main(String[] args)
	{
		Demo d=new Demo();
		//位置2
	}
}
```
下列哪些写法是可行的：  
A.位置1写new Inner();  
B.位置2写new Inner();  
C.位置2写new d.Inner();  
D.位置2写new Demo.Inner();   


### 4.补足代码 [答案4](#答案4)
```java
interface Test
{
	void func();
}
class Demo
{
	public static void main(String[] args)
	{
		//补足代码；(匿名内部类)调用show方法
		
	}
	void show(Test t)
	{
		t.func();
	}
}
```

### [练习5]
```java
要求输出30 20 10
class Outer {
	public int num = 10;
	class Inner {
		public int num = 20;
		public void show() {
			int num = 30;
			System.out.println(num);
			System.out.println(this.num);
			//System.out.println(new Outer().num);
			System.out.println(Outer.this.num);//限定this为Outer类
		}
	}
}
```



## 答案1
```java
Inter in=new Inter()
		{
			public void show(int a,int b)
			{}
			public void func()
			{}
		}
```

## 答案2
编译失败，因为非静态的内部类中不能定义静态成员


## 答案3
A.位置1写new Inner();//可以  
B.位置2写new Inner();//不可以，主函数是静态的,所以内部类也必须是静态  
C.位置2写new d.Inner();//不可以，相当于new new Demo().Inner();格式错误  
D.位置2写new Demo.Inner();//不可以，Inner()必须是静态才能这样写  


## 答案4
```java
new Demo().show(new Test()
			{
				public void func(){}
			}
		);
```
