# PATL1-008 求整数段和（java）

https://pintia.cn/problem-sets/994805046380707840/problems/994805135224455168

给定两个整数A和B，输出从A到B的所有整数以及这些数的和。

### 输入格式：

输入在一行中给出2个整数A和B，其中−100≤A≤B≤100，其间以空格分隔。

### 输出格式：

首先顺序输出从A到B的所有整数，每5个数字占一行，每个数字占5个字符宽度，向右对齐。最后在一行中按`Sum = X`的格式输出全部数字的和`X`。

### 输入样例：

```in
-3 8
结尾无空行
```

### 输出样例：

```out
   -3   -2   -1    0    1
    2    3    4    5    6
    7    8
Sum = 30结尾无空行
```



**自己的思路**:

+ 看题目

A到B的和 先看一下**int 类型**可以存下sum 吗？

最多一共200个数 如果每一个都是100

是200 00 

java中int的取值范围为-2147483648到+-2147483648

可以。

+ 看输入格式 ：

<u>输入在一行中给出2个整数A和B，其中−100≤A≤B≤100，其间以空格分隔。</u>

可以用 如下格式 读：

```java
        int i1 = sc.nextInt();
        int i2 = sc.nextInt();
```

+ 输出格式

  首先顺序输出从A到B的所有整数，每5个数字占一行，每个数字占5个字符宽度，向右对齐。最后在一行中按`Sum = X`的格式输出全部数字的和`X`。

  可以通过String.format来解决

  System.out.printf("%5d",12); 
   OR
   System.out.format("%5d",12);
   OR
   String buf=String.format("%5d",12);

+ 然后尝试解题：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int i1 = sc.nextInt();
        int i2 = sc.nextInt();
        int n=1,sum=0;
        for (int j=i1;j<=i2;j++){
            sum+=j;
            System.out.printf("%5d",j);
            if (n==5&& j!=i2){
                //够五个数 且 不是最后一个数 输出换行
                System.out.print("\n");
                n=0;
            }
            n++;
        }
        System.out.print("\n");
        System.out.println("sum="+sum);
    }
}
```

+ 易错点：

多输出 一个换行

![image-20211105215635007](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20211105215635007.png)

