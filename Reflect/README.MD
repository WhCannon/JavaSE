# 反射
  - [获取字节码文件对象](#获取字节码文件对象-class)
  - [获取Class中的构造函数](#获取class中的构造函数)
  - [获取Class中的字段](#获取class中的字段)
  - [获取Class中的函数](#获取class中的函数)
  







## 获取字节码文件对象 Class
```java
要想要对字节码文件进行解剖，必须要有字节码文件对象.

  /* 获取字节码对象的方式：
  方式一：
	 * Object类中的getClass()方法。
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
	 * 任何数据类型都具备一个静态的属性.class来获取其对应的Class对象。
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

## 获取Class中的构造函数
```java
public static void createNewObject_2() throws Exception {
		cn.itcast.bean.Person p = new cn.itcast.bean.Person("小强",39);
		
		/* 当要获取指定名称对应类中所体现的对象时，
		 * 而该对象初始化不使用空参数构造该怎么办呢？
		 * 既然是通过指定的构造函数进行对象的初始化，
		 * 所以应该先获取到该构造函数。 
		 * 通过字节码文件对象即可完成，该方法是：getConstructor(paramterTypes); 
		 */
		String name = "cn.itcast.bean.Person";
		//找寻该名称类文件，并加载进内存，并产生Class对象。
		Class clazz = Class.forName(name);
		//获取指定的构造函数对象
		Constructor constructor = clazz.getConstructor(String.class,int.class);
		
		//通过该构造器对象的newInstance方法进行对象的初始化。
		Object obj = constructor.newInstance("小明",38);
	}

	public static void createNewObject() throws ClassNotFoundException, InstantiationException, IllegalAccessException{
		//早期：new的时候，先根据被new的类的名称找寻该类的字节码文件，并加载进内存，
		//然后创建该字节码文件对象，再创建该字节文件对应的Person对象.
		cn.itcast.bean.Person p = new cn.itcast.bean.Person();
		
		//现在：
		String name = "cn.itcast.bean.Person";
		//找寻该名称类文件，并加载进内存，并产生Class对象。
		Class clazz = Class.forName(name);
		//如何产生该类的对象呢？
		Object obj  = clazz.newInstance();//创建一个该类的新实例
	}
```

## 获取Class中的字段
```java
//	 获取字节码文件中的字段。
	public static void getFieldDemo() throws Exception {
		Class clazz = Class.forName("cn.itcast.bean.Person");
		
		Field field = null;//clazz.getField("age");//只能获取公有的
		field = clazz.getDeclaredField("age");//只获取本类，但包含私有 
		
		//对私有字段的访问取消权限检查。暴力访问。
		field.setAccessible(true);
		
		Object obj = clazz.newInstance();
		field.set(obj, 89);//设置值
		Object o = field.get(obj);//需要指定对象来调用
		System.out.println(o);
	}
```

## 获取Class中的函数
```java
//获取有参的函数
	public static void getMethodDemo_3() throws Exception {
		Class clazz = Class.forName("cn.itcast.bean.Person");
		//指定方法名和参数
		Method method = clazz.getMethod("paramMethod", String.class,int.class);
		Object obj = clazz.newInstance();
		method.invoke(obj, "小强",89);//调用并传值
	}
	
	//获取一个空参函数
	public static void getMethodDemo_2() throws Exception {
		Class clazz = Class.forName("cn.itcast.bean.Person");
		
		//需要指定函数名和参数列表
		Method method = clazz.getMethod("show", null);//获取空参数一般方法。
		
		//	Object obj = clazz.newInstance();
		Constructor constructor = clazz.getConstructor(String.class,int.class);
		Object obj = constructor.newInstance("小明",37);
		
		method.invoke(obj, null);//运行函数，需要指定对象和参数列表
	}

	//	 获取指定Class中的所有公共函数。
	public static void getMethodDemo() throws Exception {
		Class clazz = Class.forName("cn.itcast.bean.Person");
		
		Method[] methods  = clazz.getMethods();//获取的都是公有的方法（包括父类中的）
		methods = clazz.getDeclaredMethods();//只获取本类中所有方法，包含私有。
		for(Method method : methods){
			System.out.println(method);
		}
```
