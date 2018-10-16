# 集合框架Collection
## [Collection常用方法](#collection常用方法)
  - [迭代器](#迭代器)
## [List接口](#list接口)
   - [ArrayList](#arraylist)
   - [LinkedList](#linkedlist)
## [Set接口](#set接口)
## [练习](#练习)


## Collection常用方法
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

### 迭代器
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
### 逆向遍历  
bollean hasPrevious();//判断迭代是否有前一个元素


### ArrayList
内部是长度可变的数组数据结构，是不同步的，替代了Vector。查询的速度快。
```java
//ArrayList集合存储自定对象
public static void main(String[] args) {
Person p1 = new Person("lisi1",21);
ArrayList al = new ArrayList();

al.add(p1);
al.add(new Person("lisi2",22));
al.add(new Person("lisi3",23));
al.add(new Person("lisi4",24));

Iterator it = al.iterator();
while(it.hasNext()){
//System.out.println(((Person) it.next()).getName()+"::"+((Person) it.next()).getAge());  lisi1::22,lisi3::24
//每次调用next方法都会向后移一位

Person p = (Person) it.next();//进行强转
System.out.println(p.getName()+"--"+p.getAge());
}
```

### LinkedList
内部是链表数据结构，是不同步的。增删元素的速度快。  
void addFirst(E);//将是定元素插入列表的开头  
void addLast(E);  
E getFirst();//返回列表的第一个元素  
E getLast();  
E removeFirst();  
E removeLast();  
上面这些方法使用时，如果链表为空，会抛出NoSuchElementException。因此，有以下改善的方法：  
offerFirst();//插入  
offerLast();  
peekFirst();//获取但不移除，如果列表为空，返回null  
peekLast();  
pollFirst();//获取并移除，如果列表为空，返回null  
pollLast();  
```java
LinkedList link = new LinkedList();
link.addFirst("abc1");
link.addFirst("abc2");
link.addFirst("abc3");
link.addFirst("abc4");//[abc4,abc3,abc2,abc1]

System.out.println(link.getFirst());//abc4   //获取第一个但不删除。
System.out.println(link.getFirst());//abc4

System.out.println(link.removeFirst());//abc4  //获取元素但是会删除。
System.out.println(link.removeFirst());//abc3

```

## 练习
### 练习1 [答案1](#答案1) 
请使用LinkedList来模拟一个堆栈或者队列数据结构。  
堆栈：先进后出 First In Last Out  FILO  
队列：先进先出 First In First Out FIFO  

### 练习2 [答案2](#答案2) 
以Person对象年龄进行从小到大的排序  





### 答案1
```java
package cn.itcast.p2.linkedlist.test;
import java.util.ArrayList;

public class LinkedTest {
	public static void main(String[] args) {
		DuiLie dl = new DuiLie();
		dl.myAdd("abc1");
		dl.myAdd("abc2");
		dl.myAdd("abc3");
		dl.myAdd("abc4");
		//队列
		while(!dl.isNull()){
			System.out.println(dl.myGet());
		}
	}
}

public class DuiLie {
	private LinkedList link;

	public DuiLie() {
		link = new LinkedList();
	}

	/**
	 * 队列的添加元素的功能。
	 */
	public void myAdd(Object obj) {
		link.addLast(obj);
	}
	//先进先出
	public Object myGet() {
		return link.removeFirst();
	}
	public boolean isNull() {
		return link.isEmpty();
	}
}
```

### 答案2
```java
public int compareTo(Object o) {
Person p = (Person)o;
int  temp = this.age-p.age;
return temp==0?this.name.compareTo(p.name):temp;
}

public static void main(String[] args) {
TreeSet ts = new TreeSet(new ComparatorByName());

ts.add(new Person("zhangsan",28));
ts.add(new Person("lisi",21));
ts.add(new Person("zhouqi",29));
ts.add(new Person("zhaoliu",25));
ts.add(new Person("wangu",29));
		
Iterator it = ts.iterator();
		
while(it.hasNext()){
Person p = (Person)it.next();
System.out.println(p.getName()+":"+p.getAge());
}
}
输出结果：lisi:21 zhaoliu:25 zhangsan:28 wanggu:29 zhouqi:29
```
