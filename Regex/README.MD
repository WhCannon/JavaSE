# 正则表达式
  - [常用操作](#常用操作)



## 常用操作
* 匹配  
String类中的matches方法
```java
public static void functionDemo_1(){
		//匹配手机号码是否正确。 
		String tel = "15800001111";
		String regex = "1[358][0-9]{9}";//或写成"1[358]\\d{9}"
		
		boolean b = tel.matches(regex);
		System.out.println(tel+":"+b);
	}
```

* 切割
```java

```

* 替换
```java

```

* 获取
```java

```
