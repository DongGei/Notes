# java 函数式编程

## 1.为什么学

1. 容于并发编程。可以使用并发 处理大数量集合，自动使用多线程去处理，而自己不用写线程有关代码。
2. 可以简化代码，解决嵌套。

## 2.Lambda表达式

Lambda是JDK8中一个语法糖。他可以对某些匿名内部类的写法进行简化。它是函数式编程思想的一个重要体现。让我们不用关注是什么对象。而是更关注我们对数据进行了什么操作。

~~~~java
(参数列表)->{代码}  小括号 和 大括号 都还可以省略
~~~~

> 语法糖：
>
> 简单的说，语法糖就是一种便捷写法。就相当于汉语里的成语。

> tip:
>
> ctrl+p  查看传参提示

```java
public class LambdaDemo_01 {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            public void run() {
     System.out.println("_____新线程中run方法____");
            }
        }).start();
    }

}
```

上面这个Thread构造需要一个实现了Runnable接口的对象

我们使用了匿名内部类，并且重写了run方法，然后调用了start()。

什么情况可以使用Lambda表达式：

+ 上面这种 这个匿名内部类 是一个接口的匿名内部类，并且只需要实现（重写）一个抽象方法（可能接口中其他的是默认方法，就一个是抽象的  ）

  **Lambda表达式  不关注类名，方法名，关注的是方法的参数和方法体**

  ```java
  public static void main(String[] args) {
      new Thread(  () -> {System.out.println("_____新线程中run方法____")
      ;}).start();
  }
  ```

  (参数列表)->{代码}  完成！

下面是几个循序渐进的案例：

```java
public static void main(String[] args) {
    int num = calculateNum(new IntBinaryOperator() {
        @Override
        public int applyAsInt(int left, int right) {
            return left + right;
        }
    });
    System.out.println(num);
}

public static int calculateNum(IntBinaryOperator operator) {
    int a = 10;
    int b = 20;
    return operator.applyAsInt(a, b);
}
```

```java
public static void main(String[] args) {
    int num = calculateNum((a,b)->{return a+b ;});
    System.out.println(num);
}


public static int calculateNum(IntBinaryOperator operator) {
    int a = 10;
    int b = 20;
    return operator.applyAsInt(a, b);
}
public static void main(String[] args) {
        int num = calculateNum((a,b)-> a+b);
        System.out.println(num);
    }
```

> tip: 使用idea alt+回车 可以转换匿名内部类到lambda表达式，也可以lambda表达式转换到匿名内部类。
>
> 灰常方便

```java
public class LambdaDemo_03 {
    public static void main(String[] args) {
        printNum(new IntPredicate() {
            @Override
            public boolean test(int value) {
                return value%2 == 0;
            }
        });
    }
    
    public static void main(String[] args) {
        printNum((a)->  a%2 == 0);
    }
    
    public static void printNum(IntPredicate predicate){
        int[] arr = {1,2,3,4,5,6,7,8,9,10};
        for (int i : arr) {
            if(predicate.test(i)){
                System.out.println(i);
            }
        }
    }

}
```

```java
public class LambdaDemo_04 {
    public static void main(String[] args) {
        Integer integer = typeConver(new Function<String, Integer>() {
            @Override
            public Integer apply(String s) {
                return Integer.valueOf(s);
            }
        });

        System.out.println(integer);
    }
    
    public static void main(String[] args) {
        //泛型都省略了
        Integer integer = typeConver(s -> Integer.valueOf(s));

        System.out.println(integer);
    }
    
    public static <R> R typeConver(Function<String,R> function){
        String str = "1235";
        R result = function.apply(str);
        return result;
    }

}
```

```java
public class LambdaDemo_04 {
    public static void main(String[] args) {
        String st = typeConver(new Function<String, String>() {
            @Override
            public String apply(String s) {
                return s+"aaa";
            }
        });

        System.out.println(st);
    }
    
    public static <R> R typeConver(Function<String,R> function){
        String str = "1235";
        R result = function.apply(str);
        return result;
    }

}

   public static void main(String[] args) {
       //泛型都可以省略了 
       String st = typeConver(s -> s+"aaa");
        System.out.println(st);
   }
```

心中目标 就是写一个 实现了哪个方法的，匿名内部类 

### 省略规则

```jav
(参数列表)->{代码} 
```

+ 小括号里的 ```参数类型```  可省略
+ 方法体里只有一句代码时 ```大括号```,```return``` 和 ```最后的分号``` 可省略
+ 方法只有 一个参数的时候  ```小括号``` 可省略

## 3.Stream流

​	Java8的Stream使用的是函数式编程模式，如同它的名字一样，它可以被用来对集合或数组进行链状流式的操作。可以更方便的让我们对集合或数组操作。

### 入门案例：

```java
public class StreamDemo {
    public static void main(String[] args) {
       // System.out.println(getAuthors());
        List<Author> authors = getAuthors();
        authors.stream()
                .distinct()
                .filter(author -> author.getAge()<18)
                .forEach(author -> System.out.println(author.getName())); //必须要有终结操作
    }
```

**idea 对debug对stream流的直观体现：**

![image-20220527225905697](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20220527225905697.png)

### 创建流

单列集合： `集合对象.stream()`

数组：`Arrays.stream(数组) `或者使用`Stream.of(数组)`来创建

双列集合：转换成单列集合后再创建

```        
        Map<String,Integer> map = new HashMap<>();
        map.put("蜡笔小新",19);
        map.put("黑子",17);
        map.put("日向翔阳",16);

        Stream<Map.Entry<String, Integer>> stream = map.entrySet().stream();
```

### 中间操作

+ filter 	流中的元素进行条件过滤



+ map 	可以把对流中的元素进行计算或转换。

  这个Map是映射的意思,映射成其他的元素

```java
public void t1(){
    getAuthors().stream()
            .map(new Function<Author, String>() {
                @Override
                public String apply(Author author) {
                    return author.getName();
                 }
            }).forEach(name-> System.out.println(name));
}
```

```java
public void t1(){
    getAuthors().stream()
            .map(new Function<Author, Author>() {
                @Override
                public Author apply(Author author) {
                   author.setAge(author.getAge()+10);
                    return author;
                }
            }).forEach(author-> System.out.println(author.getAge()));
}
```



+ distinct 	可以去除流中的重复元素。

distinct方法是依赖Object的equals方法来判断是否是相同对象的。所以需要注意重写equals方法



+ sorted  	可以对流中的元素进行排序。

  对象需要实现comparable接口 或者 传一个Comparator的匿名内部类

```java
//author对象实现了omparable接口
@Override
    public int compareTo(Author o) {
        //返回负整数、零或正整数，因为此对象小于、等于或大于指定对象
        return o.getAge()-this.getAge();
    }
public void t2(){
    getAuthors().stream()
            .sorted()
            .forEach(author -> System.out.println(author.getName()+author.getAge()));
}
 @Test
    public void t2(){
        getAuthors().stream()
                .sorted((o1, o2) -> o1.getAge()-o2.getAge()) //o1 应该是当前项目
                .forEach(author -> System.out.println(author.getName()+author.getAge()));
    }
```

+ limit	

可以设置流的最大长度，超出的部分将被抛弃。  limit 获取前N个元素

```java
public void t3(){
    getAuthors().stream()
            //年龄最小的前2个
            .sorted((o1, o2) -> o1.getAge() - o2.getAge())
            .limit(2)
            .forEach(author -> System.out.println(author.getName()+author.getAge()));
}
```

+ skip	 	跳过流中的前n个元素，返回剩下的元素

```java
@Test
public void t4(){
    getAuthors().stream()
            //跳过前两个
            .skip(2)
            .forEach(author -> System.out.println(author.getName()+author.getAge()));
}
```

+ flatMap

  map只能把一个对象转换成另一个对象来作为流中的元素。而flatMap可以把一个对象转换成多个对象作为流中的元素。

  ```java
  public void t5(){
      getAuthors().stream()
              .flatMap(new Function<Author, Stream<?>>() {
                  @Override
                  public Stream<?> apply(Author author) {
                      return author.getBooks().stream();
                  }
              })
              .forEach(book -> System.out.println(book));
  }
   public void t5(){
          getAuthors().stream()
                  .flatMap(author -> author.getBooks().stream())
                  .forEach(book -> System.out.println(book));
      }
  ```


### 终结操作

##### forEach

​	对流中的元素进行遍历操作，我们通过传入的参数去指定对遍历到的元素进行什么具体操作。

##### count

​	可以用来获取当前流中元素的个数。

```java
@Test
public void t7(){
    List<Author> authors = getAuthors();
    long count = authors.stream()
            .distinct()
            .count();
    System.out.println(count);
}
```

##### max&min

​	可以用来或者流中的最值。

```java
public void t7(){
    // 分别获取这些作家的所出书籍的最高分和最低分并打印。
    List<Author> authors = getAuthors();
    Optional<Integer> max = authors.stream()
            .flatMap(author -> author.getBooks().stream())
            .map(book -> book.getScore())
            .max((o1, o2) -> o1 - o2);
    Integer integer = max.get();
    System.out.println(integer);
    
}
```

##### collect

​	把当前流转换成一个集合。

```java
List<Author> authors = getAuthors();
List<String> list = authors.stream()
        .map(author -> author.getName())
        .collect(Collectors.toList()); //使用 java.util.stream.Collectors; 这个工具类
System.out.println(list);
```

// set 一模一样

map 需要注意的是 key 数据一定不能重复（下面用了distinct 其实是对author对象euaqls 去重，也没实际是对name去重）

```Java
List<Author> authors = getAuthors();
Map<String, List<Book>> map = authors.stream()
        .distinct()
        .collect(Collectors.toMap(author -> author.getName(), author -> author.getBooks()));

System.out.println(map);
```

##### 查找与匹配

###### anyMatch

​	可以用来判断是否有任意符合匹配条件的元素，结果为boolean类型。

###### allMatch

​	可以用来判断是否都符合匹配条件，结果为boolean类型。如果都符合结果为true，否则结果为false。

```java
public void t10() {
    List<Author> authors = getAuthors();
    boolean b = authors.stream()
            .anyMatch(author -> author.getAge() > 18);
    System.out.println(b);
}

 public void t10() {
        List<Author> authors = getAuthors();
        boolean b = authors.stream()
                .allMatch(author -> author.getAge() > 18);
        System.out.println(b);
    }
```

###### noneMatch

​	可以判断流中的元素是否都不符合匹配条件。如果都不符合结果为true，否则结果为false

```java
//        判断作家是否都没有超过100岁的。
        List<Author> authors = getAuthors();

        boolean b = authors.stream()
                .noneMatch(author -> author.getAge() > 100);

        System.out.println(b); //true 就是都没有
```



###### findAny

	获取流中的任意一个元素。该方法没有办法保证获取的一定是流中的第一个元素。



例子：

	获取任意一个年龄大于18的作家，如果存在就输出他的名字

~~~~java
 
        List<Author> authors = getAuthors();
        Optional<Author> optionalAuthor = authors.stream()
                .filter(author -> author.getAge()>18)
                .findAny();

        optionalAuthor.ifPresent(author -> System.out.println(author.getName()));
~~~~

###### findFirst

	获取流中的第一个元素。

例子：

	获取一个年龄最小的作家，并输出他的姓名。

~~~~java
//        获取一个年龄最小的作家，并输出他的姓名。
        List<Author> authors = getAuthors();
        Optional<Author> first = authors.stream()
                .sorted((o1, o2) -> o1.getAge() - o2.getAge())
                .findFirst();

        first.ifPresent(author -> System.out.println(author.getName()));
~~~~

它俩区别的注意点~~

> 返回的元素是不确定的，对于同一个列表多次调用findAny()有可能会返回不同的值。使用findAny()是为了更高效的性能。如果是数据较少，串行地情况下，一般会返回第一个结果，如果是并行的情况，那就不能确保是第一个。
>
> 所以 你测试的时候可能发现 他俩结果一样  可能是因为数据量小

##### reduce归并

内部操作：![image-20220531202521234](https://s2.loli.net/2022/05/31/LdGtbhPmONWaAUE.png)

发现 返回的值  是和元素的类型一样的  所以一般先进行map操作 连起来叫 map-reduce操作

```java
@Test
    public void t12() {
//        年龄和 
        List<Author> authors = getAuthors();
        Integer sum = authors.stream()
                .map(author -> author.getAge())
                .reduce(0, new BinaryOperator<Integer>() {
                    @Override
                    public Integer apply(Integer re, Integer elem) {
                        return re + elem;
                    }
                });
        System.out.println(sum);
    }
```

min 和 max 操作其实就是reduce

	使用reduce求所有作者中年龄的最大值

~~~~java
//        使用reduce求所有作者中年龄的最大值
        List<Author> authors = getAuthors();
        Integer max = authors.stream()
                .map(author -> author.getAge())
                .reduce(Integer.MIN_VALUE, (result, element) -> result < element ? element : result);

        System.out.println(max);
~~~~

	使用reduce求所有作者中年龄的最小值

~~~~java
//        使用reduce求所有作者中年龄的最小值
        List<Author> authors = getAuthors();
        Integer min = authors.stream()
                .map(author -> author.getAge())
                .reduce(Integer.MAX_VALUE, (result, element) -> result > element ? element : result);
        System.out.println(min);
~~~~

### 注意事项

- 惰性求值（如果没有终结操作，没有中间操作是不会得到执行的）
- 流是一次性的。不是数据结构，不会保存数据。（一旦一个流对象经过一个终结操作后。这个流就不能再被使用）
- 不会影响原数据（我们在流中可以多数据做很多处理。但是正常情况下是不会影响原来集合中的元素的。这往往也是我们期望的）

特殊：影响原数据 

```java
List<Author> authors = getAuthors();
authors.stream().forEach(author -> System.out.println("原数据"+author.getAge()));

authors.stream()
        .map(new Function<Author, Author>() {
            @Override
            public Author apply(Author author) {
                 author.setAge(author.getAge() + 10);
                return author;
            }
        })
        .forEach(author -> System.out.println(author.getAge()));


authors.stream().forEach(author -> System.out.println("原数据"+author.getAge()));
//原数据都改变了
原数据33
原数据15
原数据14
原数据14
43
25
24
24
原数据43
原数据25
原数据24
原数据24

Process finished with exit code 0

```

```java
@Test
    public void t13() {
        List<Author> authors = getAuthors();
        authors.stream().forEach(author -> System.out.println("原数据"+author.getAge()));

        authors.stream()
                .map( author -> author.getAge()+10)
                .forEach(age -> System.out.println(age));


        authors.stream().forEach(author -> System.out.println("原数据"+author.getAge()));


    }
原数据33
原数据15
原数据14
原数据14
43
25
24
24
原数据33
原数据15
原数据14
原数据14

Process finished with exit code 0

```

也就是不会影响集合内元素项的类型、个数、顺序。但由于值传递，可能改变元素项的属性

## 4. Optional

在JDK8中引入了Optional,养成使用Optional的习惯后你可以写出更优雅的代码来避免空指针异常。(比如mapper 查某个数据)

>  在实际开发中我们的数据很多是从数据库获取的。Mybatis从3.5版本可以也已经支持Optional了。我们可以直接把dao方法的返回值类型定义成Optional类型，MyBastis会自己把数据封装成Optional对象返回。封装的过程也不需要我们自己操作。

并且在很多函数式编程相关的API中也都用到了Optional。

### 使用

#### 创建

Optional就好像是包装类，可以把我们的具体数据封装Optional对象内部。然后我们去使用Optional中封装好的方法操作封装进去的数据就可以非常优雅的避免空指针异常。

```java
@Test
    public void t1() {
        Optional<Author> authorOptional = getAuthorOptional();
        authorOptional.ifPresent(author -> System.out.println(author)); //如果不是null （ifPresent）才去消费
    }

    private Optional<Author> getAuthorOptional() {
        Author author = new Author(3L, "易", 14, "是这个世界在限制他的思维", null);
        return Optional.ofNullable(author);//ofNullable创建
    }
```

​	



+ 不经常用的两个创建方法：

如果你**确定一个对象不是空**的则可以使用**Optional**的**静态方法of**来把数据封装成Optional对象。

~~~~java
        Author author = new Author();
        Optional<Author> authorOptional = Optional.of(author);
~~~~

​	但是一定要注意，如果使用of的时候传入的参数必须不为null。（尝试下传入null会出现什么结果）



​	如果一个方法的返回值类型是Optional类型。而如果我们经判断发现某次计算得到的返回值为null，这个时候就需要把null封装成Optional对象返回。这时则可以使用**Optional**的**静态方法empty**来进行封装。

~~~~java
		Optional.empty()
~~~~

+ 更多使用 更方便的应该是```ofNullable```

  

####  安全消费值

​	我们获取到一个Optional对象后肯定需要对其中的数据进行使用。这时候我们可以使用其**ifPresent**方法对来消费其中的值。

​	这个方法会判断其内封装的数据是否为空，不为空时才会执行具体的消费代码。这样使用起来就更加安全了。

​	例如,以下写法就优雅的避免了空指针异常。

~~~~java
        Optional<Author> authorOptional = Optional.ofNullable(getAuthor());

        authorOptional.ifPresent(author -> System.out.println(author.getName()));
~~~~



####  获取值

​	如果我们想获取值自己进行处理可以使用get方法获取，但是不推荐。因为当Optional内部的数据为空的时候会出现异常。

```java
@Test
public void t2() {
    Optional<Author> authorOptional = getAuthorOptional();
    Author author = authorOptional.get();  //如果下面 return Optional.ofNullable(null);  那么这一行会报错
    System.out.println(author);
    //Author(id=3, name=易, age=14, intro=是这个世界在限制他的思维, books=null)
}

private Optional<Author> getAuthorOptional() {
    Author author = new Author(3L, "易", 14, "是这个世界在限制他的思维", null);
    return Optional.ofNullable(author);
}
```



#### 安全获取值

​	如果我们期望安全的获取值。我们不推荐使用get方法，而是使用Optional提供的以下方法。

* orElseGet 

  获取数据并且设置数据为空时的默认值。如果数据不为空就能获取到该数据。如果为空则根据你传入的参数来创建对象作为默认值返回。

  ~~~~java
    public void t2() {
          Optional<Author> authorOptional = getAuthorOptional();
          Author author = authorOptional.orElseGet(() -> new Author());//否则获取
          System.out.println(author);
          //Author(id=null, name=null, age=null, intro=null, books=null)
      }
  
      private Optional<Author> getAuthorOptional() {
          Author author = new Author(3L, "易", 14, "是这个世界在限制他的思维", null);
          return Optional.ofNullable(null);
      }
  ~~~~

+ orElseThrow

     获取数据，如果数据不为空就能获取到该数据。如果为空则根据你传入的参数来创建异常抛出。

    ```java
    @Test
        public void t3() {
            Optional<Author> authorOptional = getAuthorOptional();
            Author author = null;
            try {
                author = authorOptional.orElseThrow(() -> new RuntimeException("数据为空"));
            } catch (Throwable e) {
                e.printStackTrace();
            }
            System.out.println(author);
        }
    
        private Optional<Author> getAuthorOptional() {
            Author author = new Author(3L, "易", 14, "是这个世界在限制他的思维", null);
            return Optional.ofNullable(null);
        }
    ```


#### 过滤

​	我们可以使用filter方法对数据进行过滤。如果原本是有数据的，但是不符合判断，也会变成一个无数据的Optional对象。

~~~~java
        Optional<Author> authorOptional = Optional.ofNullable(getAuthor());
        authorOptional.filter(author -> author.getAge()>100).ifPresent(author -> System.out.println(author.getName()));

~~~~

#### 判断

我们可以使用isPresent方法进行是否存在数据的判断。如果为空返回值为false,如果不为空，返回值为true。但是这种方式并不能体现Optional的好处，**更推荐使用ifPresent方法**。

```java
      Optional<Author> authorOptional = Optional.ofNullable(getAuthor());
        if (authorOptional.isPresent()) {
            System.out.println(authorOptional.get().getName());
        }
```

#### 数据转换

​	Optional还提供了map可以让我们的对数据进行转换，并且转换得到的数据也还是被Optional包装好的，保证了我们的使用安全。

例如我们想获取作家的书籍集合。

```java
public void t4() {
        Optional<Author> authorOptional = getAuthorOptional();
        Optional<List<Book>> books = authorOptional.map(author -> author.getBooks());
        books.ifPresent(books1 -> System.out.println(books1));
    //[Book(id=1, name=刀的两侧是光明与黑暗, category=哲学,爱情, score=88, intro=用一把刀划分了爱恨), Book(id=2, name=一个人不能死在同一把刀下, category=个人成长,爱情, score=99, intro=讲述如何从失败中明悟真理)]
    }

    private Optional<Author> getAuthorOptional() {
        Author author = new Author(3L, "易", 14, "是这个世界在限制他的思维", null);
        List<Book> books1 = new ArrayList<>();
        books1.add(new Book(1L, "刀的两侧是光明与黑暗", "哲学,爱情", 88, "用一把刀划分了爱恨"));
        books1.add(new Book(2L, "一个人不能死在同一把刀下", "个人成长,爱情", 99, "讲述如何从失败中明悟真理"));
        author.setBooks(books1);
        return Optional.ofNullable(author);
    }
```

## 5. 函数式接口

### 概述

​	**只有一个抽象方法**的接口我们称之为函数接口。

​	JDK的函数式接口都加上了**@FunctionalInterface** 注解进行标识。但是无论是否加上该注解只要接口中只有一个抽象方法，都是函数式接口。

 ### 常见函数式接口	

- ​	Consumer 消费接口

  根据其中抽象方法的参数列表和返回值类型知道，我们可以在方法中对传入的参数进行消费。

  ![image-20211028145622163](https://s2.loli.net/2022/06/01/BE1V9joOS8t52zw.png)

- ​	Function 计算转换接口

  根据其中抽象方法的参数列表和返回值类型知道，我们可以在方法中对传入的参数计算或转换，把结果返回

  ![image-20211028145707862](https://s2.loli.net/2022/06/01/KxAiqEe3Mwt1fU9.png)

- ​	Predicate 判断接口

  根据其中抽象方法的参数列表和返回值类型知道，我们可以在方法中对传入的参数条件判断，返回判断结果

  ![image-20211028145818743](https://s2.loli.net/2022/06/01/tTHwCE69uQcNiO4.png)

- ​	Supplier 生产型接口

  根据其中抽象方法的参数列表和返回值类型知道，我们可以在方法中创建对象，把创建好的对象返回

![image-20211028145843368](https://s2.loli.net/2022/06/01/t541pK6lTm9VXBo.png)

### 常用的默认方法

以public interface Predicate<T> 为例

使用了接口里面默认的实现了的方法 达到一种组合使用的效果

每一个接口 中有许多默认方法，方便的是我们自定义函数式编程方法时用的



![image-20220601223437649](https://s2.loli.net/2022/06/01/CL1pt9oHweFgZVu.png)

- and

  我们在使用Predicate接口时候可能需要进行判断条件的拼接。而and方法相当于是使用&&来拼接两个判断条件
  
  ```java
  public void t14() {
          List<Author> authors = getAuthors();
          authors.stream().filter(new Predicate<Author>() {
              @Override
              public boolean test(Author author) {
                  return author.getAge() > 18;
              }
          }.and(new Predicate<Author>() {
              @Override
              public boolean test(Author author) {
                  return author.getName().length()>1;
              }
          })).forEach(author -> System.out.println(author));
      }
  //上面那种 主要是说明用法  下面也可以实现效果
   public void t14() {
          List<Author> authors = getAuthors();
          authors.stream()
                  .filter((author -> author.getAge() > 18 && author.getName().length() > 1))
                  .forEach(author -> System.out.println(author));
      }
  ```

+ or 

  我们在使用Predicate接口时候可能需要进行判断条件的拼接。而or方法相当于是使用||来拼接两个判断条件。

  ```java
  @Test
      public void t14() {
          List<Author> authors = getAuthors();
          authors.stream().filter(new Predicate<Author>() {
              @Override
              public boolean test(Author author) {
                  return author.getAge() > 18;
              }
          }.or(new Predicate<Author>() {
              @Override
              public boolean test(Author author) {
                  return author.getName().length()>1;
              }
          })).forEach(author -> System.out.println(author));
      }
  ```


- negate

    Predicate接口中的方法。negate方法相当于是在判断添加前面加了个! 表示取反

    例如：

    打印作家中年龄不大于17的作家。

    ~~~~java
    //        打印作家中年龄不大于17的作家。
            List<Author> authors = getAuthors();
            authors.stream()
                    .filter(new Predicate<Author>() {
                        @Override
                        public boolean test(Author author) {
                            return author.getAge()>17;
                        }
                    }.negate()).forEach(author -> System.out.println(author.getAge()));
    ~~~~

  主要应用场景 是 在自己定义一下方法  比如：

  ```java
  public static void main(String[] args) {
      printNum(a->a%2 == 0,a->a!=2);
  }
  public static void printNum(IntPredicate predicate,IntPredicate predicate2){
      int[] arr = {1,2,3,4,5,6,7,8,9,10};
      for (int i : arr) {
          if(predicate.and(predicate2).test(i)){
              System.out.println(i);
          }
      }
  }
  ```

## 6. 方法引用

我们在使**用lambda时**，如果方法体中只有一个方法的调用的话（包括构造方法）,我们可以用方法引用进一步简化代码。

也是一个语法糖

写多了 也就熟练了 可以使用快捷键先生成 ~~面向快捷键开发~~

###  基本格式

​	类名或者对象名::方法名

### 1.引用类的静态方法

##### 格式

~~~~java
类名::方法名
~~~~

##### 使用前提

​	如果我们在重写方法的时候，方法体中**只有一行代码**，并且这行代码是**调用了某个类的静态方法**，并且我们把要重写的**抽象方法中所有的参数都按照顺序传入了这个静态方法中**，这个时候我们就可以引用类的静态方法。

例如：

如下代码就可以用方法引用进行简化

~~~~java
        List<Author> authors = getAuthors();

        Stream<Author> authorStream = authors.stream();
        
        authorStream.map(author -> author.getAge())
                .map(age->String.valueOf(age));
~~~~

注意，如果我们所重写的方法是没有参数的，调用的方法也是没有参数的也相当于符合以上规则。

优化后如下：

~~~~java
        List<Author> authors = getAuthors();

        Stream<Author> authorStream = authors.stream();

        authorStream.map(author -> author.getAge())
                .map(String::valueOf);
~~~~

### 2.引用对象的实例方法

##### 格式

~~~~java
对象名::方法名
~~~~

##### 使用前提

​	如果我们在重写方法的时候，方法体中**只有一行代码**，并且这行代码是**调用了某个对象的成员方法**，并且我们把要重写的**抽象方法中所有的参数都按照顺序传入了这个成员方法中**，这个时候我们就可以引用对象的实例方法

例如：

~~~~java
        List<Author> authors = getAuthors();

        Stream<Author> authorStream = authors.stream();
        StringBuilder sb = new StringBuilder();
        authorStream.map(author -> author.getName()) //这里就不能使用 不满足：抽象方法中所有的参数都按照顺序传入了这个成员方法中
                .forEach(name->sb.append(name));
~~~~

优化后：

~~~~java
        List<Author> authors = getAuthors();

        Stream<Author> authorStream = authors.stream();
        StringBuilder sb = new StringBuilder();
        authorStream.map(author -> author.getName())
                .forEach(sb::append);
~~~~



### 3.引用类的实例方法

##### 格式

~~~~java
类名::方法名
~~~~

##### 使用前提

​	如果我们在重写方法的时候，方法体中**只有一行代码**，并且这行代码是**调用了第一个参数的成员方法**，并且我们把要**重写的抽象方法中剩余的所有的参数都按照顺序传入了这个成员方法中**，这个时候我们就可以引用类的实例方法。

```java
public class MethDemo {
    interface UseString{
        String use(String str,int start,int length);
    }

    public static String subAuthorName(String str, UseString useString){
        int start = 0;
        int length = 1;
        return useString.use(str,start,length);
    }

    public static void main(String[] args) {
        subAuthorName("三更草堂", new UseString() {
            @Override
            public String use(String str, int start, int length) {
                return str.substring(start,length);
            }
        });

        subAuthorName("三更草堂", String::substring);

    }

}
```

```java
books.ifPresent(System.out::println);
```

### 4.构造器引用

如果方法体中的一行代码是构造器的话就可以使用构造器引用。

##### 格式

~~~~java
类名::new
~~~~

##### 使用前提

​	如果我们在重写方法的时候，方法体中**只有一行代码**，并且这行代码是**调用了某个类的构造方法**，并且我们把**要重写的抽象方法中的所有的参数都按照顺序传入了这个构造方法中**，这个时候我们就可以引用构造器。

例如：

~~~~java
        List<Author> authors = getAuthors();
        authors.stream()
                .map(author -> author.getName())
                .map(name->new StringBuilder(name))////
                .map(sb->sb.append("-AAA").toString())
                .forEach(str-> System.out.println(str));
~~~~

优化后：

~~~~java
        List<Author> authors = getAuthors();
        authors.stream()
                .map(author -> author.getName())
                .map(StringBuilder::new)////
                .map(sb->sb.append("-AAA").toString())
                .forEach(str-> System.out.println(str));
~~~~

lambda是为了精简匿名内部类各种调用分不清的情况。而方法引用是为了对lambda进一步精简。所以可读性比较差

## 基本数据类型优化

​	我们之前用到的很多Stream的方法由于都使用了泛型。所以涉及到的参数和返回值都是引用数据类型。

​	即使我们操作的是整数小数，但是实际用的都是他们的包装类。JDK5中引入的自动装箱和自动拆箱让我们在使用对应的包装类时就好像使用基本数据类型一样方便。但是你一定要知道装箱和拆箱肯定是要消耗时间的。虽然这个时间消耗很下。但是在大量的数据不断的重复装箱拆箱的时候，你就不能无视这个时间损耗了。

​	所以为了让我们能够对这部分的时间消耗进行优化。Stream还提供了很多专门针对基本数据类型的方法。

​	例如：mapToInt,mapToLong,mapToDouble,flatMapToInt,flatMapToDouble等。

```java
 private static void t6_高级用法() {

        List<Author> authors = getAuthors();
        authors.stream()
                .map(author -> author.getAge())
                .map(age -> age + 10)
                .filter(age->age>18)
                .map(age->age+2)
                .forEach(System.out::println);

        authors.stream()
                .mapToInt(author -> author.getAge())
                .map(age -> age + 10)
                .filter(age->age>18)
                .map(age->age+2)
                .forEach(System.out::println);
    }
```

```java
@Test
public void t6_高级用法() {
    List<Author> authors = getAuthors();
    authors.stream()
            .map(new Function<Author, Integer>() {
                @Override
                public Integer apply(Author author) {
                    return author.getAge();
                }
            })
            .map(new Function<Integer, Integer>() {
                @Override
                public Integer apply(Integer age) {
                    return age + 10; //拆箱装箱
                }
            })
            .filter(new Predicate<Integer>() {
                @Override
                public boolean test(Integer age) {
                    return age > 18;//拆箱装箱
                }
            })
            .map(age->age+2)//拆箱装箱
            .forEach(System.out::println);
    
    //对上面原来的内容 转化成了 匿名内部类  可以看出mapToInt后其中都是int类型解决了重复拆箱装箱
          authors.stream()
                .mapToInt(new ToIntFunction<Author>() {
                    @Override
                    public int applyAsInt(Author author) {
                        return author.getAge();
                    }
                })
                .map(new IntUnaryOperator() {
                    @Override
                    public int applyAsInt(int age) {
                        return age + 10;
                    }
                })
                .filter(new IntPredicate() {
                    @Override
                    public boolean test(int age) {
                        return age > 18;
                    }
                })
                .map(new IntUnaryOperator() {
                    @Override
                    public int applyAsInt(int age) {
                        return age + 2;
                    }
                })
                .forEach(new IntConsumer() {
                    @Override
                    public void accept(int x) {
                        System.out.println(x);
                    }
                });
}
```

## 并行流

当流中有大量元素时，我们可以使用并行流去提高操作的效率。其实并行流就是把任务分配给多个线程去完全。如果我们自己去用代码实现的话其实会非常的复杂，并且要求你对并发编程有足够的理解和认识。而如果我们使用Stream的话，我们只需要修改一个方法的调用就可以使用并行流来帮我们实现，从而提高效率。

​	parallel方法可以把串行流转换成并行流。

原形式：

```java
public void t16() {
        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Integer reduce = stream.filter(integer -> integer > 5)
                .reduce(0, (re, elem) -> re + elem);
        System.out.println(reduce);

    }
```

并行流：

```java
public void t16() {
    Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    Integer reduce = stream
            .parallel()
            .filter(integer -> integer > 5)
            .reduce(0, (re, elem) -> re + elem);
    System.out.println(reduce);

}
```

测试说明：

```java
public void t16() {
    Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    Integer reduce = stream
            .parallel()
            //peek只应该用于debug作用，不应该有修改操作产生副作用
            .peek(num -> System.out.println(num+Thread.currentThread().getName()))
            .filter(integer -> integer > 5)
            .reduce(0, (re, elem) -> re + elem);
    System.out.println(reduce);
    
    //9ForkJoinPool.commonPool-worker-5
    //3ForkJoinPool.commonPool-worker-19
    //2ForkJoinPool.commonPool-worker-27
    //1ForkJoinPool.commonPool-worker-31
    //5ForkJoinPool.commonPool-worker-17
    //10ForkJoinPool.commonPool-worker-13
    //6ForkJoinPool.commonPool-worker-23
    //8ForkJoinPool.commonPool-worker-9
    //4ForkJoinPool.commonPool-worker-3
    //7main
    //40
}
```

