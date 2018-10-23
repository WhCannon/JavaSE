# 常用API
- ## [System类](#system类)
- ## [Properties类](#propertie类)
- ## [Runtime类](#runtime类)
- ## [Math类](#math类)
- ## [Date类](#date类)
- ## [DateFormat类](#dateformat类)
- ## [练习](#练-习) 





## System类

## Properties类


## Runtime类


## Math类



## Date类

## DateFormat类


## [练 习](#答案1)
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
