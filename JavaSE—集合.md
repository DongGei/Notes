#  集合（进阶）
## 一.集合类体系结构
1.Collection （单列）--->>List（可重复）--->>ArryList
																		--->>Linklist
									--->>set(不可重复)--->>HashSet
																	--->>TreeSet
2.Map 双列--->>HashAap

Collection,Map, List,set是接口，4个是实现类

后面的可以使用前面类里的方法

## 二.collection集合
### 1.概述及常用方法
![Screenshot_2021-08-07-21-29-44-495_tv.danmaku.bil](D:\2645981073\FileRecv\MobileFile\Screenshot_2021-08-07-21-29-44-495_tv.danmaku.bil.jpg)
Arrylist重写了tosrting

add ， remove（有重复删除第一个）， clear， contains(判断是否存在指定元素)， isEmpty，size
### 2.collection集合的遍历
迭代器：iteartor 集合专用遍历
用.iteartor方法获得迭代器  是用了Iitearto的实现类 
两个常用方法：

![Screenshot_2021-08-07-21-55-55-057_tv.danmaku.bil](D:\2645981073\FileRecv\MobileFile\Screenshot_2021-08-07-21-55-55-057_tv.danmaku.bil.jpg)

![image-20210807220642894](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210807220642894.png)

## 三.List
### 1.List集合概述和特点
+ 有序集合，精确控制插入位置，索引访问元素 
+ 与set不同，列表通常允许重复的元素
特点：有序 可重复 
+ 特有方法：增，删，改，查，增加了索引（不能越界）
索引遍历
### 2.并发修改异常
  迭代器 循环列表时 增删元素报错 可以通过for和.size方法
### 3. Listiteartor
+ 前后方向都可       previous 返回上一个元素
+ 迭代期间可以修改列表  使用列表迭代器的add方法  无并发修改异常
### 4.增强for语句
+ 内部原理是一个iteartor迭代器
### List集合子类特点
+ ArryList底层是数组 查找快 增删慢
+ LinkedList底层是链表  特有方法：增，查，删开头和末尾
## 四.set
+ 无重复元素    重复元素不会加进去
+ 无索引，不能普通for循环遍历
+ 对迭代顺序不作保证
### 1.HashSet
底层数据结构是哈希表 JDK8之前是数组+链表
+ 同一对象哈希值一样 hashcode
+ 可以通过方法重写改变哈希值
+ 储存时先比哈希值再比内容 都相同时不会储存
+ ***自定义的类需要重写hashcode和equals***（自动生成） 才能保证不会重复储存

### 2.LinkedHashSet集合概述和特点

+ 哈希表和链表实现的set接口
+ 元素不重复 ，**迭代有次序**（取和存顺序一样）
### 3.TreeSet集合概述和特点
+ 它的元素是排序的，可以是natural ordering（自然排序） ，或Comparator（比较器排序）这取决于所使用的构造方法
```java
TreeSet() //构造一个新的空树集，根据其元素的自然顺序进行排序。  
TreeSet(Comparator<? super E> comparator) //构造一个新的空树集，根据指定的比较器进行排序。  
```
+ 无索引，不能普通for遍历

+ 无重复 ，有排序

  #### 自然顺序Comparable的使用
  
  TreeSet无参构造
  
  自然顺序就是让元素所属的类实现Comparable<类名>接口
  
  然后，重写compareTo
  
  compareTo中return 1，就是添加的顺序排序 ；-1就是添加的倒序排序；0，只存第一个
  
  *** 比较器里return 0时会认为是相同元素，不会储存****
  
  ***要注意主要条件和次要条件都写***
  
  this是后面要加入的那个对象 s是传进来比较的前面那个对象
  
  ![image-20210808213003447](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210808213003447.png)
  
  #### 比较器comparator的使用
  
  ​	TreeSet带参构造
  
  在需要参数的地方 new一个comparator（匿名内部类）直接重写compare方法
  
  compare方法里s1是后面要加入的那个对象，s2是前面的那个对象

![Screenshot_2021-08-08-21-40-10-797_tv.danmaku.bil](D:\2645981073\FileRecv\MobileFile\Screenshot_2021-08-08-21-40-10-797_tv.danmaku.bil.jpg)

### 应用
+ 成绩排序（注意要注意主要条件和次要条件都写）
+ 不重复的随机数

## 五.泛型
+ 运行时异常提前到编译期
+ 避免了强制类型转换
### 1.泛型类

```java
public class GenericDemo {
    public static void main(String[] args) {
        Generic<String> g1=new Generic<String>();
        g1.setT("张三");
        System.out.println(g1.getT());

        Generic<Integer> g2=new Generic<Integer>();
        g2.setT(30);
        System.out.println(g2.getT());
    }
}
```

```Java
public class Generic<T> {
    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}
```

### 2.泛型方法
与重载类似
```Java
public class Generic {
    public<T> void show(T t){
        System.out.println(t);
    }
}
```

```Java
public static void main(String[] args) {
    Generic g1 = new Generic();
    g1.show("张三");
    g1.show("1324");
}
```
### 3.泛型接口

```Java
public static void main(String[] args) {
    GenreicImpl<String> g1 = new GenreicImpl<String>();
    g1.show("张三");
    GenreicImpl<Integer> g2 = new GenreicImpl<Integer>();
    g2.show(123465);
}
```

```Java
public class GenreicImpl<T> implements Generic<T> {

    @Override
    public void show(T t) {
        System.out.println(t);
    }
}
```

```Java
public interface Generic<T> {
    void show(T t);
}
```
### 4.泛型通配符

+ List<?>:表示元素类型未知的List,它的元素可以匹配任何的类型
+ 这种带通配符的List仅表示它是各种泛 型List的父类，并不能把元素添加到其中
+ 类型通配符 上限: <?extends 类型>  List<? extends Number>:它表示的类型是Number或者其子类型

+ 类型通配符下限: <?super 类型>  List<? super Number>:它表示的类型是Number或者其父类犁

### 5.可变参数
逻辑：每个参数都封装在数组里面了
范例: public static int sum（int...a){ }   就是一个数组 可以传进去n个int
public static int sum（int b,int...a){ } //可以这样使用  多个参数 可变参数放在最后  

+ Arrays工具类里的静态方法

static <T> List<T> asList(T... a) 返回由指定数组支持的**固定大小**的列表。  
```java
List<String> list =Arrays.asLIst("hello","world");
无法使用list.add和list.remove，因为大小已经固定，可以用set（1,"java"）;
```
+ List接口中有一个静态方法

static <E> List<E> of(E... elements) 返回包含任意数量元素的不可修改列表。  

```java
List<String> list = List.of("hello","world");
无法使用list.add和list.remove和list.set
```
+ set接口中有一个静态方法

static <E> Set<E> of(E... elements) 返回包含任意数量元素的不可修改集。  

```java
Set<String> set = Set.of("hello","world");
Set<String> set = Set.of("hello","world","world");<--错误  不可重复
```

## 六.Map

### 1.概述
+ Interface Map<K,V>， K：键的类型 V:值的类型   kay和value
+  地图不能包含重复的键; 每个键最多可以映射一个值。 
+  举例：学号对应姓名

```Java
public class MapDemo {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<String, String>();

        //添加方法
        map.put("001","a");
        map.put("002","b");
        map.put("003","c");
		map.put("003","d");<<——错误 会覆盖上一个003
        System.out.println(map);

    }
}
```
### 2.Map的基本功能
put,remove(返回V),clear,containsKey(是否包含指定键)，containsValue(是否包含指定值)，isEmpy,size,
### 3.Map的获取功能
```java
	Set<K> keySet() 返回所有键的集合
	V get(Object key) 返回指定键映射到的值，如果此映射不包含键的映射，则返回 null 。  
	Collection<V> values()  获取所有值的集合
```

### 4.Map集合的遍历
遍历直接sout也行，主要是了解方法

方式一：
```java
public class MapDemo {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<String, String>();//这里的String 可以是自己创建的对象

        //添加方法
        map.put("001","a");
        map.put("002","b");
        map.put("003","c");

        Set<String> keyset = map.keySet();

        for (String key : keyset){
            System.out.println(key +","+map.get(key));
        }
    }
}
```
方式二：
```java
public class MapDemo {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<String, String>();

        //添加方法
        map.put("001","a");
        map.put("002","b");
        map.put("003","c");

        Set<String> keyset = map.keySet();

        //获取所有键值对对象 的集合
        Set<Map.Entry<String, String>> entrySet = map.entrySet();
        for(Map.Entry<String, String> me :entrySet){
            String key = me.getKey();
            String value = me.getValue();
            System.out.println(key+value);
        }
    }
}
```