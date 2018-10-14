# 基本数据类型对象包装类
- [自动装箱拆箱](#自动装箱拆箱)

### 自动装箱拆箱
```java
Integer i=new Integer(4) 可简写成
Integer i=4;//自动装箱
i = i+6；//i=new Integer(i.intValue()+6),i.intValue()自动拆箱

Integer a = new Integer(128);
Integer b = new Integer(128);
System.out.println(a==b);//false
System.out.println(a.equals(b));//true
		
Integer x = 127;
Integer y = 127;
System.out.println(x==y);//true
System.out.println(x.equals(y));//true

Integer x = 128;
Integer y = 128;
System.out.println(x==y);//false
System.out.println(x.equals(y));//true
jdk1.5以后，自动装箱，如果装箱的是一个字节，那么该数据会被共享，不会重新开辟空间。
```
