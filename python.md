# python入门

### 1. 变量

 （标识，一个变量由标识 类型 值 组成）

  ```python
a=10
print(id(a),type(a),a)
#140703780702160 <class 'int'> 10
  ```

### 2. == 与 is

==比较的是值

is 比较的id

用法刚好的Java equals相反

```python
a=10
b=10
print(a==b) #T
print(a is b) #T id一样

lst1=[10,20,30]
lst2=[10,20,30]
print(lst1 == lst2)#T
print(lst1 is lst2) #F id不一样
```

### 3. 布尔运算

+ and 并且 两个T为T

+ or  或者

+ not 非

+ in 与not in

 ```python
s="hello"
print("e" in s)
print("a" in s)
print("e" not in s)
print("a" not in s)
True
False
False
True
 ```

### 4. 运算符优先级
先后顺序如图所示
![请添加图片描述](https://gitee.com/dong2645981073/picture-summary/raw/master//image/a2b05670ac9f43b3bf506c4778f7683b.png)

### 5. intput

   intput  里面写提醒语句 返回的是str类型 需要的话 要类型转换

### 6.对象的布尔值

![请添加图片描述](https://gitee.com/dong2645981073/picture-summary/raw/master//image/ccda686f2f9340c6bea20d0cf415328c.png)


```py
print(bool(False))
print(bool(""))
print(bool(0))
print(bool(None))
print(bool([]))  # 空列表
print(bool(list()))  # 空列表
print(bool(())) #空元组
print(bool(tuple()))#空元组
print(bool({})) #空字典
print(bool(dict()))#空字典
print(bool(set())) #空集合
# 以上输出都是False
# 其他对象都为T
```



### 7. 程序的组织结构

+ 顺序结构

  程序重上到下 依次执行

+ 分支结构

```python
  a =100
  
  if a>10:
      print("ok")
  else:
      print("no")
```

```python
a = int(input("输入成绩判断等级"))
if a>0 and a<=100:
    if a>=80 :
        print("优秀")
    elif a>=60:
        print("及格")
    else:
        print("不及格")
else:
        print("输入错误")
```

+ 循环结构
  + while
  
  ```python
  a=1
  while a<10:
     	print(a,end="")
      a+=1
  ```
  
  + for-in 
  
  ```python
  for i in "python": #in 后面是迭代对象 eg：range，一个字符串
      print(i)#输出 p y t h o n
  
  for _ in  range(10): #循环体中不需要使用自定义变量，可以自定义变量写为_
      print("人生苦短 我用python")
  ```
  
  + break 语句
  
    和 C 中的类似，用于跳出最近的一级 for 或 while 循环。
  
  + continue语句
    用于结束当前循环,进入,下一次循环,通常与分支结构中的if一起使用
  
  + else 与 循环语句（for while）搭配使用
  
    循环正常执行 没有break 时，执行else
  
  ```python
  a= 0
  while a<3:
      pwd=int(input("输入密码"))
      if pwd ==888:
          print("输入正确")
          break;
      else:
          print("输入错误")
  
      a+=1
  else:
      print("三次输入错误，账号已锁定")
  ```
  
  条件表达式
  中间判断为T 值为左  判断为F 值为右
  ![请添加图片描述](https://gitee.com/dong2645981073/picture-summary/raw/master//image/62846f17fcce4630b598fb154cc77fdb.png)

### 8. pass语句

   用于搭建代码 

### 9.内置函数

前面不需要加前缀 直接使用的 

eg：intput print

+ range函数

返回值是一个迭代器对象
range类型的优点:不管range对象表示的整数序列有多长，所有range对象占用的内存空间都是相同的，因为仅仅需要存储start,stop和step，只有当用到range对象时，才会去计算序列中的相关元素
 in与not in 判断整数序列中是否存在(不存在)指定的整数

```python
r = range(10) #范围【0 10）默认从0开始 默认步长为1
print(r) #控制台输出range(0, 10)
print(list(r)) #--》列表 用于查看ranf对象中的整数序列

r =range(1,10)# 后面那个是开区间
print(list(r))
print(10 in r) #False

r =range(1,11,2)#没加步长前 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
                #加步长     [1, 3, 5, 7, 9]
print(list(r))
```
### 10.列表[]

相当于群体语言的数组 

**可以存不同的类型**

**可以存重复元素**

**根据动态分配和回收内存**

存储多个对象的引用

列表对象 按顺序有序储存

![image-20211009111016942](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009111016942.png)

#### 1)创建

  +  中括号

  + 内置函数lis= list（['hello','word',98]）

    lis[0] 和 lis[-3]就是'hello'

    从右到左 -1开始  

    从左到右 0 开始

#### 2)查找 获取

```python
lis= list(['hello','word',98,'hello'])
print(lis.index("hello")) # 输出0   返回左到右第一个
print(lis.index("hello",1,4))#3    在【1 4）里面找
```

不包括stop 步长为负数也是 

![image-20211009134717155](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009134717155.png)

  +  多个元素 切片操作

![image-20211009135157030](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009135157030.png)

**start默认是0   stop默认是最后一个  step默认是1**

+ 步长是负数

#### 3）添加

![image-20211009155627556](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009155627556.png)

![image-20211009160119517](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009160119517.png)

![image-20211009160311845](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009160311845.png)

![image-20211009160639936](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009160639936.png)

#### 4）删除

![image-20211009161142374](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009161142374.png)

#### 5）修改

![image-20211009161647327](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009161647327.png)

![image-20211009162512974](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009162512974.png)

#### 6）排序

一种是原列表上排序

一种是使用内置函数得出新的列表 原列表不变

![image-20211009162839178](D:\学习\截图\image-20211009162839178.png)

![image-20211009163027915](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009163027915.png)

#### 7)列表生成式

![image-20211009163343175](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009163343175.png)

i*i 是表示列表元素的表达式

i 是自定义变量

range是可迭代对象

### 11.字典{}

以键值对的方式存

+ hash（key）计算存储位置 

+ key 应是不可变对象 eg：int str
+ key 不能重复 Value可以重复  **重复会引起覆盖**
+ 元素无序
+ 较浪费内存
+ 与list 一样 自动动态分配空间

#### 1）创建

![image-20211009164722743](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009164722743.png)

空字典就是 d={}

#### 2)获取 判断

![image-20211009165413881](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009165413881.png)

区别：

没有查到时：

第一种报错

第二种输出None or 给的值

![image-20211009165830244](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009165830244.png)



#### 3）增加 删除 改

![image-20211009165956699](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009165956699.png)

![image-20211009170105793](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009170105793.png)

#### 4）视图

1. keys（）
2. values（）
3. items（）

![image-20211009170429550](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009170429550.png)

![image-20211009170612248](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009170612248.png)

#### 5）遍历

for  i  in scores

i 是Key    i是有顺序的

#### 6）字典生成式

![image-20211009175542969](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009175542969.png)

```python
itmes=["apple", "book","others"]
prices=[80,90,85]
d={ item:price    for item ,price in zip(itmes,prices) }
print(d)
#{'apple': 80, 'book': 90, 'others': 85}
```

```python
itmes=["apple", "book","others"]
prices=[80,90,85]
d={ item.upper():price    for item ,price in zip(itmes,prices) }
print(d)
#{'APPLE': 80, 'BOOK': 90, 'OTHERS': 85}
```

### 12.元组()

元组

+ Python内置的数据结构之一，是一个不可变序列

  **不变可变序**:字符串、元组
   不变可变序列 没有增、删,改的操作

  字符串可以s=s+“a”  但是id（地址） 是会变的

   **可变序列**:列表、字典，集合
   可变序列 可以对序列执行增、删、改操作，对象地址不发生更改

#### 1).创建 

第一种创建方式，使用() 小括号可以省略

第二种创建方式，使用内置函数tuple()

![image-20211009202810600](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009202810600.png)

元组内如果只包含一个元组的元素 需要使用**逗号**和小括号

```python
t=( 10 ,) #如果不写  ， 将会识别为str
```

#### 2）获取

+ 索引

  t[0]  t[1] t[2]

+ for..in 循环

![image-20211009204249601](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009204249601.png)

### 13.集合

集合
Python语言提供的内置数据结构
与列表、字典一样都属于可变类型的序列·**集合是没有value的字典**

可以类比数学里的集合

+ 不可重复
+ 可变序列

#### 1）创建

+ s={2,3,4,5,5,6,7,7}
+ s=set()
+ s=set(range(6))
+ s2=set ([1,2,4,5,5,5,6,6]) #会自动去掉重复元素

+ 空集合 必须set()  {}默认是空字典

#### 2）增加

+ add 添加一个元素
+ update 添加多个元素（加元组，列表.......）

#### 3）删除

+ remove() 有则删除 没有异常
+ discard  不抛异常
+ pop() **随机**删除一个
+ clear 清空

#### 4）集合之间的关系

 								4种

![image-20211009212247222](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009212247222.png)

![image-20211009212632256](D:\学习\截图\image-20211009212632256.png)

#### 5）数学操作

不影响原来的集合

+ &
+ |
+ -
+ ^

![image-20211009213142977](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009213142977.png)

![image-20211009213732128](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211009213732128.png)

![20211009213709](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20211009213709.png)

#### 6)生成式

s={ i*i for i in range(10)}

### 14.函数

```python
def calc(a ,b):'''创建'''
    c=a-b
    return c;


calc1 = calc(10, 20)'''调用'''
calc2 =calc(b=50,a=60)
print(calc1)
print(calc2)


```

**在函数调用过程中，进行参数的传递**
如果是不可变对象，在函数体的修改不会影响实参的值 arg1的修改为100，不会影响n1的值
如果是可变对象，在函数体的的修改会影响到实参的值arg2的修改，append(10)，会影响到n2的值
**函数的返回值**
(1)如果函数没有返回值【函数执行完毕之后，不需要给调用处提供数据】return可以省略不写

(2)函数的返回值，如果是1个，直接返回类型
(3)函数的返回值，如果是多个，返回的结果为元组

![image-20211010204408434](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211010204408434.png)

![image-20211010205239791](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211010205239791.png)

可以查看print

```
print(self, *args, sep=' ', end='\n', file=None)
```

end 默认换行

+ 个数可变的位置参数  

```python
def p(*args):
    print(args)
    #print(args[0]) #作为一个元组来使用
    
p(20,20,30)
p(20)
lst=[10,20,30]
p(*lst) #在函数调用时，将列表中的每个元素都转换为位置实参传入
#(20, 20, 30)
#(20,)
#(10, 20, 30)

#eg：
print(self, *args, sep=' ', end='\n', file=None)
位置参数
```

+ 个数可变的关键字参数

```python
def fun(** args):
    print(args)
    
fun(a=10)
fun(a=20,b=30,c=60)
dic={'a':111,'b':222,'c':333}
fun(**dic)#在函数调用时，将字典中的键值对都转换为关键字实参传入
#{'a': 10}
#{'a': 20, 'b': 30, 'c': 60}
#{'a': 111, 'b': 222, 'c': 333}
```

**以上两个个数可变的参数一个函数只能定义一个**

两个**同时使用时**，要求 个数可变的位置参数 在  个数可变的关键字参数**之前**

```python
```

![image-20211012174942005](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012174942005.png)

如果不加* 三种方法都可以

### 15.类

#### 1)创建语法

  ```python
  class Student:
      pass
  print(id(Student)) #已经开辟空间了
  print(type(Student))
  print(Student)
  '''
  2450633353136
  <class 'type'>
  <class '__main__.Student'>
  '''
  ```

 定义在类里的函数就是 

![image-20211012184044381](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012184044381.png)

#### 2)实例调用

  创建的实例对象中 会有一个**类指针**指向类

  + 对象名.方法名
  + 类名.方法名（类的对象）#就是定义处的self

  ![image-20211012184459754](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012184459754.png)

+ 类属性

  ![image-20211012190110090](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012190110090.png)

+ 类方法

  ![image-20211012190203684](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012190203684.png)

类方法和静态调用：

```python
Studet.cm() #cls是不需要传入的
Studet.method()
```
#### 3)动态绑定属性与方法
python是动态语言，创建对象后可以动态绑定属性与方法
+ 动态绑定属性
	eg：对象添加一个属性

```python
class Student:
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def eat(self):
        print(self.name+'在吃饭')

stu1=Student('张三',20)
stu1.gender='女'
print(stu1.gender,stu1.name,stu1.age)
```

![image-20211012192240057](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012192240057.png)

+ 动态绑定方法

```python
class Student:
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def eat(self):
        print(self.name+'在吃饭')
def show():
    print("定义在类外的 称为方法")
stu1.s=show()
stu1.s
```

#### 4) 封装，继承，多态

+ python没有修饰符来标识属性的**私有** 可以使用两个 _ 

​      只有内部可以访问，外部不能访问

![image-20211012194625453](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211012194625453.png)

（靠程序员的自觉）

+ 默认继承object类

  ```python
  class Animal(object):
  ```

  后面的括号写父类

  子类和父类有同名方法时 子类覆盖父类

  + 判断一个变量是否是某个类型及父继承链

```python
	isinstance(c, Dog) #与Java类似
```

+ 开闭原则

  + 对扩展开放

    可以新增子类继承父类，覆盖父类的方法

  + 对修改封闭

    不可以修改 **依赖父类的类型**（父类类型做传入参数）的方法

### 16.模块

.py结尾的文件 就可以作为模块

  包中有模块

+ 代码复用
+ 分工合作开发
+ 避免命名重复

![image-20211016111325841](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211016111325841.png)

+ 常用内置模块
  + sys
  + time
  + os
  + calendar（日期相关各种函数标准库）
  + urllib（读取服务器的数据标准库）
  + json
  + re
  + math
  + decimal
  + logging
+ 导入第三方包

### 17.包

Python中的包

+ 包是一个分层次的目录结构，它将一组功能相近的模块组织在一起

+  作用

  代码规范
  避免模块名称冲突·

+ 包与目录的区别
  包含__init_ _.py文件的目录称为包·目录里通常不包含_init__.py文件
  心

+ 包的导入
  import包名.模块名 as 别名

+ 使用from..import 可以导入包，模块，函数，变量

### 内置函数

#### 1.isinstance

有时候可以代替type() 

判断它是不是 某个类的对象

判断是不是 多个类其中一个的对象

```python
isinstance('a', str)

isinstance([1, 2, 3], (list, tuple))
True
isinstance((1, 2, 3), (list, tuple))
True
```

#### 2.dir（）

获取一个对象的所有属性和方法



```python
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
#__add__  命名的是有特殊用途的 比如__len__方法返回长度
```

```python
>>> hasattr(obj, 'x') # 有属性(方法和变量)'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```

