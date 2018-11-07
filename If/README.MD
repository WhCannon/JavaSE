# 流程控制语句
  - [判断结构](#判断结构)
  - [选择结构](#选择结构)
  - [循环结构](#循环结构)
  - [break&continue](#break-continue)
  - [练习](#练习)



## 判断结构
```java
//第一种格式
if()
{ }

//第二种格式
if()//结果为bollean
{ }
else
{ }//否则执行

//第三种格式
if()
{ }
else if()
{ }
......
else
{ }
```


## 选择结构
```java
switch()
{
case 值1://若符合，则执行此case
    执行语句;
    break;//跳出
case 值2:
    ...
    ...
default:
    执行语句;
    break;
}
```
只能选择byte/short/int/char 4种数据类型

## 循环结构
```java
while()
{ }

do
{ }
while();

for(初始化表达式；条件表达式；循环后操作表达式)
{ }
```

## break&continue
* break
```java
for(int x=0;x<3;x++)
{if(x==1)
    break;
 System.out.println(x);   
}
```
* continue
```java
for(int x=0;x<11;x++)
{if(x%2==0)
    continue;
 System.out.println(x);
}
```

## 练习
### [练习1](#答案1)
找出所有的水仙花数，并统计有多少个  
所谓的水仙花数是指一个三位数，其各位数字的立方和等于该数本身。  
举例：153就是一个水仙花数。  
153 = 1 * 1 * 1 + 5 * 5 * 5 + 3 * 3 * 3 = 1 + 125 + 27 = 153  





## 答案1
```java
public class Test1 {
	public static void main(String[] args) {
	int count=0;
	for(int x=1;x<=9;x++)
	{
		for(int y=0;y<=9;y++) 
		{
			for(int z=0;z<=9;z++)
			{
				int fl=x*100+y*10+z;
				if(fl==x*x*x+y*y*y+z*z*z) 
				{
					System.out.println(fl);
					count++;
				}
			}
		}
	}
	System.out.println("水仙花数共有："+count+"个");

	for(int x=100; x<1000; x++) {
		int ge = x%10;
		int shi = x/10%10;
		int bai = x/10/10%10;
		
		//让ge*ge*ge+shi*shi*shi+bai*bai*bai和该数据比较
		if(x == (ge*ge*ge+shi*shi*shi+bai*bai*bai)) {
			//如果相同，就把该数据在控制台输出。
			System.out.println(x);
		}
	}
}
}
```
