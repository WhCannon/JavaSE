# 常用API
- [Scanner类](#scanner类)
- [System类](#system类)
- [Properties类](#propertie类)
- [Runtime类](#runtime类)
- [Math类](#math类)
- [Date类](#date类)
- [DateFormat类](#dateformat类)
- [Calendar类](#calendar类)
- [练习](#练-习) 




## Scanner类
```java
public static void main(String[] args) {
		// 创建对象
		Scanner sc = new Scanner(System.in);

		// 获取数据
		if (sc.hasNextInt()) {
			int x = sc.nextInt();
			System.out.println("x:" + x);
		} else {
			System.out.println("你输入的数据有误");
		}
	}

```


## System类

## Properties类


## Runtime类


## Math类
需求：请设计一个方法，可以实现获取任意范围内的随机数。  
```java
public static int getRandom(int start, int end) {
	// 回想我们讲过的1-100之间的随机数
	// int number = (int) (Math.random() * 100) + 1;
	// int number = (int) (Math.random() * end) + start;
	int number = (int) (Math.random() * (end - start + 1)) + start;
	return number;
}
```


## Date类
```java
Date date = new Date();//将当前时间封装成Date对象。
Date date2 = new Date(1335664696656l);//将指定毫秒值封装成Date对象。
int compareTo(Date a);比较两个日期的顺序
```


## DateFormat类
* Date-->String
```java
SimpleDateFormat的构造方法：
SimpleDateFormat():默认模式
SimpleDateFormat(String pattern):给定的模式
这个模式字符串该如何写呢?
通过查看API，我们就找到了对应的模式
年 y;月 M	;日 d;时 H;分 m;秒 s

		// 创建日期对象
		Date d = new Date();
		// 创建格式化对象
		// SimpleDateFormat sdf = new SimpleDateFormat();
		// 给定模式
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
		String s = sdf.format(d);
```
* String-->Date
```java
		String str = "2008-08-08 12:12:12";
		//在把一个字符串解析为日期的时候，请注意格式必须和给定的字符串格式匹配
		SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		Date dd = sdf2.parse(str);
```

## Calendar类
是一个抽象类。  
static Calendar getInstance();返回一个Calendar对象  
get();获取指定的时间信息（Calendar.YEAR等）  
set();设置时间  
add();在指定日期上增加时间  
```java
public static void main(String[] args) {
Calendar c = Calendar.getInstance();
		
int year = c.get(Calendar.YEAR);
int month = c.get(Calendar.MONTH)+1;//加1才与现实月份相等
int day = c.get(Calendar.DAY_OF_MONTH);
int week = c.get(Calendar.DAY_OF_WEEK);
		
System.out.println(year+"年"+month+"月"+day+"日"+getWeek(week));
}
		
public static String getWeek(int i) {
String[] weeks = {"","星期日","星期一","星期二","星期三","星期四","星期五","星期六"};
return weeks[i];
}
		
public static void showDays(int year) {//判断某一年2月有几天
		Calendar c = Calendar.getInstance();
		c.set(year, 2, 1);//将时间设置为指定年份的3月1日
		c.add(Calendar.DAY_OF_MONTH, -1);//将时间前推1天，就是2月的最后一天
		showDate(c);
	}
```



## [练 习1](#答案1)
计算2个日期"2012-3-17"到"2012-4-6"中间有多少天？





## 答案1
```java
public class DateTest {
	/**
	 * @param args
	 * @throws ParseException 
	 */
	public static void main(String[] args) throws ParseException {
		String str_date1 = "2012-3-17";
		String str_date2 = "2012-4-18";
		test(str_date1,str_date2);
	}

	public static void test(String str_date1,String str_date2) throws ParseException {
		//1,将日期字符串转成日期对象。
		//定义日期格式对象。
		DateFormat dateFormat = DateFormat.getDateInstance();
		dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		
		Date date1 = dateFormat.parse(str_date1);
		Date date2 = dateFormat.parse(str_date2);
		
		long time1 = date1.getTime();
		long time2 = date2.getTime();
		long time = Math.abs(time1-time2);
		
		int day = getDay(time);
		System.out.println(day);
	}
	
	private static int getDay(long time) {
		int day = (int)(time/1000/60/60/24);
		return day;
	}
}
```
