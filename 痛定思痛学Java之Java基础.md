####  JAVA自我复习与进修

+ **JAVA中堆和栈的区别**

  JAVA在程序运行时，在内存中划分5片空间进行数据的存储。分别是：1：寄存器。2：本地方法区。3：方法区。4：栈。5：堆。其中，堆与栈的区别如下：

  + 各司其职

    **基本数据类型、局部变量**都是存放在栈内存中的，用完就消失。

    而new创建的**实例化对象及数组**，是存放在堆内存中的，用完之后靠垃圾回收机制不定期自动消除。

  + 独有还是共享

    栈内存归属于单个线程，每个线程都会有一个栈内存，其存储的变量只能在其所属线程中可见，即栈内存可以理解成线程的私有内存。
    而堆内存中的对象对所有线程可见。堆内存中的对象可以被所有线程访问。

  + 空间大小

    栈的内存要远远小于堆的内存

  + 异常错误

    如果栈内存没有可用的空间存储方法调用和局部变量，JVM会抛出java.lang.StackOverFlowError。
    而如果是堆内存没有可用的空间存储生成的对象，JVM会抛出java.lang.OutOfMemoryError。

++++

+ **八大基本类型**

  byte、short、int、long、float、double、boolean、char

  基本类型都有对应的包装类型。

+++

+ **string**

  + string被声明为final，因此不可被继承。

    在 Java 8 中，String 内部使用**char 数组**存储数据。

    在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。

  + 不可变的好处

    + 可以缓存hash值：因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。
    + String Pool 的需要：如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。
    + 安全性：String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。
    + 线程安全：String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

+++

+ **string、string buffer、string builder**

  + 可变性：string不可变，其他可变
  + 线程安全
    + string不可变，线程安全
    + string builder不是线程安全的
    + string buffer线程安全，内部使用synchronized进行同步。

+ **string pool**

  字符串常量池，具体区别是两字符串同时调用另一个字符串的**intern**方法时，都是**引用到**那个字符串，而分别**new**则不是引用到那个字符串而是**重新创建**两个新的字符串。

+++

+ **Java集合**

  集合类存放于java.util包中。

  集合类存放的都是对象的引用，而非对象本身，出于表达上的便利，我们称集合中的对象就是指集合中对象的引用（reference)。 

  集合类型主要有3种：set(集）、list(列表）和map(映射)。

  其中，list主要分为三类：ArrayList， LinkedList和Vector

  **集合详解**：

  **1、Iterator**：迭代器，他是Java集合的顶层接口（不包括map系列的集合，Map接口是map系列集合的顶层接口）

  + Object next()：返回迭代器刚越过的元素的引用，返回值是Object，需要强制转换成自己需要的类型。
  + boolean hasNext()：判断容器内是否还有可供访问的元素。
  + void remove()：删除迭代器刚越过的元素。

  所以除了map系列的集合，我们都能通过迭代器来对集合中的元素进行遍历。

  **2、Collection**：List接口和Set接口的父接口

  **3、List**：有序、可以重复的集合。由于List接口是继承于Collection接口，所以基本的方法如上所示。List接口的三个典型实现：

  + List list1 = new ArrayList();

    底层数据结构是数组，查询快，增删慢；线程不安全，效率高。 

  + List list2 = new Vector();

    底层数据机构是数组，查询快，增删慢；线程安全，效率低，几乎已经淘汰了这个集合。

  + List list3 = new LinkedList();

    底层数据结构是链表，查询慢，增删快；线程不安全，效率高。

  **4、Set**：典型实现HashSet()是一个无序，不可重复的集合

  + 1、Set hashSet = new HashSet();

    HashSet 不能保证元素的顺序，不可重复，是线程安全的；集合元素可以为NULL。

    底层其实是一个数组，存在的意义是加快查询速度。我们知道在一般的数组中，元素在数组中的索引位置是随机的，元素的取值和元素的位置之间存在不确定性关系，因此，在数组中查找特定的值时，需要把查找值和一系列的元素进行比较，此时的查询效率依赖于查找过程中比较的次数。而HashSet集合底层数组的索引和值之间有一个确定的关系：index = hash(value)，那么只需要调用这个公式，就能快速的找到元素或者索引。

    对于HashSet：如果两个对象通过equals()方法返回true，这两个对象的hashCode值也应该相同。

    当向HashSet集合存入一个元时，HashSet会先调用该对象的hashCode()方法来得到该对象的hashCode值，然后根据hashCode值决定该对象在HashSet中的存储位置。

    + 如果hashCode值不同，直接把该元素存储到hashCode()指定的位置。
    + 如果hashCode值相同，那么会继续判断该元素和集合对象的equals作比较。
      + hashCode相同，equals为true，则视为同一个对象，不保存在HashSet()中。
      + hashCode相同，equals为false，则存储在之前对象同槽位的链表上，这非常麻烦，我们应该约束这种情况，即保证：如果两个对象通过equals()方法返回true，这两个对象的hashCode值也应该相同。

  + 2、Set linkedHashSet = new LinkedHashSet();

    不可重复，有序

    因为底层采用链表和哈希表的算法，链表保证元素的添加顺序，哈希表保证元素的唯一性。

  + 3、Set TreeSet = new TreeSet();

    TreeSet：有序，不可重复，底层使用红黑树算法，擅长于范围查找。

    如果使用TreeSet()无参数的构造器创建一个TreeSet对象，则要求放入其中的元素的类必须实现Comparable接口，所以其中不能放入null元素。 

    必须放入同样类的对象，(默认会进行排序)否则可能会发生类型转换异常，我们可以使用泛型来进行限制。

  + **以上三个Set接口的实现类比较**：

    + 相同点

      都不允许元素重复。

      都不是线程安全的类，解决办法：Set set = Collection.synchronizedSet(set对象)。

    + 不同点

      HashSet：不保证元素的添加顺序，底层采用哈希表算法，查询效率高。判断两个元素是否相等，equals()方法返回true，hashCode值相等。即要求存入HashSet中的元素要覆盖equals()方法和hashCode()方法。

      linkedHashSet：HashSet的子类，底层采用了哈希表算法以及链表算法，既保证了元素的添加顺序，也保证了查询效率。但整体性能要低于HashSet

      TreeSet：不保证元素的添加顺序，但是会对集合中的元素进行排序。底层采用红黑树算法。

  **5、Map**：key-value的键值对，key不允许重复，value可以。

  + 严格来说Map并不是一个集合，而是两个集合之间的映射关系。

  + 这两个集合每一条数据通过映射关系，我们可以看成是一条数据。即Entry(key,value)。Map可以看成是由多个Entry组成。

  + 因为Map集合既没有实现与Collection接口，也没有实现Iterable接口，所以不能对Map集合进行for-each遍历。

  + map实现类如图：

    ![here](http://pohpioxm9.bkt.clouddn.com/map%E5%AE%9E%E7%8E%B0%E7%B1%BB.png)

  **6、Map和Set的关系**

  + 都有几个类型的集合。HashMap和HashSet都采用哈希表算法；TreeMap和TreeSet都采用红黑树算法；LinkedHashMap和LinkedHashSet都采用哈希表算法和红黑树算法。
  + 分析set的底层源码，可以看到，Set集合就是由Map集合的key构成。

+++

+ **运算**
  + 参数传递：JAVA的参数传递使用**值传递**而**不是引用传递**。
  + float和double：Java不能隐式执行向下转型，因为这会使得精度降低。*不能往精度降低的方向隐式转型。*
  + switch：从Java 7开始，可以在switch条件判断语句中使用String对象。但是switch不支持long。

+++

+ **继承**

+ **抽象类与接口**

  + 抽象类：抽象类和抽象方法都使用abstract关键字进行声明。抽象类一般会包含抽象方法，抽象方法一定位于抽象类中。

    抽象类和普通类的最大区别是，抽象类不能被实例化，需要继承抽象类才能实例化其子类。

  + 接口：接口是抽象类的延伸，在Java 8以前，他可以看成是一个完全抽象的类，也就是说他不能有任何的方法实现。从Java 8开始，接口也可以拥有默认的方法实现，这是因为不支持默认方法的接口维护成本太高了。在Java 8之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。

    接口的成员（字段+方法）默认都是public的，并且不允许定义为private或者protected。

    接口的字段默认都是static和final的。

  + 比较

    + 从设计层面上看，抽象类提供了一种IS-A关系，那么就必须满足里氏替换原则，即子类对象必须能够替换掉所有父类对象。而接口更像是一种LIEK-A关系，他只是提供一种方法实现契约，并不要求接口和实现接口的类具有IS-A关系。
    + 从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。
    + 接口的字段只能是static和final的，而抽象类的字段没有这种限制。
    + 接口的成员之能是public的，而抽象类的成员可以有多种访问权限。

  + 使用选择

    使用接口：

    + 需要让不相关的类都实现一个方法，例如不相关的类都可以实现Compreable接口中的compareTo()方法；
    + 需要使用多重继承

    使用抽象类：

    + 需要在几个相关的类中共享代码。
    + 需要能控制继承来的成员的访问权限，而不是都为public。
    + 需要继承非静态和非常量字段

    在很多情况下，接口优于抽象类，因为接口没有抽象类严格的类层次要求，可以灵活地为一个类添加行为。并且从Java 8开始，接口也可以有默认的方法实现，使得修改接口的成本也变得很低。

+++

+ **Super**

  访问父类的构造函数：可以使用super()函数访问父类的构造函数，从而委托父类完成一些初始化的工作。

  访问父类的成员：如果子类重写了父类的某个方法，可以通过super关键字来引用父类的方法实现。

+ **重写与重载**

  **重写**(**Override**)：存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

  为了满足里氏替换原则，重写有以下两个限制：

  + 子类方法的访问权限必须大于等于父类方法。
  + 子类方法的返回类型必须是父类方法返回类型或为其子类型。

  使用@Override注解，可以让编译器帮忙检查是否满足上述两个条件。

  **重载**(**Overload**)：存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。应该注意的是，返回值不同，其它都相同不算是重载。

  涉及到重写时，方法调用的优先级为：

  - this.show(O)
  - super.show(O)
  - this.show((super)O)
  - super.show((super)O)

+++

+ **Object通用方法**

  **概览**

  ```java
  public native int hashCode()
  
  public boolean equals(Object obj)
  
  protected native Object clone() throws CloneNotSupportedException
  
  public String toString()
  
  public final native Class<?> getClass()
  
  protected void finalize() throws Throwable {}
  
  public final native void notify()
  
  public final native void notifyAll()
  
  public final native void wait(long timeout) throws InterruptedException
  
  public final void wait(long timeout, int nanos) throws InterruptedException
  
  public final void wait() throws InterruptedException
  ```

  +++

  **equals()**

  + 等价关系

    + 自反性：x.equals(x);	//true
    + 对称性：x.equals(y) == y.equals(x);    // true
    + 传递性：if (x.equals(y) && y.equals(z))   x.equals(z);      // true;
    + 一致性：多次调用 equals() 方法结果不变；

    对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false

  + 等价与相等

    对于基本类型，==判断两个值是否相等，基本类型没有equals()方法。

    对于引用类型，==判断两个变量是否引用一个对象，而equals()判断引用的对象是否等价。

    ```java
    Integer x = new Integer(1);
    Integer y = new Integer(1);
    System.out.println(x.equals(y)); // true
    System.out.println(x == y);      // false
    ```

  + 实现

    - 检查是否为同一个对象的引用，如果是直接返回 true；
    - 检查是否是同一个类型，如果不是，直接返回 false；
    - 将 Object 对象进行转型；
    - 判断每个关键域是否相等。

  +++

  **hashCode()**

  hashCode是jdk根据对象的地址或者字符串或者数字算出来的int类型的数值。支持此方法是为了提高哈希表（例如 java.util.Hashtable 提供的哈希表）的性能。

  hashCode() 返回散列值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价。

  在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。

  +++

  **toString()**

  默认返回 ToStringExample@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示。

  +++

  **clone()**

  + cloneable

    clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。

    应该注意的是，clone() 方法并不是 Cloneable 接口的方法，而是 Object 的一个 protected 方法。Cloneable 接口只是规定，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，就会抛出 CloneNotSupportedException。

  + 浅拷贝

    拷贝对象和原始对象的引用类型引用同一个对象。

    将原对象或原数组的引用直接赋给新对象，新数组，新对象／数组只是原对象的一个引用

  + 深拷贝

    拷贝对象和原始对象的引用类型引用不同对象。

    创建一个新的对象和数组，将原对象的各项属性的“值”（数组的所有元素）拷贝过来，是“值”而不是“引用”

  + 为什么要使用深拷贝

    我们希望在改变新的数组（对象）的时候，不改变原数组（对象）

  + 怎么检验深浅拷贝

    改变任意一个新对象/数组中的属性/元素,     都不改变原对象/数组

  + clone() 的替代方案

    使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。

  +++

  **关键字**

  + final

    + 数据：声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。
      + 对于基本类型，final 使数值不变；
      + 对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。
    + 方法：声明方法不能被子类重写。private 方法隐式地被指定为 final，如果在子类中定义的方法和基类中的一个 private 方法签名相同，此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。
    + 类：声明类不允许被继承

  + static

    + 静态变量

      + 又称为类变量，也就是说这个变量是属于类的，类的所有实例都共享静态变量，可以直接通过类名来访问它。静态变量在内存中只存在一份。
      + 实例变量：每创建一个实例就会产生一个实例变量，他与该实例同生共死。

    + 静态方法

      静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法。

  