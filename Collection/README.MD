# 集合框架Collection
- [Collection常用方法](#1)
- [迭代器](#迭代器)
- [List接口](#2)
- [ArrayList](#3)
- [LinkedList](#LinkedList)

## 1
### Collection常用方法
### 添加
boolean add(Object obj);  
boolean addAll(Collection coll);  
### 删除
boolean remove(object obj);//remove会改变集合的长度  
boolean removeAll(Collection coll);//将内部与coll集合中相同的元素删除  
void clear();//清空
### 判断
boolean contains(object obj);  
boolean containsAll(Colllection coll);  
boolean isEmpty();//判断集合中是否有元素。  
### 获取
int size();//获取长度  
Iterator iterator();//迭代器(取出集合中元素的方式称为迭代器)  
### 其他
boolean retainAll(Collection coll);//取交集（相同的元素）  
Object[] toArray();//将集合转成数组。  

方法示例：
```java
//创建两个集合
Collection c1 = new ArrayList();
Collection c2 = new ArrayList();

//添加
//给c1添加元素。
c1.add("abc1");
c1.add("abc2");
c1.add("abc3");
c1.add("abc4");//c1=[abc1,abc2,abc3,abc4]
//给c2添加元素。
c2.add("abc2");
c2.add("abc6");
c2.add("abc7");//c2=[abc2,abc6,abc7]

c1.addAll(c2);//c1=[abc1,abc2,abc3,abc4,abc2,abc6,abc7]

//删除 
//删除会改变集合的长度
c1.removeAll(c2);//c1=[abc1,abc3,abc4]  将两个集合中相同的元素删除

//判断
boolean b =ci.containsAll(c2);//false 

//其他
c1.retainAll(c2);//c1=[abc2],取交集
```
## 迭代器
```java
Collection coll = new ArrayList();
coll.add("abc1");
coll.add("abc2");
coll.add("abc3");
coll.add("abc4");
//使用Collection中的iterator()方法, 调用集合中的迭代器方法，是为了获取集合中的迭代器对象。
Iterator it = coll.iterator();//创建一个迭代器对象		
System.out.println(it.next());//abc1   返回迭代的下一个元素
System.out.println(it.next());//abc2
System.out.println(it.next());//abc3
System.out.println(it.next());//abc4
System.out.println(it.next());//java.util.NoSuchElementException
//改为循环
for(Iterator it = coll.iterator(); it.hasNext(); ){
			System.out.println(it.next());
		}
```  
## 2
## List接口
### 添加
	void add(index,element);
	void add(index,collection);
### 删除
	Object remove(index);//返回被删除的对象
### 修改
	Object set(index,element);//返回被修改的对象
### 获取
	Object get(index);
	int indexOf(object);
	int lastIndexOf(object);
	List subList(from,to);(包括from,不包括to)
### [列表迭代器](#列表迭代器)
	ListIterator it = list.listIterator();//获取列表迭代器对象
	
方法示例：
```java
List list = new ArrayList();//创建一个List类型的变量
//添加元素
list.add("abc1");
list.add("abc2");
list.add("abc3");//[abc1,abc2,abc3]

//插入元素。	
//list.add(1,"abc9");//[abc1,abc9,abc2,abc3]
		
//删除元素。
//System.out.println("remove:"+list.remove(2));//remove:abc3   list=[abc1,abc2]
		
//修改元素。
//System.out.println("set:"+list.set(1, "abc8"));//set:abc2   list=[abc1,abc8,abc3]
		
//获取元素。
//System.out.println("get:"+list.get(0));//get:abc1
		
//获取子列表。
//System.out.println("sublist:"+list.subList(1, 2));//[abc2]
```
### 列表迭代器
```java
list.add("abc1");
list.add("abc2");
list.add("abc3");
Iterator it = list.iterator();
while(it.hasNext()){
Object obj = it.next();	//java.util.ConcurrentModificationException
if(obj.equals("abc2")){
	list.add("abc9");
			}
else
	System.out.println("next:"+obj);
		}
```
在迭代器过程中，如果使用集合操作元素，容易出现异常。  
可以使用Iterator接口的子接口ListIterator来完成在迭代中对元素进行更多的操作。
```java
ListIterator it = list.listIterator();//获取列表迭代器对象
//它可以实现在迭代过程中完成对元素的增删改查。
//注意：只有list集合具备该迭代功能.
while(it.hasNext()){
Object obj = it.next();
if(obj.equals("abc2")){
	it.add("abc9");
	}
}
```
逆向遍历  
bollean hasPrevious();//判断迭代是否有前一个元素

## 3
## ArrayList
内部是长度可变的数组数据结构，是不同步的，替代了Vector。查询的速度快。
