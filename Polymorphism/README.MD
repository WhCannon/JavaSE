# 多态
- [练习](#练习)


## 练习
### 1.写出程序运行结果[答案1](#答案1)
```java
class Fu
{
	boolean show(char a)
	{
		System.out.println(a);
		return true;
	}
}
class Demo extends Fu
{
	public static void main(String[] args)
	{
		int i=0;
		Fu f=new Demo();
		Demo d=new Demo();
		for(f.show('A');f.show('B')&&(i<2);f.show('C'))
		{
				i++;
				d.show('D');
		}
	}
	boolean shou(char a)
	{
		System.out.println(a);
		return false;
	}
}
```

### 2.写出程序运行结果[答案2](#答案2)
```java
class Super
{
	int i=0;
	public Super(String a)
	{
		System.out.println("A");
		i=1;
	}
	public Super()
	{
		System.out,println("B");
		i+=2;
	}
}
class Demo extends Super
{
	public Demo(String a)
	{
		//super();
		System.out.println("C");
		i+=5;
	}
	public static void main(String[] args)
	{
		int i=4;
		Super d=new Demo("A");
		System.out.println(d.i);
	}
}
```


### 3.写出程序运行结果[答案3](#答案3)
```java
class Fu
{
	int num=4;
	void show()
	{
		System.out.println("showfu");
	}
}
class Zi extends Fu
{
	int num=5;
	void show()
	{
		System.out.println("showzi");
	}
}
class T
{
	public static void main(String[] args)
	{
		Fu f=new Zi();
		Zi z=new Zi();
		System.out,println(f.num);
		System.out.println(z.num);
		f.show();
		z.show();
	}
}
```

### 练习4
```java
写出运行结果
class A {
	public void show() {
		show2();
	}
	public void show2() {
		System.out.println("我");
	}
}
class B extends A {
	/*从父类继承的
	public void show() {
		show2();
	}
	*/

	public void show2() {
		System.out.println("爱");
	}
}
class C extends B {
	public void show() {
		super.show();
	}
	public void show2() {
		System.out.println("你");
	}
}
public class DuoTaiTest4 {
	public static void main(String[] args) {
		A a = new B();
		a.show();
		
		B b = new C();
		b.show();
	}
}
```








## 答案1
A B

## 答案2
B C 7

## 答案3
4 5 showzi showzi

## 答案4
爱你

