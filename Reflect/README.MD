# 反射
  - [获取字节码文件](#获取字节码文件-class)
  







## 获取字节码文件 class
```java
要想要对字节码文件进行解剖，必须要有字节码文件对象.

  /* 获取字节码对象的方式：
	 * 1、Object类中的getClass()方法。
	 * 想要用这种方式，必须要明确具体的类，并创建对象。
	 */
	public static void getClassObject_1(){
		Person p = new Person();
		Class clazz = p.getClass();
		
		Person p1 = new Person();
		Class clazz1 = p1.getClass();
		System.out.println(clazz==clazz1);//true
	}
  
  /* 方式二：
	 * 2，任何数据类型都具备一个静态的属性.class来获取其对应的Class对象。
	 * 相对简单，但是还是要明确用到类中的静态成员。
	 * 还是不够扩展。 
	 */
	public static void getClassObject_2() {
		Class clazz = Person.class;
		Class clazz1 = Person.class;
		System.out.println(clazz==clazz1);
	}
  
  /* 方式三：
	 * 只要通过给定的类的字符串名称就可以获取该类，更为扩展。
	 * 可以用Class类中的方法完成，该方法就是forName.
	 * 这种方式只要有名称即可，更为方便，扩展性更强。 
	 */
	public static void getClassObject_3() throws ClassNotFoundException {
		String className = "cn.itcast.bean.Person";//类名必须写全
		Class clazz = Class.forName(className);
		System.out.println(clazz);
	}
```
