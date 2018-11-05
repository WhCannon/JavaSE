# 结构语句
  - [判断结构](#判断结构)
  - [选择结构](#选择结构)
  - [循环结构](#循环结构)
  - [break&continue](#break&continue)



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
