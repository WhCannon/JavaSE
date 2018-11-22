# 多线程
  - [创建线程](#创建线程)
### [等待唤醒机制](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/等待唤醒.MD)
### [Lock，Condition接口](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/Lock.MD)
### [例1：卖票示例（同步代码块）](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例1.MD)
### [例2：存钱示例（同步函数）](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例2.MD)
### [例3：多线程下的单例设计模式](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例3.MD)
### [例4：死锁](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例4.MD)
### [例5：Lock，Condition接口](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例5.MD)


## 创建线程
### 创建线程方法1，并获取当前运行线程名称
```java
class Demo extends Thread
{
	private String name;
	Demo(String name)
	{
		super(name);
		//this.name = name;
	}
	public void run()
	{
		for(int x=0; x<10; x++)
		{
			//for(int y=-9999999; y<999999999; y++){}
			System.out.println(name+"....x="+x+".....name="+Thread.currentThread().getName());
		}
	}
}

class ThreadDemo2 
{
	public static void main(String[] args) 
	{
		Demo d1 = new Demo("旺财");
		Demo d2 = new Demo("xiaoqiang");
		d1.start();//开启线程，调用run方法。
		d2.start();
		System.out.println("over...."+Thread.currentThread().getName());
	}
}
```
### 创建线程方法2
```java
class Demo implements Runnable
{
	public void run()
	{
		show();
	}
	public void show()
	{
		for(int x=0; x<20; x++)
		{
			System.out.println(Thread.currentThread().getName()+"....."+x);
		}
	}
}

class  ThreadDemo
{
	public static void main(String[] args) 
	{	
		Demo d = new Demo();
		Thread t1 = new Thread(d);
		Thread t2 = new Thread(d);
		t1.start();
		t2.start();

	}
}
```

### 传参的特点
```java

class Thread 
{
	private Runnable r;
	Thread()
	{
	}
	Thread(Runnable r)  //子类对象传递给this.r
	{
		this.r  = r;
	}

	public void run()
	{
		if(r!=null)
			r.run();     //如果r不为空，调用目标的run方法
	}

	public void start()
	{
		run();
	}
}
class ThreadImpl implements Runnable   //实现的特点
{
	public void run()
	{
		System.out.println("runnable run");
	}
}
ThreadImpl i = new ThreadImpl();
Thread t = new Thread(i);
t.start();


class SubThread extends Thread  //继承的特点
{
	public void run()
	{
		System.out.println("hahah");
	}
}
SubThread s = new SubThread();
s.start();   //调用子类对象的run方法

```
