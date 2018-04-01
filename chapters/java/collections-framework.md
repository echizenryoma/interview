## 集合框架

Java集合框架提供了数据持有对象的方式，提供了对数据集合的操作。Java集合框架位于`java.util`包下，主要有三个大类：`Collection`、`Map`接口以及对集合进行操作的工具类。

### Collection

![](/assets/java.util.Collection.svg)

| **Collection** | **线程安全性** | **初始容量** | **加载因子** | **扩容增量** | **有序** |
| :---: | :---: | :---: | :---: | :---: | :---: |
| `Vector` | **线程安全** | 10 | 1 | 100% | - |
| `Stack` | **线程安全** | 10 | 1 | 100% | - |
| `ArrayList` | 线程不安全 | 10 | 1 | 50%+1 | - |
| `LinkedList` | 线程不安全 | - | - | - | - |
| `HashSet` | 线程不安全 | 16 | 0.75 | 100% | 无序 |
| `TreeSet` | 线程不安全 | - | - | - | **有序** |
| `EnumSet` | 线程不安全 | - | - | - | **有序** |

* `ArrayList`：线程不同步。默认初始容量为10，当数组大小不足时增长率为当前长度的`50%`。
* `Vector`：**线程同步**。默认初始容量为10，当数组大小不足时增长率为当前长度的`100%`。它的同步是通过`Iterator`方法加`synchronized`实现的。
* `LinkedList`：线程不同步。**双端队列形式**。
* `Stack`：**线程同步**。继承自`Vector`，添加了几个方法来完成栈的功能。
* `Set`：Set是一种不包含重复元素的Collection，Set最多只有一个null元素。
* `HashSet`：线程不同步，内部使用`HashMap`进行数据存储，提供的方法基本都是调用`HashMap`的方法，所以两者本质是一样的。**集合元素可以为**`NULL`。
* `NavigableSet`：添加了搜索功能，可以对给定元素进行搜索：小于、小于等于、大于、大于等于，放回一个符合条件的最接近给定元素的 key。
* `TreeSet`：线程不同步，内部使用`NavigableMap`操作。默认元素“自然顺序”排列，可以通过`Comparator`改变排序。
* `EnumSet`：线程不同步。内部使用Enum数组实现，速度比`HashSet`快。**只能存储在构造函数传入的枚举类的枚举值**。

### Map

![](/assets/java.util.Map.svg)

* `HashMap`：线程不同步。根据`key`的`hashcode`进行存储，内部使用静态内部类`Node`的数组进行存储，默认初始大小为16，每次扩大一倍。当发生Hash冲突时，采用拉链法（链表）。**可以接受为null的键值（key）和值（value）**。JDK 1.8后：当单个桶中元素个数大于等于8时，链表实现改为红黑树实现；当元素个数小于6时，变回链表实现。由此来防止hashCode攻击。
* `LinkedHashMap`：**保存了记录的插入顺序**，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的. 也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢，因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。
* `TreeMap`：线程不同步，基于**红黑树**（Red-Black tree）的NavigableMap 实现，**能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。**
* `HashTable`：线程安全，HashMap的迭代器（Iterator）是`fail-fast`迭代器。**HashTable不能存储NULL的key和value。**

### 工具类

* `Collections`、`Arrays`：集合类的一个工具类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作。
* `Comparable`、`Comparator`：一般是用于对象的比较来实现排序，两者略有区别。

**答疑一**：比较`Comparator`和`Comparable`

**相同点**

1. 都是用于比较两个对象“顺序”的接口
2. 都可以使用`Collections.sort()`方法来对对象集合进行排序

**不同点**

1. `Comparable`位于`java.lang`包下，而`Comparator`则位于`java.util`包下
2. `Comparable`是在集合内部定义的方法实现的排序，`Comparator`是在集合外部实现的排序

**总结**

1. 可以说一个是自己完成比较，一个是外部程序实现比较的差别而已。
2. 用`Comparator`是策略模式（strategy design pattern），就是不改变对象自身，而用一个策略对象（strategy object）来改变它的行为。
3. 两种方式，各有各的特点：使用`Comparable`方式比较时，我们将比较的规则写入了比较的类型中，其特点是高内聚。但如果哪天这个规则需要修改，那么我们必须修改这个类型的源代码。如果使用`Comparator`方式比较，那么我们不需要修改比较的类，其特点是易维护，但需要自定义一个比较器，后续比较规则的修改，仅仅是改这个比较器中的代码即可。

**答疑二**：`hashCode()`和`equals()`方法的在`HashMap`中的作用。

`HashMap`使用key对象的`hashCode()`和`equals()`方法去决定key-value对的索引。当我们试着从`HashMap`中获取值的时候，这些方法也会被用到。如果这些方法没有被正确地实现，在这种情况下，两个不同key也许会产生相同的`hashCode()`和`equals()`输出，`HashMap`将会认为它们是相同的，然后覆盖它们，而非把它们存储到不同的地方。同样的，所有不允许存储重复数据的集合类都使用`hashCode()`和`equals()`去查找重复，所以正确实现它们非常重要。

equals()和hashCode()的实现应该遵循以下规则：
1. 如果`o1.equals(o2)`，那么`o1.hashCode() == o2.hashCode()`总是为`true`的
2. 如果`o1.hashCode() == o2.hashCode()`，并不意味着`o1.equals(o2)`会为`true`
