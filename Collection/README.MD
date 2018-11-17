# 集合框架Collection
## [Collection常用方法](#collection-常用方法)
  - [迭代器](#迭代器)
## [List接口](#list)
   - [ArrayList](#arraylist)
   - [LinkedList](#linkedlist)
## [Set接口](#set)
   - [HashSet](#hashset)
      - [LinkedHashSet](#linkedhashset)
   - [TreeSet](#treeset)
## [泛型](#泛-型)
   - [泛型类](#泛型类)
   - [泛型接口](#泛型接口)
   - [泛型的限定](#泛型的限定)
## [练习](#练-习)


## Collection 常用方法
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
while(it.hasNext()){
	System.out.println(it.next());
		}
```  

## List

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
在迭代器过程中，如果使用集合操作元素，容易出现异常。  
可以使用Iterator接口的子接口ListIterator来完成在迭代中对元素进行更多的操作。
```java
ListIterator it = list.listIterator();//获取列表迭代器对象
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
```java
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
```
示例：
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


## Set
### HashSet
内部数据结构是哈希表，是不同步的。提高了查询的速度。  
#### LinkedHashSet
HashSet的子类，内部为哈希表和链表结构，变成了元素唯一的有序集合。  
### TreeSet
interface Comparable();//此接口强行对实现它的每个类的对象进行自然排序  
可以对Set集合中的元素进行排序。判断元素唯一性是根据compareTo方法,可以进行重写。 
	  
	  
如果对象不具备比较性或者其比较方法是我们不需要的，此时可以使用TreeSet集合的第二种排序方式，  
让集合自身具备比较功能：定义一个类实现Comparator（比较器）接口，覆盖compare方法，  
将该类对象作为参数传递给TreeSet集合的构造函数。
```java
/**
 * 创建了一个根据Person类的name进行排序的比较器。
 */
public class ComparatorByName implements Comparator {
public int compare(Object o1, Object o2) {
Person p1 = (Person)o1;
Person p2 = (Person)o2;
int temp = p1.getName().compareTo(p2.getName());
return temp==0?p1.getAge()-p2.getAge(): temp;
	}
}
```

## 泛 型
符号：<>  定义容器时用来明确该容器中要存储什么类型的元素（引用数据类型）  
定义格式：ArratList<String> al = new ArrayList<String>();  
	Iterator<String> it =al.iterator();//迭代时也需要用<>来明确  
### 泛型类
示例：

```java
public class ObjectTool<T> {//定义一个参数T，传入什么类型T就代表什么类型
	public void show(T t) {
		System.out.println(t);
	}
}
```

### 泛型方法
```java
	// 将泛型定义在方法上。放在返回值类型前
public <W> void show(W str){
	System.out.println("show : "+str.toString());
}
public void print(QQ str){
	System.out.println("print : "+str);
}
	
	// 当方法静态时，不能访问类上定义的泛型。如果静态方法使用泛型，只能将泛型定义在方法上。 
public static <Y> void method(Y obj){
	System.out.println("method:"+obj);
	}
```

### 泛型接口
```java
//泛型接口，将泛型定义在接口上。 
interface Inter<T>{
	public void show(T t);
}

class InterImpl implements Inter<String>{//实现时明确数据类型
	public void show(String str){
		System.out.println("show :"+str);
	}
}

class InterImpl2<Q> implements Inter<Q>{//实现时不明确数据类型，继续使用泛型
	public void show(Q q){
		System.out.println("show :"+q);
	}
}

public class GenericDefine{
public static void main(String[] args) {
InterImpl in = new InterImpl();
in.show("abc");

InterImpl2<Integer> in2 = new InterImpl2<Integer>();
in2.show(5);
	}
}
```
### 泛型的限定
通配符： ?  表示未知类型
```java
public static void main(String[] args) {
ArrayList<String> al = new ArrayList<String>();
HashSet<Integer> al2 = new HashSet<Integer>();//有多个不同的存储不同数据类型的集合，需要使用同一个工具类
}
//迭代并打印集合中的元素
public static void printCollection(Collection<?> al) {//使用Collection接收不同的集合类型，用<?>代表不同数据类型
Iterator<?> it = al.iterator();
while(it.hasNext()){
System.out.println(it.next().toString());
		}
	}
	
//与下面这种方法类似
public static <T> void printCollection(Collection<T> al) {
Iterator<T> it = al.iterator();
while(it.hasNext()){
T str=it.next();
System.out.println(str.toString());
		}
//T t=it.next;  这种方法可以对接收的类型做进一步的操作
//return t;
		
}
```
泛型的限定：  可以对泛型的类型进行限定  
? extends E：接收E类型或者E的子类型对象，上限   
? super E ：接收E类型或者E的父类型，下限  
示例：
```java
public static void printCollection(Collection<? extends Person> al) {//限定只接收Person及其子类
Iterator<? extends Person> it = al.iterator();
while(it.hasNext()){
Person p = it.next();//取出Person，只能取出父类
System.out.println(p.getName()+":"+p.getAge()); //可以使用父类中的方法
	}
}
```



## 练 习
### 练习1 [答案1](#答案1) 
请使用LinkedList来模拟一个堆栈或者队列数据结构。  
堆栈：先进后出 First In Last Out  FILO  
队列：先进先出 First In First Out FIFO  

### 练习2 [答案2](#答案2) 
以Person对象年龄进行从小到大的排序  

### [练习3](#答案3)
ArrayList去除集合中重复的元素




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

## 答案3
* 方法一
```java
1.创建一个新集合
2.遍历旧集合，获取每一个元素
3.拿获取到的元素到新集合去找，如果没有就添加到新集合，如果有就不添加
public static void main(String[] args) {
		// 创建集合对象
		ArrayList array = new ArrayList();

		// 添加多个字符串元素(包含内容相同的)
		array.add("hello");
		array.add("world");
		array.add("java");
		array.add("world");
		array.add("java");
		array.add("world");
		array.add("world");
		array.add("world");
		array.add("world");
		array.add("java");
		array.add("world");

		// 创建新集合
		ArrayList newArray = new ArrayList();

		// 遍历旧集合,获取得到每一个元素
		Iterator it = array.iterator();
		while (it.hasNext()) {
			String s = (String) it.next();
			// 拿这个元素到新集合去找，看有没有
			if (!newArray.contains(s)) {
				newArray.add(s);
			}
		}

		// 遍历新集合
		for (int x = 0; x < newArray.size(); x++) {
			String s = (String) newArray.get(x);
			System.out.println(s);
		}
	}
```
* 方法二
```java
1.从第一个元素开始，依次和后面的元素比较，如果有相同的，就删除后面的
2.同理，第二、第三个元素....

for (int x = 0; x < array.size() - 1; x++) {
			for (int y = x + 1; y < array.size(); y++) {
				if (array.get(x).equals(array.get(y))) {
					array.remove(y);
					y--;
				}
			}
		}
		// 遍历集合
		Iterator it = array.iterator();
		while (it.hasNext()) {
			String s = (String) it.next();
			System.out.println(s);
		}
```
