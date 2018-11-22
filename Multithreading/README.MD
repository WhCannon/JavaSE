# 多线程
  - [创建线程](#创建线程)
  - [等待唤醒机制](#等待唤醒机制)
### [Lock锁](#lock锁)
### [例1：卖票示例（同步代码块）](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例1.MD)
### [例2：存钱示例（同步函数）](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例2.MD)
### [例3：多线程下的单例设计模式](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例3.MD)
### [例4：死锁](https://github.com/WhCannon/JavaSE/tree/master/Multithreading/例4.MD)
  - [Lock锁](#lock锁)
  - [练习](#练-习)


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

## 等待唤醒机制
```java
class Resource
{
	private String name;
	private String sex;
	private boolean flag = false;

	public synchronized void set(String name,String sex)
	{
		if(flag)
			try{this.wait();}catch(InterruptedException e){}
		this.name = name;
		this.sex = sex;
		flag = true;
		this.notify();
	}

	public synchronized void out()
	{
		if(!flag)
			try{this.wait();}catch(InterruptedException e){}
		System.out.println(name+"...+...."+sex);
		flag = false;
		notify();
	}
}

//输入
class Input implements Runnable
{
	Resource r ;
	Input(Resource r)
	{
		this.r = r;
	}
	public void run()
	{
		int x = 0;
		while(true)
		{
			if(x==0)
			{
				r.set("mike","nan");
			}
			else
			{
				r.set("丽丽","女女女女女女");
			}
			x = (x+1)%2;
		}
	}
}

//输出
class Output implements Runnable
{

	Resource r;
	Output(Resource r)
	{
		this.r = r;
	}

	public void run()
	{
		while(true)
		{
			r.out();
		}
	}
}

class  ResourceDemo3
{
	public static void main(String[] args) 
	{
		//创建资源。
		Resource r = new Resource();
		//创建任务。
		Input in = new Input(r);
		Output out = new Output(r);
		//创建线程，执行路径。
		Thread t1 = new Thread(in);
		Thread t2 = new Thread(out);
		//开启线程
		t1.start();
		t2.start();
	}
}
```

## lock锁
### Lock接口与Condition接口
```java
import java.util.concurrent.locks.*;

class Resource
{
	private String name;
	private int count = 1;
	private boolean flag = false;

	//创建一个锁对象。
	Lock lock = new ReentrantLock();

	//通过已有的锁获取两组监视器，一组监视生产者，一组监视消费者。
	Condition producer_con = lock.newCondition();
	Condition consumer_con = lock.newCondition();

	public  void set(String name)
	{
		lock.lock();
		try
		{
			while(flag)
			try{producer_con.await();}catch(InterruptedException e){}
			this.name = name + count;
			count++;
			System.out.println(Thread.currentThread().getName()+"...生产者5.0..."+this.name);
			flag = true;
			consumer_con.signal();
		}
		finally
		{
			lock.unlock();
		}
		
	}

	public  void out()
	{
		lock.lock();
		try
		{
			while(!flag)
			try{cousumer_con.await();}catch(InterruptedException e){}	
			System.out.println(Thread.currentThread().getName()+"...消费者.5.0......."+this.name);
			flag = false;
			producer_con.signal();
		}
		finally
		{
			lock.unlock();
		}
		
	}
}

class Producer implements Runnable
{
	private Resource r;
	Producer(Resource r)
	{
		this.r = r;
	}
	public void run()
	{
		while(true)
		{
			r.set("烤鸭");
		}
	}
}

class Consumer implements Runnable
{
	private Resource r;
	Consumer(Resource r)
	{
		this.r = r;
	}
	public void run()
	{
		while(true)
		{
			r.out();
		}
	}
}

class  ProducerConsumerDemo2
{
	public static void main(String[] args) 
	{
		Resource r = new Resource();
		Producer pro = new Producer(r);
		Consumer con = new Consumer(r);

		Thread t0 = new Thread(pro);
		Thread t1 = new Thread(pro);
		Thread t2 = new Thread(con);
		Thread t3 = new Thread(con);
		t0.start();
		t1.start();
		t2.start();
		t3.start();
	}
}
```







## 练 习
### [练习1](#答案1)
卖票示例，4个窗口同时卖100张票。

### [练习2](#答案2)
Lock与Condition接口对数组的操作





## 答案1
```java
class Ticket implements Runnable //extends Thread
{
	private  int num = 100;
	Object obj = new Object();
	public void run()
	{
		while(true)
		{
			synchronized(obj)  //同步锁
			{
				if(num>0)
				{
					try{Thread.sleep(10);}catch (InterruptedException e){}
					
					System.out.println(Thread.currentThread().getName()+".....sale...."+num--);
				}
			}
		}
	}
}

class  TicketDemo
{
	public static void main(String[] args) 
	{

		Ticket t = new Ticket();//创建一个线程任务对象。

		Thread t1 = new Thread(t);
		Thread t2 = new Thread(t);
		Thread t3 = new Thread(t);
		Thread t4 = new Thread(t);

		t1.start();
		t2.start();
		t3.start();
		t4.start();

		/*
		t1.start();
		t1.start();//一个线程不能开启两次，会抛出无效线程状态异常 
		t1.start();
		t1.start();
		*/
	}
}
```

## 答案2
* Lock与Condition接口对数组的操作
```java
class BoundedBuffer {
   final Lock lock = new ReentrantLock();
   final Condition notFull  = lock.newCondition(); 
   final Condition notEmpty = lock.newCondition(); 

   final Object[] items = new Object[100];
   int putptr, takeptr, count;

   public void put(Object x) throws InterruptedException {
     lock.lock();
     try {
       while (count == items.length) 
         notFull.await();
       items[putptr] = x; 
       if (++putptr == items.length) putptr = 0;
       ++count;
       notEmpty.signal();
     } finally {
       lock.unlock();
     }
   }

   public Object take() throws InterruptedException {
     lock.lock();
     try {
       while (count == 0) 
         notEmpty.await();
       Object x = items[takeptr]; 
       if (++takeptr == items.length) takeptr = 0;
       --count;
       notFull.signal();
       return x;
     } finally {
       lock.unlock();
     }
   } 
 }

```
