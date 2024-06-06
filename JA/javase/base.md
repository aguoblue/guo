# 2.26
数据类型

基础类型

```ascii 
       ┌───┐
 byte  │   │
       └───┘
       ┌───┬───┐
 short │   │   │
       └───┴───┘
       ┌───┬───┬───┬───┐
   int │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
  long │   │   │   │   │   │   │   │   │    9999999999L
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┬───┬───┐
 float │   │   │   │   │    12.1F
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
double │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┐
  char │   │   │
       └───┴───┘
	        ┌───┐
	boolean │   │
		    └───┘
```

引用类型


字符串类型  双引号



逻辑运算符和短路运算符

三元运算符

switch 的新特性

数组的内存



# 2.27

方法、方法重载
	同类 同名 不同参（个数、类型、顺序）
		方法签名：**方法名称 + 参数类型 + 参数个数** 组成的一个唯一值，**JVM（Java 虚拟机）** 就是通过这个方法签名来决定调用哪个方法的
	匹配原则
		匹配原则1：精准类型匹配
			方法重载会优先调用和方法参数类型一模一样的方法，这是第一优先匹配原则：精准类型匹配。
		匹配原则2：
			基本类型自动转换成更大的基本类型
		匹配原则3：
			匹配自动装箱或拆箱的数据类型。
		匹配原则4：
			依次向上匹配父类的方法调用。
			如果一个类没有显式地继承任何类，则默认继承 java.lang.Object 类。
		匹配原则4：
			匹配可选参数
构造方法：
	首先加载两个class文件进入方法区
	main方法入栈，然后执行语句Phone p=new Phone（）；
	new对象创建之前，首先会去方法区找是否有对应的class文件，有，然后把class文件的成员变量拿到新创建的对象中。而方法不用拿，调用时，只需取出来用即可。
	然后把成员变量赋值，
	调用方法，通过地址调用去方法区找，调用的方法入栈
	方法调用完毕，则从内存中消失，整个做完，最后main方法出栈
	class加载后就不用再加载了，直接调用
	
	Phone p=new Phone（）
	1. 加载Phone.class文件进内存
	2. 在栈内存为p开辟空间
	3. 在堆内存为手机对象开辟空间
	4. 对手机对象的成员变量进行默认初始化
	5. 对手机对象的成员变量进行显示初始化
	6. 通过构造方法对手机对象的成员变量赋值
	7. 手机对象初始化完毕，把对象地址赋值给s变量
	默认初始化是系统在堆内存创建一个新的对象时，进行的默认初始化，如null 和0
	显示初始化是在类定义时，直接在各个成员变量的定义时，优先进行赋值，这叫显示初始化。

继承：
	复用代码
	子类自动获得了父类的所有字段，严禁定义与父类重名的字段！
	没有明确写`extends`的类，编译器会自动加上`extends Object`。所以，任何类，除了`Object`，都会继承自某个类
	只允许一个class继承自一个类，因此，一个类有且仅有一个父类。只有`Object`特殊，它没有父类。
	子类无法访问父类的`private`字段或者`private`方法，用`protected`修饰的字段可以被子类访问，`protected`关键字可以把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问
	`super`关键字表示父类（超类）
	任何`class`的构造方法，第一行语句必须是调用父类的构造方法。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super();`所有要注意看父类是否有无参构造方法，或者super(参数)
	`final`修饰符**阻止继承**
	Java 15开始，允许使用`sealed`修饰class，并通过`permits`明确写出能够从该class继承的子类名称。启用它，必须使用参数`--enable-preview`和`--source 15`。
``` java
public sealed class Shape permits Rect, Circle, Triangle {
}
```

把一个子类类型安全地变为父类类型的赋值，被称为**向上转型**（upcasting）。实际上是把一个子类型安全地变为更加抽象的父类型。
把一个父类类型强制转型为子类类型，就是**向下转型**（downcasting）。
实际类型是`Person`，不能把父类变为子类`Student`，因为子类功能比父类多，多的功能无法凭空变出来。
为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型
在Java 14之前，如果你想要检查一个对象是否是特定类型的实例，并且打算将其转换为那个类型，你需要首先使用`instanceof`进行类型检查，然后再进行显式的类型转换。
```java
Object obj = "This is a string";

if (obj instanceof String) {
    String str = (String) obj;
    System.out.println(str.toUpperCase());
}

```
从Java 14开始，你可以在进行`instanceof`检查的同时直接声明一个新的变量来引用被检查对象的转换类型
```java
Object obj = "This is a string";

if (obj instanceof String str) {
    // 在这个范围内，str直接被识别为String类型，无需显式转换
    System.out.println(str.toUpperCase());
}
```
在这个新的语法中，`str`是在`instanceof`表达式中声明的变量，它自动被类型检查和转换为`String`。如果`obj`实际上是一个`String`实例，那么`str`就可以直接使用，无需任何额外的类型转换。这种特性使得代码不仅更加简洁，而且减少了出错的可能性，因为它避免了不必要的类型转换和相关的变量声明。

继承和组合
组合，把其他类作为类内的属性字段



String
	字符串常量  字符串串池 StringTable
	String类
	比较 equals()
	StringBuilder
		toString()
		append
		reverse
	StringJoiner(间隔符合，起始，结束)
		add
		toString()

### 使用集合框架的类

1. **List接口的实现**：创建列表（可以包含重复元素）。
    
    - `ArrayList`：一个基于动态数组的实现，提供快速的随机访问。

        `List<String> arrayList = new ArrayList<>();`
        
    - `LinkedList`：基于双向链表的实现，优化了插入和删除操作。

        `List<String> linkedList = new LinkedList<>();`
        
2. **Set接口的实现**：创建集合（不包含重复元素）。
    
    - `HashSet`：基于哈希表的实现，存储无序的唯一元素。
   
        `Set<String> hashSet = new HashSet<>();`
        
    - `LinkedHashSet`：基于哈希表和链表的实现，维护元素的插入顺序。

        `Set<String> linkedHashSet = new LinkedHashSet<>();`
        
    - `TreeSet`：基于红黑树的实现，元素会按照自然顺序或自定义的比较器进行排序。

        `Set<String> treeSet = new TreeSet<>();`
        
3. **Map接口的实现**：创建键值对的映射（键是唯一的）。
    
    - `HashMap`：基于哈希表的实现，存储键值对，允许快速访问。

        `Map<String, Integer> hashMap = new HashMap<>();`
        
    - `LinkedHashMap`：基于哈希表和链表的实现，维护键值对的插入顺序。
        
        `Map<String, Integer> linkedHashMap = new LinkedHashMap<>();`
        
    - `TreeMap`：基于红黑树的实现，键会按照自然顺序或自定义的比较器进行排序。

        `Map<String, Integer> treeMap = new TreeMap<>();`
        

### 使用工具类

Java还提供了工具类如`Collections`和`Arrays`，简化集合的创建和初始化。

1. **Collections类**：提供静态方法来操作或返回集合。
    
    - 使用`Collections.emptyList()`, `Collections.emptySet()`, `Collections.emptyMap()`来创建空的不可变集合。
    - `Collections.singletonList()`, `Collections.singleton()`, `Collections.singletonMap()`创建只包含单一元素的不可变集合。
2. **Arrays类**：主要用于操作数组，但其`asList`方法可用于创建固定大小的列表。
```java
List<String> list = Arrays.asList("element1", "element2", "element3");
```
3. **Java 9及以上的List, Set, Map静态工厂方法**：简化了小集合的创建。
``` java
List<String> immutableList = List.of("a", "b", "c"); 
Set<String> immutableSet = Set.of("a", "b", "c"); 
Map<String, Integer> immutableMap = Map.of("a", 1, "b", 2);
```


包装类

Java的包装类是指将Java的基本数据类型封装在一个类中，提供了一种方式将基本类型当作对象操作。这些类属于java.lang包，每个基本数据类型都有一个对应的包装类。这些包装类主要用于在需要对象而非基本类型时使用，例如在集合类中存储数据，或者提供了将基本类型与字符串之间转换的方法。

以下是Java基本数据类型及其对应的包装类：

1. **byte** - **Byte**
2. **short** - **Short**
3. **int** - **Integer**
4. **long** - **Long**
5. **float** - **Float**
6. **double** - **Double**
7. **char** - **Character**
8. **boolean** - **Boolean**

### 使用包装类的优点

- **对象操作**：允许将基本数据类型作为对象处理，这在Java的集合框架中尤为重要，因为集合框架（如`ArrayList`，`HashMap`等）只能存储对象。
- **提供了额外的方法**：包装类提供了许多有用的方法，例如将字符串转换为相应的基本数据类型值，以及反之亦然的转换。
- **支持null值**：包装类可以赋值为`null`，表示缺失或未定义的状态，而基本类型不支持`null`值。

### 自动装箱和拆箱

从Java 5开始，引入了自动装箱和拆箱的概念，以简化包装类和基本数据类型之间的转换。

- **自动装箱（Autoboxing）**：编译器自动将基本数据类型转换为对应的包装类对象。例如，当你将一个`int`赋值给一个`Integer`对象时。
    
    javaCopy code
    
    `Integer i = 10; // 自动装箱，将int转换为Integer`
    
- **自动拆箱（Unboxing）**：编译器自动将包装类对象转换回对应的基本数据类型。例如，当你将一个`Integer`对象赋值给一个`int`类型时。
    
    javaCopy code
    
    `int n = i; // 自动拆箱，将Integer转换为int`
    

这些特性使得在编程时可以更加灵活地在基本类型和对象类型之间转换，同时保持代码的简洁性。





# 2.27

继承、继承的内存
	虚方法表 非private 非final 非static
	重写方法

接口的静态字段：  public static final  ，`interface`是可以有静态字段的，并且静态字段必须为`final`类型
接口可以有静态方法
接口的default 默认方法
1. **向后兼容**：允许开发者向接口添加新方法，而不破坏已经发布的或正在使用这个接口的现有代码基。在引入默认方法之前，一旦一个接口被发布，如果需要添加新方法，就必须修改实现该接口的所有类。默认方法解决了这个问题。
2. **多重继承的行为**：默认方法为Java添加了类似多重继承的能力，允许一个类从多个接口继承行为（即方法实现），而不仅仅是类型。这增加了Java接口的灵活性和功能性。
3. 默认方法可以继承也可以重写，
子类确实可以实现一个或多个接口的同时继承一个父类，即使父类没有实现这些接口。
1. **父类变量引用子类对象**：Java允许使用父类类型的变量来引用一个子类的实例。这是面向对象多态性的一部分，允许子类对象被看作是父类对象的一个特殊版本。
    
2. **接口方法的调用**：如果子类实现了一个或多个接口，那么它必须或可以实现这些接口中的方法。然而，如果你通过父类类型的引用来操作这个子类实例，那么只能调用那些在父类中声明或继承的方法。你将无法直接通过这个父类引用调用子类实现的接口中的方法，除非你进行向下转型（强制类型转换）到具体的子类或实现的接口类型。
    
3. **向下转型**：为了调用子类特有的方法或子类实现的接口方法，你需要将父类的引用向下转型为子类的类型或该子类实现的接口类型。向下转型需要显式地进行，并且有运行时的类型检查。如果转型是非法的，它会引发`ClassCastException`。
适配器设计模式：中间类实现接口，对接口所有方法进行空实现，再使用一个类继承中间类，然后重写需要的方法

包

内部类
	成员内部类
	局部内部类
	静态内部类 static
	匿名类
	举例：ArrayList内部类Itr迭代器类，用来遍历ArrayList，得现有ArrayList，再有内部类Itr

抽象类



Object
	ToString()
	equals()
		注意重写的
	clone()
		浅拷贝

BigInteger
BigDecimal

正则表达式

Arrays 操作数组的工具类

lambda表达式
	函数式接口，接口、只有一个方法、



# 3.1




# 3.2
	
自动装箱
	创建“新”对象的静态方法称为静态工厂方法。`Integer.valueOf()`就是静态工厂方法，它尽可能地返回缓存的实例以节省内存。
	自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；
	装箱和拆箱会影响执行效率，且拆箱时可能发生`NullPointerException`；


# 3.4

List  编写equals()方法   new ArrayList<>();
1. 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
2. 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
3. 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。
Map  
HashMap
	new HashMap<>();
	for (String key : map.keySet())
	for (Map.Entry<String, Integer> entry : map.entrySet())
		String key = entry.getKey();
		Integer value = entry.getValue();
	编写equals()方法和hashCode()方法
	实现`hashCode()`方法可以通过`Objects.hashCode()`辅助方法实现。
	HashMap内部找到的实际上是`List<Entry<String, Person>>`
	自动扩容
EnumMap
	如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。
	使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。
TreeMap
	`TreeMap`在Java中是一个基于红黑树实现的`Map`接口的实现，它存储键值对，且按照键的自然顺序或者`Comparator`进行排序

使用Properties
	配置文件的特点是，它的Key-Value一般都是`String`-`String`类型的，因此我们完全可以用`Map<String, String>`来表示它。
```
# setting.properties

last_open_file=/data/hello.txt
auto_save_interval=60
```

```java
String f = "setting.properties";
Properties props = new Properties();
props.load(new java.io.FileInputStream(f));
String filepath = props.getProperty("last_open_file");
String interval = props.getProperty("auto_save_interval", "120");
```

Set
	`HashSet`是无序的，因为它实现了`Set`接口
	`SortedSet`接口，保证元素是有序
	`TreeSet`是有序的，因为它实现了`SortedSet`接口。




# 3.5

迭代器 hasNext   next   remove
```java
Collection<String> list = new ArrayList<>();
Iterator<String> it = list.iterator();
while(it.hasNext()){
	String s = it.next();
}
```

增强for遍历只适合单列集合和数组，不适合双列集合，底层是迭代器，为了简化迭代器代码，JDK5之后出现。
```
for(T t : 数组或者单列集合){

}
```

Lambda表达式遍历 forEach
JDK8之后
```java
void forEach(Consumer<? super T> action)

list.forEach(new Consumer<String>() {
	@Override
	public void accept(String s){
		sout(s);
	}
})

list.forEach( s -> sout(s))



```

`Consumer<? super T>`是一个函数式接口，它定义了一个`accept`方法，用于接受一个参数并执行某些操作。


列表迭代器
	列表迭代器（ListIterator）是Java集合框架中的一个接口，它扩展了普通迭代器（Iterator）的功能，提供了双向遍历列表的能力，并且可以在遍历过程中对列表进行增删操作。
	列表迭代器可以通过`previous`方法向前遍历列表，也可以通过`add`、`remove`和`set`方法在遍历过程中修改列表的内容。
	列表迭代器只能用于列表类型的集合（如ArrayList、LinkedList等），而不能用于Set等其他类型的集合。



ArrayList、LinkedList、迭代器源码

泛型类、方法、接口、
public < T > void show (T t)
	泛型的通配符
		? 表示不确定的类型
		进行类型限定
		？extends E : 表示可以传递E或E的子类类型
		？super E ： 表示可以传递E或者E所有的父类类型
		public static void method(ArrayList< ?  extends E> list)
泛型无基础，数据具备继承

# 3.6
二叉树
二叉查找树
平衡二叉树
	右旋、左旋
	左左    一次右旋
	左右    局部左旋变成左左、整体右旋
	右右    一次左旋
	右左    局部右旋变成右右、整体左旋

红黑树

HashSet 
	无序
	jdk8前 数组+链表 后 数组+链表+红黑树
	链表长度大于8、数组长度大于64，转化为红黑树
LinkedHashSet
	存取有序
	底层也是哈希表，每个元素多了个双链表机制记录顺序
TreeSet
	不重复、有序
	**可排序**：默认按元素从小到大   
	底层基于红黑树数据结构实现排序


# 3.7
### `ArrayDeque`
内部使用一个循环数组，没有同步开销，作为栈使用时提供了更高效的操作，作为双端队列，`ArrayDeque`允许从两端添加或移除元素，不允许存储`null`元素，需要偶尔进行数组扩展和收缩
作为栈
```java
Deque< Integer > stack = new ArrayDeque<>();
stack.push(1); // 入栈 
stack.push(2);
int top = stack.peek(); // 查看栈顶元素，不移除 
int popped = stack.pop(); // 出栈
```
作为队列
```java
Deque< Integer > queue = new ArrayDeque<>(); 
queue.offer(1); // 在队尾添加元素 
queue.offer(2); 
int head = queue.peek(); // 查看队头元素，不移除 
int removed = queue.poll(); // 移除队头元素
```

大根堆、小根堆

下沉操作
```java
public void sink(int[] heap, int k, int n) {
    while (2 * k <= n) { // 确保有子节点
        int j = 2 * k; // 左子节点的索引
        if (j < n && heap[j] < heap[j + 1]) { // 如果存在右子节点，且右子节点大于左子节点
            j++; // 使用较大的子节点进行比较
        }
        if (!(heap[k] < heap[j])) { // 如果当前节点大于等于两个子节点，停止下沉
            break;
        }
        // 交换当前节点与较大子节点的位置
        int temp = heap[k];
        heap[k] = heap[j];
        heap[j] = temp;
        // 继续下沉
        k = j;
    }
}
```


上浮
```java
private void swim(int k) { 
	while (k > 1 && heap[k / 2] < heap[k]) { 
		// 父节点小于当前节点，需要交换 
		swap(k, k / 2); 
		k = k / 2; // 继续向上比较 
	} 
}
```

插入
```java
public void insert(int x) { 
	if (n == heap.length - 1) { 
	// 如果数组已满，扩容操作（略） 
	} 
	// 将新元素添加到堆的末尾 
	n++; 
	heap[n] = x; 
	// 上浮调整 
	swim(n); 
}
```




# 3.8
Map 顶层接口
	遍历方法
		获取所有的键放到一个单列集合 Set< T > keys = map.keySet();
		Set< Map.Entry<T,K>> entries = map.entrySet()
HashMap
	哈希表结构，只根据键的哈希值
	数组+链表+红黑树
	源码学习
LinkedHashMap
	存取顺序有序
TreeMap
	不重复、有序、对键进行排序
	**可排序**：默认按元素从小到大   
	底层基于红黑树数据结构实现排序
可变参数
Collections集合类
不可变集合
	List.of()
	Set.of()
	Map< K,V>.of   Map.copyOf()

Stream流
	单列集合 Collections中的默认方法  list.stream()
	双列集合  map.keySet().stream()
	数组  Arrays.stream()
	一堆零散数据  Stream.of(1,2,3,4,5,6,9)
	filter 过滤
	limit 留下前几个元素
	skip 跳过前几个元素
	distinct 元素去重，底层用HashSet，依赖hashCode和equals
	concat 合并两个流
	map 转换流中的数据类型
	流的终结方法
		forEach 遍历
		count  统计流中个数
		toArray  收集数据，放到数组中
		collect    收集数据，放到集合中
			collect(Collectors.toList())
			collect(Collectors.toSet())
			collect(Collectors.toMap(,))
方法引用
	引用静态方法
	引用其他类的成员方法
	引用本类成员方法
	引用父类成员方法
	引用构造方法
	引用数组的构造方法
	类名::方法名
	对象::方法名

异常
	编译时异常
	运行时异常
	自定义异常

try catch
# 3.10

## File
文件、文件夹
判断、获取
创建、删除
获取遍历

## IO流
### 基本流
字节流
	InputStream
	OutputStream
	操作文件
		FileInputStream
			read() 读到末尾返回-1  两种重载
		FileOutputStream
			write()  三种重载
			换行和续写  是否续写在构造的时候加上append true?false
	释放资源
	close()  先开启的流最后关闭
	异常
		try catch finally
		创建对象有编译时异常
		释放资源有编译时异常
		释放资源需要判断非空，否则有运行时异常
		自动释放资源 实现AutoCloseable接口 省略close

字符集
GBK 系统显示ANSI  中日韩
	汉字 两个字节 高位字节1开头
	英文 一个字节 兼容assic
Unicode
编码规则
UTF  Unicode Transfer  Format
	UTF-8  1-4个字节
		ASSIC 1个字节      0xxxxxxx
		其他   2个              110xxxxx  10xxxxxx
		中文   3个字节	      1110xxxx   10xxxxxx  10xxxxxx   汉字->查询Unicode->放到xx
		其他   4个              11110xxx  10xxxxxx 10xxxxxx 10xxxxxx
编码解码
String.getBytes()
String()

字符流  一次读一个字节，遇到中文一次读多个字节，读取规则遵循指定字符集规则
	Reader
	Writer
	操作文件
		FileReader
			接收文件对象 创建时关联文件，创建缓冲区8192字节数组
			read()  read(Char [ ]  buffer)   读取后解码，返回整数，有参返回读取到的个数，读到文件尾返回-1
		FileWriter
			接收文件对象 创建时关联文件，创建缓冲区8192字节数组
			write(int c) 把c编码后写到文件
			wirte(String s)    ！！！！
			write(Char [ ]  cbuf)
			flush()



### 高级流
包装基本流
#### 缓冲流
带缓冲区的流，创建对象时关联基本字节流FileInputStream  FileOutputStream
	BufferedInputStream
	BufferedOutputStream
	默认8192  创建对象时可设置  byte []   8kb
带缓冲区的流，创建对象时关联基本字符流  FileReader FileWriter
	默认8192  创建对象时可设置  char []   16kb
	BufferedReader
		String readLine() 读取一行，无则返回null，不会把换行符读到内存
	BufferedWriter
		void newLine() 跨平台的换行
		

#### 转换流
是字符流和字节流之间的桥梁
	数据源 ->  字节流 + 转换流 -> 内存   InputStreamReader(new FileInputStream, )
		有字符流的特性 不乱码、一次读取多个字节
	内存  -> 转换流  + 字节流 -> 数据源  OutputStreamWriter
```java
FileInputStream fis = new FileInputStream("base-note\\src\\test.txt");  
InputStreamReader isr = new InputStreamReader(fis);  
BufferedReader br = new BufferedReader(isr);
```

#### 序列化流/反序列化流
序列化流/对象操作输出流
	ObjectOutputStream(OutputStream out) 
		writeObject(Object obj)
反序列化流/对象操作输入流
	ObjectInputStream(InputStream out) 
		readObject()
	类的属性加上transient关键字修饰，则该属性不参与序列化
	读写多个对象，可以把对象放到ArrayList里面

接口里没有抽象方法，则为标记型接口
	Serializable 实现该接口的类可以被序列化


#### 打印流
只操作文件目的地，不操作数据源
特有方法原样写出
特有方法自动刷新、自动换行      --- 写出+换行+刷新
	PrintStream  四种构造  字节打印流
	PrintWriter
		write
		println
		print
		printf

#### 压缩流
解压缩
ZipInputStream(new FileInputStream("xxx"));
```java
ZipInputStream zis = new ZipInputStream(new FileInputStream("xxx"));  
ZipEntry zipEntry;  
while((zipEntry = zis.getNextEntry()) != null){  
    System.out.println(zipEntry);  
    if(zipEntry.isDirectory()){  
  
    }else{  
        //file  
        //下面只是表示有这几个方法，具体怎么用还需要查阅  
        zis.read();  
        //  
        zis.closeEntry();  
    }  
}  
zis.close();
```
压缩
ZipOutputStream(new FileInputStream("xxx"));


Commons-io 工具包
Hutool 工具包


# 3.11
#### 多线程
并发和并行
实现
	没有返回值  void run
		继承Thread类的方式实现
		实现Runnable接口
			Thread thread = Thread.currentThread();得到当前线程对象
	获取到多线程运行的结果
		利用Callable接口和Future接口
		重写call，有返回值
```java
MyCallable mc = new MyCallable();  
FutureTask<Integer> ft = new FutureTask<>(mc);  
Thread t1 = new Thread(ft);  
t1.start();  
Integer i = ft.get();
```
方法
	getName
	setName
	static currentThread
	static sleep  ms
	setPriority
	getPriority   默认是5   抢占式调度
	setDaemon  设置为守护线程
	static yield  礼让线程
	static join  插入线程
生命周期
	新建
	就绪   包括运行    
	阻塞    等待锁
	等待    wait
	计时等待 sleep
	终止

守护线程（Daemon Thread）在Java中是一种特殊类型的线程，主要用于为其他线程（通常是用户线程）提供服务或执行后台任务，如垃圾收集、JVM监控和日志记录等。守护线程的最大特点是它不会阻止JVM的退出。当所有非守护线程（即用户线程）都结束时，JVM会自动退出，此时不会考虑守护线程是否还在运行。
- **使用守护线程时的考虑**：由于守护线程可能在任何时候被JVM终止，因此在设计守护线程执行的任务时要特别小心。确保它们执行的操作可以安全地被中断，不会在程序退出时留下破坏性的后果。
    
- **资源释放和清理**：如果守护线程正在操作文件、数据库连接等资源，那么它可能在JVM退出时没有机会正常清理和释放这些资源。因此，最好让用户线程负责资源的清理工作，或确保使用适当的机制来避免资源泄露。

#### 同步 锁
同步代码块
	synchronized(obj){   }  obj锁对象，需要保证唯一
同步方法
	synchronized修饰方法
		锁住方法里的所有代码
		锁对象不能自己指定，非静态 ： this   静态：当前类的字节码文件对象
锁对象
	Lock是接口
	实现类ReentrantLock
		lock()
		unlock()
Java中的每个对象都有`wait()`方法，因为这个方法是`java.lang.Object`类的一部分，而Java中的所有类都是继承自`Object`。`wait()`方法用于让当前线程等待，直到另一个线程调用同一个对象的`notify()`或`notifyAll()`方法，或者直到超过指定的时间量。
1. **在同步代码块中调用**：`wait()`, `notify()`, 和 `notifyAll()`方法必须在同步代码块或同步方法中调用，也就是说，调用线程必须拥有该对象的锁。这是因为调用`wait()`方法会释放锁，而调用`notify()`或`notifyAll()`方法则是通知其他在等待这个锁的线程。
    
2. **释放锁**：当线程调用某个对象的`wait()`方法时，它会释放该对象的锁，这使得其他线程可以进入同步代码块并调用`notify()`或`notifyAll()`方法。
    
3. **等待被唤醒**：调用`wait()`方法的线程会被放入对象的等待集（wait set）中，直到其他线程调用同一对象的`notify()`方法（随机唤醒一个等待的线程）或`notifyAll()`方法（唤醒所有等待的线程）。
    
4. **异常处理**：`wait()`方法会抛出`InterruptedException`，这表示等待中的线程可以被中断。因此，调用`wait()`方法时需要适当处理这个异常。



本地方法（Native Method）是Java中的一个概念，指的是用非Java语言（如C或C++）实现的方法。这些方法是Java Native Interface（JNI）的一部分，JNI是一个框架，允许Java代码与本地应用程序和库进行交互。本地方法允许Java程序执行以下操作：

1. **调用系统级或底层操作系统的功能**：有些操作系统级的功能，比如访问硬件、系统调用或特定平台的特性，可能在Java标准库中不直接可用。通过本地方法，Java程序可以使用这些平台特定的功能。
    
2. **性能优化**：对于某些性能敏感的操作，用本地代码（如C/C++）实现可能会比Java实现更高效。因此，本地方法可以用来优化性能。
    
3. **使用已有的库**：允许Java程序利用大量现有的、用其他语言编写的库，例如，一些高性能的数学计算库或系统级的库。


# 3.12
阻塞队列


#### 线程池
Executors  工具类
// 创建一个固定大小的线程池 
ExecutorService executor = Executors.newFixedThreadPool(5);
//一个可以根据需要创建新线程的线程池，但会在之前构建的线程可用时重用它们。
ExecutorService pool = Executors.newCachedThreadPool();
pool.submit(new MyRunnable());
pool.shutdown();

自定义线程池
```java
int corePoolSize = 5; // 核心线程数 
int maximumPoolSize = 10; // 最大线程数 
long keepAliveTime = 5000; // 非核心线程空闲等待新任务的最长时间 
TimeUnit unit = TimeUnit.MILLISECONDS; // keepAliveTime的时间单位
ArrayBlockingQueue<Runnable> workQueue = new ArrayBlockingQueue<>(100); // 工作队列 
ThreadPoolExecutor executor = new ThreadPoolExecutor( corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue);
```

Runtime.getRuntime().availableProcessors();//可用处理器数目

cup密集型运算  i/o密集型运算

**线程转储（Thread Dump）** 是一个非常有用的诊断工具，它提供了程序中所有线程在某一时刻的快照。这个快照包含了每个线程的当前状态、调用栈（Stack Trace）、持有的锁信息以及等待锁的信息等。通过分析线程转储，开发者可以识别死锁、线程饥饿、过度锁竞争等问题，从而帮助优化和调试多线程应用程序。
1. **JVM工具**：
    
    - **jstack**：使用Java自带的`jstack`命令行工具可以生成Java应用程序的线程转储。你需要提供运行Java应用程序的Java进程ID（PID）。命令格式为：`jstack -l <PID>`。
    - **VisualVM**：一个可视化工具，可以连接到Java进程并生成线程转储。它还提供了对Java应用程序的深入监控和分析。
2. **程序内触发**：
    
    - 通过Java代码，可以在运行时从应用程序内部请求线程转储。这可以通过调用`Thread.getAllStackTraces()`方法实现，该方法返回所有活动线程的堆栈跟踪。
3. **其他工具**：
    
    - **IDE**：像IntelliJ IDEA和Eclipse这样的集成开发环境通常提供了生成线程转储的功能。
    - **操作系统命令**：在某些操作系统中，如Linux，你可以使用如`kill -3 <PID>`的命令来生成Java进程的线程转储，并将其打印到标准错误流或日志文件中。


为什么要对线程间共享的变量用关键字`volatile`声明？这涉及到Java的内存模型。在Java虚拟机中，变量的值保存在主内存中，但是，当线程访问变量时，它会先获取一个副本，并保存在自己的工作内存中。如果线程修改了变量的值，虚拟机会在某个时刻把修改后的值回写到主内存，但是，这个时间是不确定的！
`volatile`关键字的目的是告诉虚拟机：
- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。
x86的架构下，JVM回写主内存的速度非常快，但是，换成ARM的架构，就会有显著的延迟。


原子性

JVM允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，就叫做可重入锁。
获取锁的时候，不但要判断是否是第一次获取，还要记录这是第几次获取。每获取一次锁，记录+1，每退出`synchronized`块，记录-1，减到0的时候，才会真正释放锁。
Java的`synchronized`锁是可重入锁

死锁
不同线程获取多个不同对象的锁可能导致死锁
线程获取锁的顺序要一致

因为`synchronized`是Java语言层面提供的语法，所以我们不需要考虑异常，而`ReentrantLock`是Java代码实现的锁，我们就必须先获取锁，然后在`finally`中正确释放锁
`ReentrantLock`是可重入锁，它和`synchronized`一样，一个线程可以多次获取同一个锁。
`ReentrantLock`可以尝试获取锁：
```
private final Lock lock = new ReentrantLock();
lock.lock();
lock.unlock();
lock.tryLock(1, TimeUnit.SECONDS)
```
`ReentrantLock`可以替代`synchronized`进行同步

`Condition`对象配合`ReentrantLock`来实现`wait`和`notify`的功能。
`Condition`可以替代`wait`和`notify`；
`Condition`对象必须从`Lock`对象获取。
- `await()`会释放当前锁，进入等待状态；
- `signal()`会唤醒某个等待线程；
- `signalAll()`会唤醒所有等待线程；
```java
Lock lock = new ReentrantLock(); 
Condition condition = lock.newCondition(); 

// 等待条件 lock.lock(); 
try { 
	while (<condition does not hold>) { 
		condition.await(); 
		} 
		// Proceed when condition holds 
	} finally { 
		lock.unlock(); 
	} 
		
// 改变条件并通知等待线程 
lock.lock(); 
try { 
	// Change the condition 
	condition.signalAll(); // Or condition.signal(); 
} finally { 
	lock.unlock(); 
	}
```

`await()`可以在等待指定时间后，如果还没有被其他线程通过`signal()`或`signalAll()`唤醒，可以自己醒来：
```java
if (condition.await(1, TimeUnit.SECOND)) {
    // 被其他线程唤醒
} else {
    // 指定时间内没有被其他线程唤醒
}
```


`ReadWriteLock`时，适用条件是同一个数据，有大量线程读取，但仅有少数线程修改。
- `ReadWriteLock`只允许一个线程写入；
- `ReadWriteLock`允许多个线程在没有写入时同时读取；
- `ReadWriteLock`适合读多写少的场景。
如果有线程正在读，写线程需要等待读线程释放锁后才能获取写锁，即读的过程中不允许写，这是一种悲观的读锁

`StampedLock`和`ReadWriteLock`相比，改进之处在于：读的过程中也允许获取写锁后写入！这样一来，我们读的数据就可能不一致，所以，需要一点额外的代码来判断读的过程中是否有写入，这种读锁是一种乐观锁
乐观锁的意思就是乐观地估计读的过程中大概率不会有写入，因此被称为乐观锁。反过来，悲观锁则是读的过程中拒绝有写入，也就是写入必须等待。显然乐观锁的并发效率更高，但一旦有小概率的写入导致读取的数据不一致，需要能检测出来，再读一遍就行。



CAS锁机制（无锁、自旋锁、乐观锁、轻量级锁）
常见并发工具类




网络编程

InetAddress()
DatagramSocket()
DatagramPacket()
Socket()
ServerSocket()
流


UUID


#### 反射
反射就是Reflection，Java的反射是指程序在运行期可以拿到一个对象的所有信息。


#### 动态代理
动态代理是Java反射机制的一个应用，它允许你在运行时动态地创建代理类和代理实例，用于拦截对特定接口方法的调用。这种机制广泛用于实现各种高级功能，如远程方法调用（RMI）、事务管理、日志记录、权限检查等，而无需修改原始类的代码。
动态代理主要通过`java.lang.reflect.Proxy`类和`java.lang.reflect.InvocationHandler`接口实现。`InvocationHandler`是一个接口，你需要实现这个接口来定义方法调用的拦截逻辑。当代理对象的方法被调用时，调用会被转发到`InvocationHandler`的`invoke`方法。
```java
interface Hello {
    void sayHello();
}

class HelloImpl implements Hello {
    public void sayHello() {
        System.out.println("Hello, world!");
    }
}

class DynamicProxyHandler implements InvocationHandler {
    private final Object target;

    public DynamicProxyHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method call: " + method.getName());
        Object result = method.invoke(target, args); // 调用原对象的方法
        System.out.println("After method call: " + method.getName());
        return result;
    }
}

public class ProxyTest {
    public static void main(String[] args) {
        Hello original = new HelloImpl();
        Hello proxyInstance = (Hello) Proxy.newProxyInstance(
                Hello.class.getClassLoader(),
                new Class<?>[] {Hello.class},
                new DynamicProxyHandler(original));

        proxyInstance.sayHello(); // 通过代理对象调用方法
    }
}

```
在这个例子中，`Hello`接口的`sayHello`方法通过动态代理`proxyInstance`被调用。这个调用会被`DynamicProxyHandler`的`invoke`方法拦截，允许你在调用原始方法前后执行额外的操作（在这个例子中是打印日志）

- **灵活性和可扩展性**：可以在不修改原始代码的情况下，动态添加或改变对象的行为。
- **解耦**：代理类的使用使得客户端代码和实际执行代码之间的耦合度降低。
- **代码复用**：通过代理类，可以在多个地方重用前置或后置逻辑，如安全检查、事务处理、日志记录等。


注解
注解（Annotations）是Java 5引入的一个特性，它提供了一种为代码添加元数据（即数据关于数据的数据）的方法。注解本身不会直接影响代码的操作，但可以被编译器、开发工具和运行时库检测和处理，从而在编译时、加载时或运行时改变程序的行为。注解可以被用于类、方法、变量、参数和Java包等。

- **内置注解**：Java提供了一些预定义的注解，如`@Override`、`@Deprecated`和`@SuppressWarnings`等。
- **元注解**：用于注解其他注解的注解。Java提供的元注解包括`@Target`、`@Retention`、`@Inherited`和`@Documented`等。
- **自定义注解**：用户可以定义自己的注解来满足特定的需求。

单元测试框架 Junit