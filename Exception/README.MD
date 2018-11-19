# 异常
### [练习1](https://github.com/WhCannon/JavaSE/tree/master/Exception/练习1.MD)
### [自定义异常](https://github.com/WhCannon/JavaSE/tree/master/Exception/自定义异常.MD)
  - [练习](#练习)




## 捕捉
```java
try{  }                    //需要检测异常的代码，发生异常就进入catch
catch(异常类 变量名)      //该变量用于接受发生异常的对象
{ }                     //处理异常的代码
finally{  }             //一定会被执行的代码（finally语句不需要时可以省略）
```


## 练习
### 1.写出程序结果 [答案1](#答案1)
```java
class Demo
{
	public static void main(String[] args)
	{
		try
		{
			showExce();
			System.out.println("A");
		}
		catch (Exception e)
		{
			System.out.println("B");
		}
		finally
		{
			System.out.println("C");
		}
	}

	public static void showExce() extends Exception
	{
		throw new Exception();
	}
}
```


### 2.写出程序结果 [答案2](#答案2)
```java
class Demo
{
	public static void func()
	{
		try
		{
			throw new Exception();
			System.out.println("A");//该条无法被执行，注意与13题的区别（throw下边不能写任何语句）
		}
		catch (Exception e)
		{
			System.out.println("B");
		}

	}
	public static void main(String[] args)
	{
		try 
		{
			func();
		}
		catch (Exception e)
		{
			System.out.println("C");
		}
		System.out.println("D");
	}
}
```


### 3.写出程序结果 [答案3](#答案3)
```java
class Exc0 extends Exception
{
}
class Exc1 extends Exc0
{
}

class Demo
{
	public static void main(String[] args)
	{
		try
		{
			throw new Exc1();
		}
		catch (Exception e)
		{
			System.out.println("E");
		}
		catch(Exc0 e)
		{
			System.out.println("Exc0")
		}
	}
}
```

### 4.写出程序结果 [答案4](#答案4)
```java
class Test  
{
	public static String output="";
	public static void foo(int i)
	{
		try
		{
			if(i==1)
				throw new Exception();
			output+="1";
		}
		catch (Exception e)
		{
			output+="2";
			return;
		}
		finally
		{
			output+="3";
		}
		output+="4";
	}
	public static void main(String[] args)
	{
		foo(0);
		System.out.println(output);
		foo(1);
	System.out.println(output);
	}
}
```




## 答案1
输出结果：B C

## 答案2
编译失败
```java
public static void func()
	{
		try
		{
			throw new Exception();
			System.out.println("A");//该条无法被执行，注意与1题的区别（throw下边不能写任何语句）
		}
```

## 答案3
编译失败，多catch时，父类的catch要放在最下面


## 答案4
输出结果：134；13423
