## JAVA面向过程

#### 随记：for each循环

**2022.03.22**

增强型for循环的基本格式为:

```java
for(variable : collection)
{
    statement
}
```

variable 是定义的一个for循环内部的一个局部变量，用于暂存集合内的每一个元素；

collection这一个集合表达式必须是一个数组或者一个实现了Iterable接口的类对象；

```java
for(Book book : list )//example : 对于存储 Book 对象的 (ArrayList< Book >) list:
{
	statement 
}
```

#### 随记：数组深拷贝

**2022.03.22**

在java中，实现数组深拷贝的函数为

```java
Array.copyOf(数组名称,数组长度);
```

我们可以通过这个方法来实现数组长度的延长，也就是类似于c中的realloc

```java
int[] arr=Array.copyOf(arr,2*arr.length);//example
```

#### 随记：不规则数组

**2022.03.22**

在java中，实际上没有多维数组这个概念，所谓的多维数组实际上是数组嵌套数组

例如，假设我们要存储一个下三角矩阵:

```
1
1 1
1 2 1
1 3 3 1
1 4 6 4 1
1 5 10 10 5 1
```

我们可以如下实现:

```java
int[][] odds=new int[MAX+1][];
for(int i=0;i<=MAX;i++)
{
	odds[i]=new int[i+1];
}
```

我们可以通过如下方式遍历这个数组:

```java
for(int i=0;i<odds.length;i++)
{
	for(int j=0;j<odds[i].length;j++)
	{
		statement;
	}
}
```

#### 随记:大数

**2022.03.22**

java中有两个类BigInteger和BigDecimal，分别支持任意精度的整数与浮点数运算

使用static函数ValueOf可以把普通的数转换成大数:

```java
BigInteger num=BigInteger.ValueOf(100);
BigInteger num=BigInteger.ValueOf("1001313510530310351");//较大的数传入String对象
```

Tips:大数类没用重载运算符，运算需要调用相应的方法

#### 随记:带标记的break语句

**2022.03.22**

```java
mylabel:
while(...)
{
	for(...)
	{
		if(...)
		{
			break mylabel;
		}
	}
}
//break后将在这里继续执行
//---------------------------

mylabel2:
{
	if(...) break mylabel2;
}
//break后将在这里继续执行
```

带标记的break语句将跳出整个标记标记的语句块

#### 随记：数组初始化

**2022.04.13 **
所谓初始化：就是为数组中的数组元素分配内存空间，并为每个数组元素赋值
        数组初始化
              动态初始化
              静态初始化

```java
  动态初始化：
     初始化时只指定数组长度，由系统为数组分配初始值
     格式：数据类型[] 变量名 = new 数据类型[数组长度];
     范例：int[] arr = new int[3];

     注意：数组长度从0开始，并不是1
```

#### 随记：三目运算符的性质

所谓双目数值提升，在双目运算符java的开发环境下可以简单的理解为类型转换的问题。 

1. 如果定义了数据类型的变量和未定义数据类型的变量参与双目运算符的后双目运算，那么返回的结果就是范围大（精度高）的类型。
2. 如果两个定义了数据类型的变量参与双目运算符的后双目运算，那么返回的结果就是范围大（精度高）的类型。
3. 如果直接进行数值的比较，则自动转型为范围大（精度高）的类型。

#### 随记：Java运算的注意点

1. 不可把非布尔值当做布尔值在逻辑表达式中使用。当和string 使用时，布尔值可以自动转换为文本。
2. 短路：一旦可以明确确定逻辑表达式的值，余下的部分可以不用计算。
3. 对char、byte或short进行的每个算术运算都会产生一个**int结果**，必须显式地将其转换回原始类型，才能重新分配给该类型。
4. 通常，表达式中最大的数据类型是决定结果大小的数据类型。
5. 若表达式以一个string起头，那么后续所有运算对象都必须是字串。

#### 随记：在java中左值的特点

**2022.05.09**

在java中,**左值必须是一个明确的、已经命名的变量**

在我本地的ide上

```java
++a=1;
a++=1;
//上述两个表达式无法通过编译器检查
int[] c=new int[100];
c[a++]=20;
c[++a]=100;
//上述两个表达式不会报错
```

#### 随记：一些错题

**1.main 方法可以保证java程序独立运行，一个Java程序的主方法是main方法，一个Java程序不一定要有main方法。**

批注：Java程序不一定要有main方法，可以通过static块运行

**2.i为int型，存在 i + 1 < i的数字。**

i=Integer.MAX_VALUE时成立

**3.使用lambda表达式创建匿名内部类将不生成class文件。**

**lambda表达式与匿名内部类的区别**

**所需类型不同**

- 匿名内部类：可以是接口，也可以是抽象类，还可以是具体类
- Lambda表达式：只能是接口

**使用限制不同**

- 如果接口中有且仅有一个抽象方法，可以使用Lambda表达式，也可以使用匿名内部类
- 如果接口中多于一个抽象方法，则只能使用匿名内部类，而不能使用Lambda表达式

**实现原理不同**

- 匿名内部类：在编译后，会生成一个单独的class字节码文件
- Lambda表达式：在编译后没有一个单独的class字节码文件，对用的字节码会在运行的适合动态生成

**4.short s1 = 1; s1 = s1 + 1;不可以编译通过。**

批注：s1+1自动转为int,但是int不能自动转回short，必须使用强制类型转换

**5.String.substring(a,b)方法前闭后开。**

**6.增强型for循环的一些坑点**

for增强型循环只能用于遍历数组或集合中的元素

for增强型循环不能直接用于遍历Map集合中的元素

相当于传统for循环来说，for增强型循环简化了书写

for增强型循环遍历过程中不能对集合或数组元素进行修改

**7.Objectoutputstream的writeobject方法是将对象写入内存**

**8.classpath是javac编译器专用的一个环境变量，作用是告诉Java执行环境，在哪些目录下可以找到您所要执行的Java程序所需要的类或者包。**

**9.线程安全的类:Vector、StringBuffer、stack、hashtable、enumeration**

**10.泛型提供了编译时的类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。**

**11.二维数组第二维可以为空**

例子: int [] [] A =new int [3] [];

也可以int [] [] A={},int [] []A={{}},int [] [] A=new {{},{}}

**12.类变量指被static修饰的字段,实例变量指在类中定义的非静态字段**

实例变量和类变量可以不初始化，会有默认值

局部变量不能是静态的



## Java面向对象

### Chapter1:对象与类

#### 1.1 面向对象程序设计概述

##### 1.1.1类

略

##### 1.1.2 对象

略

##### 1.1.3 识别类

略

##### 1.1.4 类之间的关系

在类之间，最常见的关系有:

* 依赖("uses-a")

  所谓的依赖，实质上就是一个类的方法使用或者操纵另一个类的对象

* 聚合("has-a")

  一个类的的对象包含另一个类的对象

* 继承("is-a")

  一个类具备另一个类的特征,但还有一些其他功能

#### 1.2使用预定义类

##### 1.2.1对象与对象变量

```java
Date birthday=new Date();//example
Scanner input=new Scanner(System.in);
```

如上所示，在java中要想使用对象，首先需要构造对象，在括号中我们根据构造函数的参数列表来传入参数

<font color="red">**Attention:**</font>

<font color="red">对象变量并没有实际包含一个对象，他只是引用一个对象</font>

<font color="red">在java中，任何对象变量的值都是对存储在另外一个地方(堆)的某个对象的引用。new操作符的返回值也是一个引用。</font>

##### 1.2.2 Java类库中的LocalDate类

略

##### 1.2.3 更改器方法与访问器方法

在java中，只访问对象不修改对象的方法被称为访问器方法，如果一个方法要改变对象实例值，则被称为更改器方法。

在java中，两者在语法上没有明显的界限，一般来说，对于私有字段，程序会为其提供一个get()访问器方法和一个set()更改器方法。

#### 1.3 用户自定义类

##### 1.3.1 Employee 类

略

##### 1.3.2 多个源文件的使用

略

##### 1.3.3剖析Employee类

略

##### 1.3.4 从构造器开始

1. 构造器与类同名
2. 每个类可以有一个以上的构造器
3. 构造器可以有0个、1个或者多个参数
4. 构造器没有返回值
5. 构造器总是伴随着new 操作符一起调用

##### 1.3.5用var声明局部变量

```java
//在 java 10 之后，如果可以从变量的初始值推导出他们的类型，那么可以使用var关键字声明局部变量
Employee harry = new Emplyee("Harry Hacker",50000,1989,10,1);
var harry = new Emplyee("Harry Hacker",50000,1989,10,1);
```

Tips:

1. var 关键字只能用于<font color="red">方法中的局部变量</font>。参数和字段的类型必须声明。
2. 我们一般不会对数值类型使用var。

##### 1.3.6 使用null引用

一个对象变量包含着一个对象的引用或者包含一个特殊值null，以表示没有该对象变量没用引用任何对象。

<b><font color="red">如果对null对象变量应用一个方法，会抛出NullPointerException异常!!!</font></b>

如果我们不想在调用构造器时传入null，我们可以应用下述两种方法。

```java
public Employee(String n,double s,int year,int day)
{
	name=Objects.requireNonNullElse(n,"unknown");
}
```

或者

```java
public Employee(String n,double s,int year,int day)
{
	Objects.requireNonNull(n,"The name cannot be null");
	this.name=n;
}
```

第一种方法当你传入null时,n将被赋值为unknown，第二种方法一旦传入null就直接抛出NullPointerException异常。

##### 1.3.7隐式参数与显式参数

对于在类内定义的方法

```java
public void raiseSalary(double Bypercent)
{
	double raise=this.salary*byPercent/100.0;
	this.salary+=raise;
}
```

在其他地方调用这个方法

```java
number007.raiseSalary(5.0);
```

raiseSalary方法实质上有两个参数，第一个参数被称为隐式(implicit)参数，是出现在方法名前的Employee类型的对象。第二个参数是参数列表里面的参数，这是一个显式(explicit)参数。

在每一个方法中，我们用关键字this来指示隐式参数。

##### 1.3.8封装的优点

一般来说，在比起提供一个公共数据字段，我们更倾向于提供

1. 一个私有的数据字段
2. 一个公共的字段访问器方法
3. 一个公共的字段修改器方法

<font color="red"> 注意:<b>不要编写返回可变对象引用的访问器方法！！！</b></font>

```java
public Date getHireDay()
{
	return hireDay;//这是一个不正确的访问器方法
}
```

因为Date类本身存在一个更改器方法setTime,使得其封装性会被破坏！

```java
Employee harry=...;
Date d=harry.getHireDay();
d.setTime(d.getTime()-1000);
//d和harry.hireDay()引用了同一个对象实例，对d调用更改器方法就可以自动的改变这个Employee对象的私有状态！
```

如果需要返回一个可变对象的引用，首先应该对他进行克隆，对象克隆是指存放在另一个新位置上的对象副本。

```java
public Date getHireDay()
{
	return (Date)hireDay.clone();
}
```



##### 1.3.9基于类的访问权限

一个方法可以访问所属类的所有对象的私有字段！

```java
public boolean equals(Employee others)
{
	return name.equals(other.name);
}
```

##### 1.3.10 私有方法

略

##### 1.3.11 final实例字段

我们可以把实例字段定义为final,这样的字段必须要在构造对象时初始化。

我们必须确保在每一个构造器执行之后，这个字段的值已经设置，并且以后不能再修改这个字段。

Tips:

1. final修饰符对于类型为基本类型或者不可变类的字段尤其有用。(如果类中所有方法都不会改变其对象，这样的类就被称为不可变类。)
2. 对于可变的类，final 关键字只是表示存储在对象变量中的对象引用不会变成其他的对象引用，不过这个对象依旧可以更改。

#### 1.4 静态字段与静态方法

##### 1.4.1静态字段

如果将一个字段定义为static，那么每一个类都只有一个这样的字段。而对于非静态的实例字段，每个对象都有自己的一个副本。

##### 1.4.2 静态常量

由于每个类对象都可以修改公共字段，所以最好不要有公共的静态字段。然而公共常量不存在这个问题，因为其被声明为final。

##### 1.4.3静态方法

静态方法是不在对象上执行的方法。

我们可以认为静态方法是不含隐式参数的方法，也就是说，他不存在this参数。

注意，我们可以使用对象调用静态方法，这是合法的，但是这样的写法容易造成混淆，我们建议使用类名来调用静态方法。

在下面两种情况下我们可以考虑使用静态方法:

1. 方法不需要访问对象状态，因为他需要的所有参数都由显式参数提供。
2. 方法只需要访问类的静态字段。

##### 1.4.4 工厂方法

静态方法还有另外一种常见的用途。类似LocalDate和NumberFormat的类使用静态工厂方法(factory method)来构造对象。

```java
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
NumberFormat percentFormatter = NumberFormat.getPercentInstance();
double x = 0.1;
System.out.println(currencyFormatter.format(x));
System.out.println(percentFormatter.format(x));
```

使用工厂方法的原因:

1. 无法命名构造器。构造器的名字必须与类名相同。但是，工厂方法有两个不同的名字。比如在上述例子之中，如果用普通的构造器，我们会发现同样都是空参，我们无法区别getCurrencyInstance和getPercentInstance。
2. 使用构造器时，无法改变所构造对象的类型。而工厂方法可以返回别的类型。比如在上述的例子之中，工厂方法返回的是DecimalFormat类的对象，这是NumberFormat的一个子类。


##### 1.4.5 main方法

main方法是一个static方法，这意味着main方法不含隐式参数。
每一个类都可以有一个main方法，这是常用于对类进行单元测试的一个技巧。

#### 1.5 方法参数

<font color="red">Java程序设计语言总是采用按值调用</font>

这意味着，方法得到的是所有参数值的一个副本。也就是说，方法不能修改传递给它的任何参数变量的内容。

然而，有两种类型的方法参数：基本数据类型和对象引用。

我们要注意到，一个方法不可能修改基本数据类型的参数，但是对象引用作为参数就不同了，参见下述例子:

```java
public static void tripleSalary(Employee x)
{
	x.raiseSalary(200);
}
```

当调用

```java
harry = new Employee(...);
tripleSalary(harry);
```

时，具体的执行过程为:

1. x初始化为harry值的一个副本，这里就是一个对象引用。
2. raiseSalary方法应用于这个对象引用。x和harry同时引用的那个Employee对象的工资提高了200%。
3. 方法结束之后，参数变量x不再使用。当然，对象变量harry继续引用那个工资增至三倍的员工对象。

Tips:

**Java的对象引用是按值传递的，x与harry同时引用一个对象，改变x也就改变了harry。**

下面总结一下在Java中对方法参数能做什么和不能做什么

1. 方法不能修改基本数据类型的参数。
2. 方法可以改变参数的状态。
3. 方法不能让一个对象参数引用一个新的对象。//**也就是说，实际上由于按值传递，我们不能改变传入的对象变量所指向的引用。**

#### 1.6 对象构造

##### 1.6.1 重载

如果多个方法(比如，StringBuilder构造器方法)有相同的名字、不同的参数，便出现了重载。编译器挑选具体调用哪个方法的过程被称为重载解析。

Tips：

1. Java允许重载任何方法，而不只是构造器方法。要完整地描述一个方法，需要指定方法名以及参数类型。方法名以及参数类型叫做方法的签名(signature)。
2. 返回类型不是方法签名的一部分。也就是说，不能有两个名字相同、参数类型也相同却有不同类型的方法。

##### 1.6.2 默认字段初始化

如果在构造器中没有显式地为类中的字段设置初值，那么就会被自动地赋为默认值：数值为0，布尔值为false，对象引用为null。

注意：方法中的局部变量必须明确地初始化，否则会报错！

##### 1.6.3 无参数的构造器

如果写一个类的时候没有编写构造器，那么类会为我们提供一个无参数构造器。这个构造器将所有的实例字段设置为默认值。

注意：如果我们为类提供了至少一个构造器，但是没有提供无参数的构造器，那么这时构造对象不提供参数就是不合法的。

##### 1.6.4 显式字段初始化

在java中，我们有很多种方式对实例字段进行初始化。

1. 直接为任何字段赋值

   ```java
   class Employee
   	{
   		private Stirng name="";
   }
   //在执行构造器之前我们就会完成这个赋值操作。
   ```

2. 在构造器之中为实例字段初始化

3. 初始化块(见1.6.7)

在1.6.7中，我将叙述构造顺序。

##### 1.6.5 参数名

参数变量会遮蔽同名的实例字段，但是我们可以使用this隐式参数来访问同名的实例字段。

```java
public Employee(String name,double salary)
{
	this.name=name;
	this.salary=salary;
}
```

##### 1.6.6 调用另一个构造器

关键字this指示一个方法的隐式参数，同时，this还有另一种含义:

如果构造器的<font color="red">第一个语句</font>形如this(...)，这个构造器将调用同一个类的另一个构造器。

```java
public Employee(double s)
{
	this("Employee # "+nextId,s);//calls Employee(String , double)
	nextId++;
}
```

##### 1.6.7 初始化块

在一个类的声明中，可以包含任意多个代码块，只要构造这个类的对象，这些块就会被执行。

```java
class Employee
{
	private static int nextId;
    private int id;
    private String name;
    private double salary;
    //object initialization block
    {
    	id=nextId;
    	nextId++;
    }
    public Employee(String name,double salary)
    {
    	this.name=name;
    	this.salary=salary;
    }
    public Employee()
    {
    	this("",0.0);
    }
    ...
}
```

<b><font color="red">下面是调用构造器的具体处理步骤:</font></b>

   1.<font color="blue">如果构造器的第一行调用了另一个构造器则基于所提供的的参数执行第二个构造器。//实际上，如果没有调用另外一个构造器，则会调用父类的默认构造器，也就是说，如果没有显式地继承某一个类，就会调用Object的默认构造器</font>

2. <font color="blue">否则, </font>
   	<font color="blue">(1).所有数据字段初始化为其默认值(0,false或者null)</font>
   	<font color="blue">(2).按照在<b>类声明中出现的顺序</b>，执行所有字段初始化方法和初始化块</font>
3. <font color="blue">执行构造器主体代码。</font>



我们还可以使用静态初始化块用来初始化静态字段

```java
static
{
	var generator = new Random();
	nextId = generator.nextInt(10000);
}
```

在类进行<b>第一次加载</b>的时候，将会进行静态字段的初始化。

与实例字段一样，除非将静态字段显式地设置成其他的值，否则默认的初始值是0、false或者null。

所有的静态初始化方法以及静态初始化块都将依照按照在<font color="red"><b>类声明中出现的顺序</b>执行。</font>

Attention:

**声明顺序看似不重要但在考试中非常重要！！！！！**

##### 1.6.8 对象析构与finalize方法

这是从Object类继承来的一个protected方法，基本已经被废弃。



#### 1.7 包



##### 1.7.1 包名

一般以因特网域名的逆序形式+工程子包名作为包名。



##### 1.7.2 类的导入

注意：在用*导入包的时候，一次只能导入一个包。

```java
import java.* ; //这是错误的
```



##### 1.7.3 静态导入

在源文件顶部，通过以下形式的命令可以实现导入类中的静态方法和静态字段。

```java
import static java.lang.System.*;
```

这样我们使用System类中的静态方法和静态字段时，就不需要加上System.前缀。

当然，我们也可以导入特定方法和字段:

```java
import static java.lang.System.out;
```



##### 1.7.4 在包中增加类

<font color="red">要想将类放入包中，就必须将包的名字放在源文件的开头！！！</font>



##### 1.7.5 包访问

在前面，我们已经接触到了public和private。标记为public的部分可以有任意类使用；标记为private的部分只能有定义它们的类使用。

如果没有指定public 或者private，这个部分(类、方法、变量)可以被同一个包中的所有方法访问。



##### 1.7.6 类路径

略



##### 1.7.7设置类路径

略



#### 1.8 JAR 文件



##### 1.8.1 创建jar文件

略



##### 1.8.2 清单文件

略



##### 1.8.3 可执行jar文件

略



##### 1.8.4 多版本jar文件

略



##### 1.8.5 关于命令行选项的说明

略



#### 1.9 文档注释



##### 1.9.1 注释的插入

略



##### 1.9.2 类注释

略



##### 1.9.3 方法注释

略



##### 1.9.4 字段注释

略



##### 1.9.5 通用注释

略



##### 1.9.6包注释

略



##### 1.9.7 注释抽取

略



#### 1.10 类设计技巧

1. 一定要保证数据私有

2. 一定要对数据进行初始化

3. 不要在类中使用过多的基本数据类型
   我们尽量使用其他的类替换使用多个相关的基本类型。 

4. 不是所有的私有字段都需要单独的字段访问器和字段修改器

5. 分解有过多职责的类

6. 类名和方法名要能够体现它们的职责

7. 优先使用不可变的类

   

### Chapter2:继承

#### 2.1 类、超类和子类

##### 2.1.1 定义子类

**要点：**

在java中，所有的继承都是公有继承，已经存在的类被称为父类或者超类(superclass)，继承的类被称为子类(subclass)

##### 2.1.2 覆盖方法

**要点:**

* 在java中，私有字段继承之后被封装，子类无法访问父类的私有字段。

  ```java
  public double getSalary()
  {	
  	retrun salary+bonus;//这里无法工作,因为salary是子类Manager从父类Employee中继承来的私有字段
  }
  ```

  ```java
  public double getSalary()
  {
  	return getsalary()+bonus;//这里同样无法工作，这里相当于写了一个无法停止的递归
  }
  ```

  ```java
  public double getSalary()
  {
  	return super.getsalary()+bonus//正确写法
  }
  ```

* 在Java中，若想访问父类的方法，可以使用super关键字解决问题

  但是，要注意：

  <font color="red">super与this并不相同!</font>

  super并不是一个对象的引用，因此不能把super赋给一个新的变量，而this可以。

  super只是一个指示编译器调用父类方法的特殊关键字。

  this的双重含义:1. 指示隐式参数的引用

                         2.调用该类的其他构造器

  super的双重含义:1.指示编译器调用父类方法的关键字

                            2.调用该类的父类构造器

  

##### 2.1.3 子类构造器

**要点:**

由于Manager类的构造器不能访问Employee类的私有字段，所以必须通过构造器来初始化这些字段。

* <font color ="red">调用构造器的语句只能作为另一个构造器的第一条语句出现。</font>

  构造器参数可以传给当前类的(this)的另一个构造器，也可以传递给超类(super)的构造器。

* 一个变量可以指示多种实际类型的现象称为多态，在运行时能够自动地选择恰当的方法，称为动态绑定。(dynamic binding)

##### 2.1.4 继承层次

**要点:**

由一个公共超类派生出来的所有类的集合被称为继承层次。

##### 2.1.5 多态

**要点:**

在Java程序设计语言中，<font color="blue">对象变量是多态的。一个父类类型的变量既可以引用一个父类类型的对象实例，也可以引用父类的任何一个子类的对象实例。</font>

<font color="green">for example</font>

```java
Manager boss = new Manager(...);
Employee[] staff =new Employee[3];
staff[0]=boss;
```

在这个exmaple中,staff[0]和Boss引用同一个对象，但是编译器只会把staff[0]看成是一个Employee对象。

这意味着，可以这样调用

```java
boss.setBonus(5000);//OK
```

但是不能这样调用

```java
staff[0].setBonus(5000);//Error
```

这是因为staff[0]声明的类型是Employee，而setBonus不是Employee类的方法。

注:<font color="red">多态父类引用在指向子类对象时只能调用子类对象从父类那继承来的方法(当然可以重写)</font>

##### 2.1.6 理解方法调用

**要点:**

方法调用的过程:

1. 编译器查看对象的声明类型和方法名。

2. 编译器确定方法调用中提供的参数类型，如果存在一个与所提供参数类型完全匹配的方法，就选择这个方法。这个过程被称为重载解析(overloading resolution)

   tips: 方法的名字和参数列表被称为方法的签名。

   如果在子类中定义了一个与超类签名相同的方法，那么子类中的这个方法就会覆盖超类中这个相同签名的方法。

   <font color="red">java允许子类将覆盖方法的返回值修改为原返回类型的子类型</font>

3. 如果是Private方法、static方法、final方法或者构造器，那么编译器将可以准确地知道应该调用哪一个方法。这被称为静态绑定。

   与此相对应的，如果要调用的方法依赖于隐式参数的实际类型，那么必须在运行时使用动态绑定。

4. 程序运行并且采用动态绑定调用方法时，虚拟机必须调用与x所引用对象的实际类型对应的那个方法。假设x的实际类型是D，他是C类的子类。如果D类定义了方法f(String),就会调用这个方法；否则，将在D类的超类中寻找f(string)，以此类推。
   <font color="red">java虚拟机预先为每一个类计算了一个方法表(method table),其
   中列出了所有方法的签名和要调用的实际方法。</font>

Warning:
	在覆盖一个方法的时候，子类方法的可见性不能低于超类方法的可见性。特别地，如果超类方法是Public,子类方法也必须被声明为public。

##### 2.1.7 阻止继承:final类和方法

**要点:**

在java中，不允许被继承的类被称为final类，在定义类的时候使用Final修饰符就表明这个类是final类。

将方法或者类声明为final的主要原因在于:确保他们在子类中不会被改变语义。比如说String类是final类，这意味着不允许任何人定义String的子类。换言之，如果有一个String的引用，它引用的一定是一个String对象，而不可能是其他类的对象。

##### 2.1.8 强制类型转换

**要点:**

在Java中，将一个值存入变量时，编译器将检查你是否承诺过多。<font color="red">**如果将一个子类的引用赋给一个超类变量，编译器是允许的。但是将一个超类的引用赋给一个子类变量时，就承诺过多了。**</font>必须进行强制类型转换，这样才能通过编译时的检查。

注意如果“谎报"对象包含的内容，在编译时不会被查出，但是在运行时会产生ClassCastException的异常:

```java
var staff=new Employee[3];
var boss=new Manager();
staff[0]=boss;
staff[1]=new Employee();

Manager boss1=(Manager)staff[1];//ClassCastException
```

可以看出在向下转型时，我们试图谎报staff[1]的类型，他作为Employee类型的指针并没有指向子类Manager但是我们试图把他转换成Manager，故在运行时抛出异常。

Tips:

	我们应该养成一个较为良好的代码习惯，也就是在强制类型转换之前，我们先要检查是否能够进行强制类型转换:

```java
Manager boss1;
if(staff[1] instanceof Manager)
{
	boss1=(Manager)staff[1];
}
```

综上所述:

1. 只能在继承层次内进行强制类型转换。(example: String c = (String) staff[0]; 很显然会报错)

2. 在将超类强制转换为子类之前，应该使用Instanceof进行检查。

3. 在大多数情况下并不需要将Employee对象强制转换成Manager对象，因为实现多态性的动态绑定机制能够自动找到正确的方法

   只有在使用Manager中特有的方法时才需要进行强制类型转换，例如setBonus方法。

##### 2.1.9 抽象类

**要点:**

1. 抽象类不能实例化，也就是说，如果将一个类声明为abstract，就不能创建这个类的对象。

2. 需要注意，<font color="red">可以定义一个抽象类的对象变量,但是这样的变量只能引用非抽象子类的对象。</font>

   ```java
   Person p=new Student();
   ```

   这里的p是一个抽象类型Person的变量，他引用了一个非抽象子类Student的实例。

##### 2.1.10 受保护访问

**要点:**

在Java中，最好将类中的字段标记为Private，将方法标记为public。任何声明为Privated的内容对其他类都是不可见的。这对于子类也完全使用，即子类也不能访问超类的私有字段。

不过在有些时候，你可能希望限制超类中的某个方法只允许子类访问，或者允许子类的方法访问超类的某一个字段。为此，你需要将这些类方法或字段声明为Protected。

example:-

现在考虑一个Administrator子类，这个子类在另一个不同的包中。<font color="red">Administrator类中的方法只能查看Administrator对象自己的保护字段，不能查看其他Employee对象的这个字段。</font>

<font color="blue">Java中的四个访问控制修饰符</font>

1. private-仅对本类可见
2. public -对外部完全可见
3. protected - 对本包和所有子类可见
4. 默认(不需要修饰符)-对本包可见

#### 2.2 Object:所有类的超类

##### 2.2.1 Object 类型的变量

可以使用Object类型的变量引用任何类型的对象:

```java
Object obj = new Employee("Harry Hacker" , 35000);
```

当然，Object类型的变量只能用于作为各种值的一个泛型容器。要想对其中的内容进行具体的操作，还需要弄清楚对象的原始类型，并进行相应的强制类型转换:

```java
if (obj instanceof Employee)
{
	Employee e = (Employee) obj;
}
```

在Java中，只有基本类型不是对象，例如，数值，字符和布尔类型的值都不是对象。

所有的数组类型，不管是对象数组还是基本类型的数组都扩展了Object类。

```java
Employee[] staff = new Employee[10];
obj = staff;
obj = new int[10];
```

##### 2.2.2 equals 方法

Object类中的equals方法用于检测一个对象是否等于另一个对象。

Object类中实现的equals方法用于确定两个对象引用是否相等。我们常常重写Object类中实现的equals方法

```java
public boolean equals (Object otherObject)
{
	//a quick test to see if the objects are identical
	if(this == otherobject) return true;
	
	//must return false if the explicit parameter is null
	if(otherObject == null) return false;
	
	//if the classes don't match, they can't be equal
	if(getClass()!=otherObject.getClass()) return false;
	
	// now we know otherObject is a non-null Employee
	Employee other = (Employee) otherObject;
	
	//test whether the fields have identical values
	return name.equals(other.name) && salary == other.salary && hireDay.equals(other.hireDay);
}
```

当子类调用equals方法时，首先要调用超类的equals。如果检测失败，对象就不可能相等。如果超类中的实例字段都相等，那么需要比较子类中的实例字段。

```
public class Manager extends Employee
{
	...
	public boolean equals(Object otherObject)
	{
		if(!super.equals(otherObject)) return false;
		//super equals checked that this and otherObject belong to the same class
		Manager other = (Manager) otherObject;
		return bonus == otherObject.bonus;
	}
}
```



##### 2.2.3 相等关系和继承

Java 语言规范要求equals方法具有以下特性

1. 自反性
2. 对称性
3. 传递性
4. 一致性(如果x和y引用的对象没有发生变化，反复调用x.equals(y)应该返回同样的结果)。

下面给出编写一个完美的equals方法的建议:

1. 显式参数命名为otherObject，稍后完美需要将其强制转换成另一个名为other的变量。
2. 检测this与otherObject是否相等
3. 检测otherObject是否为null,如果为null直接返回false
4. 比较this与otherObject的类，如果不一样直接返回false。尽量使用getClass()检测，若使用instanceof可能会出现违反对称性规则的情况
5. 将otherObject强制转换成相应类型的变量
6. 现在根据相等性概念的要求来比较字段。使用 == 比较基本类型字段，使用Objects.equals比较对象字段。

##### 2.2.4 hashCode方法

hashCode方法是Object类的一个方法，返回由对象导出的一个整型值。

注意，Object类的默认hashCode方法会从对象的存储地址得出散列码

在我们常用的类中，String类帮助我们重写了一个hashCode方法，String类的hashCode是由String所存储的内容算出来的

因此会出现如下问题:

```java
var s="ok";
var sb=new StringBuilder(s);
System.out.println(s.hashCode() + " " + sb.hashCode());
var t="ok";
var tb=new StringBuilder(t);
System.out.println(t.hashCode() + " " + tb.hashCode());
//我们会发现s.hashCode()和t.hashCode()相等，但是sb和tb不等
```

Tips:

* <font color = "red">equals方法和hashCode方法一般一并重写，而且两者必须相容。</font>
* 具体表现为equals方法相等的两个对象变量hashCode一定相等，hashCode相等equals不一定相等
* 也就是说equals必须比hashCode更严格

##### 2.2.5 toString方法

toString是Object类中的一个重要的方法，他会返回表示对象值的一个字符串。

在默认的Object类中的toString方法是返回 "对象的类名+@+散列码" 格式的字符串

我们一般会重写这个方法，这样我们可以直接System.out.println一个对象变量来获取他的信息。

#### 2.3 泛型数组列表

ArrayList是一个有类型参数的泛型类。

##### 2.3.1 声明数组列表

如果没有使用var关键字，可以省去右边的类型参数。

```java
ArrayList<Employee> staff = new ArrayList<>();
//如果用var声明ArrayList,省去类型参数的话会生成ArrayList<Object>
```

##### 2.3.2 访问数组列表元素

java中没有重载运算符，所以我们需要使用get()和set()方法来访问元素

##### 2.3.3 类型化与原始数组列表的兼容性

略

#### 2.4 对象包装器与自动装箱

自动装箱:

```java
Integer a =1000;//相当于Integer a = Integer.valueOf(1000);
```

自动拆箱:

```java
Integer a = Integer.valueOf(1000);
a++//自动拆箱为int，再执行++,再自动装箱
```

包装器类一般不直接调用比较运算符，需要调用equals方法或者compare方法。

Tips：

* 如果在<font color="red">一个条件表达式中混合使用Integer和Double类型，Integer值就会拆箱，提升为Double,再装箱为Double:</font>

  ```java
  Integer n =1;
  Double x=2.0;
  System.out.println(true :  n ? x);//prints 1.0
  ```

<font color="red">**Integer对象是不可改变的**</font>:包含在包装器中的内容不会改变。我们不能使用这些包装器类创建会修改数值参数的方法。

#### 2.5 参数数量可变的方法

也被称为变参方法(varargs)

```java
public static double max(double... values)
{
	double largest = Double.NEGATIVE_INFINITY;
	for(double v : values) if (v>largest) largest = v;
	return largest;
}
```

可以像下面这样调用这个方法

```java
double m = max(3.1,40.4,-5)
```

编译器将new double[]{3.1,40.4,-5}传递给max方法。

#### 2.6 枚举类

略

#### 2.7 反射

##### 2.7.1 Class类

略



##### 2.7.2 声明异常入门

略



##### 2.7.3 资源

略



##### 2.7.4 利用反射分析类的能力

略



##### 2.7.5 使用反射在运行时分析对象

略



##### 2.7.6 使用反射编写泛型数组代码

略



##### 2.7.7 调用任意方法和构造器

略



#### 2.8 继承的设计技巧

1. 将公共操作和字段放在超类中
2. 不要使用受保护的字段
3. 使用继承实现"is-a"关系
4. 除非所有继承的方法都有意义，否则不要使用继承
5. 在覆盖方法时，不要改变预期的行为
6. 使用多态，而不要使用类型信息
7. 不要滥用反射

### Chapter3:接口、Lambda表达式、内部类

#### 3.1 接口

##### 3.1.1 接口的概念

在Java程序设计语言中，接口不是类，而是对希望符合这个接口的类的一组需求。

<font color="green">example:</font>

> Arrays类中的sort方法承诺可以对对象数组进行排序，但要求满足下面的条件:对象所属的类必须实现Comparable接口
>
> 下面给出Comparable接口的代码:
>
> ```java
> public interface Comparable
> {
> 	int compareTo(Object other);
> }
> ```

接口所有的方法都默认为public方法，接口中决不允许有实例字段。

提供实例字段和方法实现的任务应该由实现接口的那个类来完成。

为了让类实现一个接口，通常需要完成下面两个步骤:

1. 将类声明为实现给定的接口。
2. 对接口中的所有方法提供定义。

注意：
	在实现接口时，我们必须把方法声明为public，因为在类中，方法默认为包访问，而继承要求子类方法可见性不能低于父类方法可见性。

##### 3.1.2 接口的属性

接口不是类。具体来说，不能使用new运算符实例化一个接口。

```java
x = new Comparable(...)//Error
```

尽管不能构造接口的对象，却能声明接口的变量:

```java
Comparable x ;//OK
```

接口变量必须引用实现了这个接口的类对象。

我们同样可以用Instanceof检查一个对象是否实现了某个特定的接口

```java
if (anObject instanceof Comparable)
{
	...
}
```

同类的继承一样，我们可以扩展接口。

```java
public interface Moveable
{
	void move(double x,double y);
}
```

虽然在接口中不能包含实例字段，但是可以包含常量。

```java
public interface Powered extends Moveable
{
	double milesPerGallon();
	double SPEED_LIMIT = 95;//a public static final constant
}
```

与接口中的方法自动被设置成public一样，接口中的字段总是被默认设置为public static final。

尽管每个类只能有一个超类，但却可以实现多个接口。可以使用逗号将想要实现的各个接口分隔开。

```java
class Employee implements Cloneable,Comparable
```



##### 3.1.3 接口与抽象类

使用抽象类表示通用属性有一个严重的问题：每个类只能拓展一个类。

##### 3.1.4 静态和私有方法

在Java8中，允许在接口中增加静态方法。

在Java9中，接口中的方法可以是Private。private方法可以是静态方法或实例方法。由于私有方法只能在接口本身的方法中使用，所以他们的用法很有限，只能作为接口中其他方法的辅助方法。

##### 3.1.5 默认方法

可以为接口方法提供一个默认实现，必须用default修饰符标记这样一个方法。

```java
public interface Comparable<T>
{
		default int compareTo(T other)
		{
			return 0;//by default,all elements are the same.
		}
}
```

##### 3.1.6 解决默认方法冲突

如果先在一个接口中将一个方法定义为默认方法，然后又在超类或另个一个接口中定义同样的方法，会发生什么情况？

在Java中对应的规则如下：

1. 超类优先。如果超类提供了一个具体方法，同名且有相同参数类型的默认方法会被忽略。
2. 接口冲突。如果一个接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型（不论是否为默认参数）相同的方法，必须覆盖这个方法来解决冲突。也就是说，如果某个类同时实现了两个具有相同签名方法的接口，那么类必须要把这个方法重写，否则会报错。

##### 3.1.7 接口与回调

回调(callback)是一种常见的程序设计模式。在这种模式中，可以指定某个特定事件发生时应该采取的动作。

例如，按下鼠标或者选择某个菜单项的时候，你可能希望完成某个特定的动作。

##### 3.1.8 Comparator 接口

在3.1.1节中，我们已经了解了如何实现对一个对象数组进行排序，前提是这些对象是实现了Comparable接口的类的实例。

例如，可以对一个字符串数组排序，因为String类实现了Comparable< String >，而且String.compareTo方法可以按字典顺序比较字符串。

那如果我们我们不想按照字典顺序，而是想按照长度递增的顺序去比较字符串呢？

要处理这种情况，Arrays.sort方法还提供了第二个版本，有一个数组和一个比较器作为参数，比较器是实现了Comparator接口的类的实例。

```java
public interface Comparator<T>
{
	int compare(T first,T second);
}
class LengthComparator implements Comparator<String>
{
	public int compare(String first,String second)
	{
		return first.length()-second.length();
	}
}

-------------------main-------------------
String[]a = new String[]{"abililty","abandon","abolish"};
Arrays.sort(a,new LengthComparator());
```

我们也可以在这个比较器对象上具体完成比较。

```java
var comp = new LengthComparator();
if(comp.compare(a[i],a[j])>0) ...
```

在3.2中我们会了解，运用lambda表达式将会更容易地使用Comparator。

##### 3.1.9 对象克隆

要了解克隆的具体含义，我们先来回忆一下为一个包含对象引用的变量建立副本时会发生什么。	

> 原变量和副本都是同一个对象的引用。这说明，任何一个变量改变都会影响另一个变量。
>
> ```java
> var original = new Employee("John Public",50000);
> Employee copy = original;
> copy.raiseSalary(10);//oops--also changed original
> ```

如果希望copy是一个新对象，它的初始状态与original相同，但是之后他们各自会有自己不同的状态，这种情况下我们就要使用clone方法。

```java
Employee copy=original.clone();
copy.raiseSalary(10);//OK--original unchanged
```

不过clone方法并没有那么智能。

默认的clone方法对我们要克隆的对象一无所知，只能逐个字段地进行拷贝。如果对象中所有数据字段都是数值或其他基本类型，拷贝这些字段没有任何问题。<font color="red">但是如果对象包含子对象的引用，拷贝字段就会得到相同子对象的另一个引用</font>，这样一来，原对象和克隆对象仍然会共享一些信息。

因此，<font color="red">默认的克隆操作是浅拷贝</font>,并没有克隆对象中引用的其他对象。所以，我们一般会重写clone方法来建立一个深拷贝，从而克隆所有子对象。

下面看看深拷贝的一个例子:

```java
class Employee implements Cloneable
{
	public Employee clone() throws CloneNotSupportedException
	{
		//call Object.clone();
		Employee cloned = (Employee) super.clone();
		//clone mutable fields
		cloned.hireDay = (Date) hireDay.clone();
		return cloned;
	}
}
```

总结:

> 对于每一个类，需要确定：
>
> 1. 默认的clone方法是否满足要求
> 2. 是否可以在可变的子对象上调用clone来修补默认的clone方法
> 3. 是否不该使用clone

实际上第三个选项是默认选项。如果选择第一项或者第二项，类必须

1. 实现Cloneable接口
2. 重新定义clone方法，并指定Public修饰符

注意：

1. Cloneable接口是Java提供的少数标记接口之一。（有些程序称之为记号接口）大家应该记得，Comparable等接口的通常用途是确保一个类实现一个或者一组特定的方法。标记接口不含任何方法；他唯一的作用就是允许在类型查询中使用instanceof。

2. 如果在一个对象上调用clone，但是这个对象并没有实现cloneable接口，Object类的clone方法就会抛出CloneNotSupportedException。

3. <font color="red">所有数组类型都有一个公共的clone方法。可以用这个方法建立一个新数组，包含原数组所有元素的副本。</font>

   ```java
   int[] luckyNumbers=new int[]{2,3,5,7,11,13};
   int[] cloned = luckyNumbers.clone();
   cloned[5] = 12;//doesn't change luckyNumbers[5]
   ```

   

#### 3.2 lambda 表达式

##### 3.2.1 为什么引入lambda表达式

lambda表达式是一个可传递的代码块。可以在以后执行一次或多次。

##### 3.2.2 lambda表达式的语法

lambda表达式的形式：(int i)

参数，箭头(->)，以及一个表达式。

例如：

```java
(String first,String second)->
{
	if(first.length()<second.length()) return -1;
	else if(first.length()>second.length()) return 1;
	else return 0;
}
```

即使lambda表达式没有参数，仍然要提供空括号，就像无参数方法一样

```java
()->
{
	for(int i=100;i>=0;i--) System.out.println(i);
}
```

如果可以推导出一个lambda表达式的参数类型，则可以忽略其类型。

```java
Comparator<String> comp
=(first,second)//same as (String first,String second)
 ->{return first.length()-second.length();}
//在这里，编译器可以推导出first和second必然是字符串，因为这个lambda表达式将赋予给一个字符串比较器。
```

如果lambda表达式的返回类型可以由上下文推导得出，则无须指定lambda表达式的返回类型。

```java
(String first,String second)->first.length()-second.length();
```

##### 3.2.3 函数式接口

对于只有一个抽象方法的接口，需要使用这种接口的对象时，就可以提供一个lambda表达式。这种接口被称为函数式接口(functional interface)。

##### 3.2.4 方法引用

当lambda表达式的体只调用一个方法而不做其他操作时，可以把lambda表达式重写为方法引用。

在使用方法引用时，要用::运算符分隔方法名与对象名或类名。主要有三种情况:

1. object::instanceMethod
2. Class::instanceMethod
3. Class::staticMethod

在第一种情况下，方法引用等价于向方法传递参数的lambda表达式。对于System.out::println,对象是System.out，所以方法表达式等价于x->System.out.println(x)。

```java
System.out::println ; //等价于x->System.out.println(x) ;
```

对于第二种情况，第1个参数会成为方法的隐式参数。例如，String::compareToIgnoreCase等价于(x,y)->x.compareToIgnoreCase(y)

```java
String::compareToIgnoreCase;//等价于(x,y)->x.compareToIgnoreCase(y);
```

在第三种情况下，所有参数都会被传递到静态方法。例如，Math::pow等价于(x,y)->Math.pow(x,y)

```java
Math::pow;//等价于(x,y)->Math.pow(x,y);
```

##### 3.2.5 构造器引用

构造器引用和方法引用很类似，只不过方法名为new。例如Person::new 是Person构造器的一个引用。

但是我们会调用哪一个构造器呢？

这取决与上下文。

注意，我们也可以用数组类型建立构造器引用。例如，int[]::new 是一个构造器引用，他有一个参数：数组的长度，相当于x->new int[x];

##### 3.2.6 变量作用域

有时，我们可能希望能够在lambda表达式中访问外围方法或者类中的变量。

但是，在lambda表达式中对引用外围变量有一个限制：<font color="red">lambda表达式中，只能引用值不会改变的变量。</font>

```java
public static void countDown(int start,int delay)
{
	ActionListener = event -> 
	{
		start--;//Error:Can't mutate captured variable;
		System.out.println(start);
	}
	new Timer(delay,listener).start();
}
```

换句话说:

<font color="red">lambda表达式中捕获的变量必须为事实最终变量。事实最终变量是指，这个变量初始化之后就不会再为它赋新值。</font>

lambda表达式与嵌套块有相同的作用域。这里同样适用命名冲突和遮蔽的有关规则。<font color="red">在lambda表达式中声明与一个局部变量同名的参数或者局部变量是不合法的。</font>

##### 3.2.7 处理lambda表达式

要使用lambda表达式或者使用方法引用，我们需要提供一个函数式接口。

```java
public interface IntConsumer
{
	void accept(int value);
}
public static void repeat(int n,IntConsumer action)
{
    for(int i=0;i<n;i++) action.accept(i);
}
--------------main--------------
repeat(10,i->System.out.println("CountDown: "+ (9-i)));
```

##### 3.2.8 再谈Comparator

comparator接口包含很多方便的静态方法来创建比较器。这些方法可以用于lambda表达式或方法引用。

静态comparing方法取一个"键提取器"函数，它将类型T映射为一个可比较的类型（如String)。对要比较的对象应用这个函数，然后对返回的键完成比较。

```java
Arrays.sort(people,Comparator.comparing(Person::getName));//相当于x->x.getName()
Arrays.sort(people,Comparator.comparing(Person::getLastName).thenComparing(Person::getFirstName));//如果lastname相同就会比较firstname
```

当然comparing方法还有一些变体形式，我们可以为其指定一个比较器。

```java
Arrays.sort(people,Comparator.comparing(Person::getName,(s,t)->Integer.compare(s.length(),t.length())));
```

另外，comparing和thencomparing都有变体形式，可以避免int、long或者double值的装箱。要完成前一个操作，还有一个更容易的做法:

```java
Arrays.sort(people,Comparator.comparingInt(p -> p.getName().getlength()));
```

如果键函数可以返回null，可能就要用到nullsFirst和nullsLast适配器。这些静态方法会修改现有的比较器，从而在遇到null值时不会抛出异常，而是将这个值标记为小于或大于正常值。

```java
Comparator.comparing(Person::getMiddleName(),Comparator.nullsFirst(...))
```

静态reverseOrder方法会提供自然顺序的逆序。要让比较器逆序比较，可以使用reversed实例方法。例如naturalOrder().reversed()等同于reverseOrder()。

#### 3.3 内部类

内部类(inner class)是定义在另一个类中的其他类。

我们为什么要使用内部类？

1. 内部类可以对同一个包中的其他类隐藏
2. 内部类方法可以访问定义这个类的作用域中的数据，包括原本私有的数据

##### 3.3.1 使用内部类访问对象状态

一个内部类方法既可以访问自己本身的数据字段，也可以访问创建他的外围类对象的数据字段。

因为内部类的对象总有一个隐式引用，指向创建它的外部类对象。

注：

我们也可以把内部类声明为私有private。这样一来，只有外部类的方法才能构造内部类的对象。

<font color="red">只有内部类才可以是私有的，而常规类可以有包可见性或公共可见性</font>

##### 3.3.2 内部类的特殊规则

内部类的对象使用外围类的引用的正规语法如下:

```java
outerClass.this
```

同样的，我们可以采用如下语法规则更加明确地编写内部类对象的构造器:

```java
outerObject.new InnerClass(construction parameters)
```

例如TimePrinter是一个公共内部类,对于任意一个语音时钟都可以构造一个TimePrinter:

```java
var jabberer=new TalkingClock(1000,true);
TalkingClock.TimePrinter listener = jabberer.new TimePrinter();
```

在外围类的作用域之外，可以这样引用内部类：

```java
OuterClass.InnerClass
```

**Attention:**

1. <font color="blue">内部类中声明的所有静态字段都必须是final,并且初始化为一个编译时常量。也就是说，内部类不能有非final的静态字段。</font>
2. <font color="blue">内部类中不能有static方法。</font>

##### 3.3.3 内部类是否有用、必要和安全

	略

##### 3.3.4 局部内部类

局部内部类是在方法中定义的内部类。

局部内部类特点与限制：

1. 声明局部类时不能有访问说明符(即public或private)。局部类的作用域被限定在声明这个局部类的块中。
2. 局部类有一个很大的优势，即对外部世界完全隐藏，同一个类的方法外代码都不能访问它。
3. 不仅能够访问外部类的字段，还可以访问局部变量。不过要注意，与lambda表达式类似，这些局部变量必须是事实最终变量

##### 3.3.5 由外部方法访问变量

略

##### 3.3.6 匿名内部类

假如只想创建这个类的一个对象，甚至不需要为这个类指定名字。这样的一个类被称为匿名内部类(anonymous inner class)。

一般语法如下：

```java
new SuperType(construction parameters)
{
	inner class methods and data
}
//其中，SuperType可以是一个接口，如果是这样，内部类就要实现这样一个接口；
//SuperType也可以是一个类，如果是这样，内部类就要拓展这个类。
```

由于构造器的名字必须和类名相同，而匿名内部类没有类名，所以，匿名内部类不能有构造器。实际上，构造参数要传递给超类(superclass)构造器。具体地，只要内部类实现一个接口，就不能有任何构造参数。不过，仍然要提供一个小括号。

也就是说，如果内部类构造器内有东西，那么一定是传递给超类构造器的参数。

```java
var queen = new Person("Mary");
//a person object
var count = new Person("Dracula"){...}
//an object of an inner class extending Person
```

尽管匿名类不能有构造器，但可以提供一个对象初始化块：

```java
var count = new Person("Dracula")
{
	(initialzation)
}
```

下面给出一个双括号初始化的例子：

```java
invite(new ArrayList<String>() {{add ("Harry");add("Tony");}});
//外层大括号建立一个匿名类作为ArrayList<String>的一个匿名子类。内层括号则是一个对象初始化块。
```

##### 3.3.7 静态内部类

有时候，使用内部类只是为了把一个类隐藏在另外一个类的内部，并不需要内部类有外围类对象的一个引用。为此，可以将内部类声明为static，这样就不会生成那个引用。

注意：

* <font color="red">如果内部类对象是在静态方法内构造的，我们就必须使用静态内部类</font>，因为普通内部类的构造会生成外围类对象的引用，而静态方法是不存在隐式参数的。
* 与常规内部类不同，常规内部类不能有static字段和方法，而静态内部类可以有static对象和方法。
* 在接口里定义的内部类自动为Public和static。

##### 3.3.8 内部类示例代码

```java
//内部类测试代码
package test2;

public class Test
{
    public static void main(String[] args)
    {
        var values=new double[20];
        for(int i=0;i< values.length;i++)
        {
            values[i]=100*Math.random();

        }
    }

}
class ArrayAlg
{
    public static int a=1;
    private int b=1;
    public class Triple
    {
        public void setOuterB()
        {
            ArrayAlg.this.b=2;//OuterClass.this是对外围类的引用;
        }
        //public static void k();报错，内部类无法定义静态方法
        //static int a=0;报错，内部类无法定义非final静态字段
    }
    public static class pair//当不需要内部类有外围类对象的一个引用的时候，我们可以把内部类声明为static
    {
        private double first;
        private double second;
        public pair(double f,double s)
        {
            this.first=f;
            this.second=s;
            //ArrayAlg.this.b=2;
            //Error,静态内部类无法引用b;因为没有对外围类对象的引用
        }

        public double getFirst() {
            return first;
        }

        public double getSecond() {
            return second;
        }
    }
    public static pair minmax(double[] values)//这里必须为静态方法，因为pair是静态内部类，没有对外围类对象的引用，如果不为静态方法，那么在return new pair(min,max)的时候会报错，因为无法生成对外围类对象的引用。
    {
        double max=Double.NEGATIVE_INFINITY;
        double min=Double.POSITIVE_INFINITY;
        for(double value:values)
        {
            if(max<value) max= value;
            if(min>value) min=value;
        }
        return new pair(min,max);
    }
    public void method(int testPara)
    {
        class methodClass
        {
            int a=5;
            int b=3;
            void add()
            {
                int c=a+b+testPara;
                //testPara++;报错，因为内部类的变量必须为事实最终变量
            }
        }
        methodClass k=new methodClass();
        System.out.println(k.a);
    }
}

```

#### 3.4 服务加载器

略

#### 3.5 代理

##### 3.5.1 如何使用代理

略

##### 3.5.2 创建代理对象

略

##### 3.5.3 代理类的特性

略

### Chapter4:异常、断言和日志

#### 4.1 处理错误

##### 4.1.1 异常分类

在Java程序设计语言中，异常对象都是派生与Throwable类的一个类实例。在下一层立刻分解为Error和Exception两大分支。

Exception类又有RuntimeExcepion和IOException等子类。Java语言规范将派生于Error类或RuntimeException类的异常称为非检查型异常，其他异常称为检查型异常。

##### 4.1.2 声明检查型异常

Java在下述四种情况下抛出异常：

1. 调用了一个抛出检查型异常的方法。
2. 检测到了一个错误，并且利用throw语句抛出一个检查型异常
3. 程序出现错误
4. Java虚拟机或运行时库出现错误

我们应当通过方法首部的异常规范声明这个方法可能抛出异常：

```java
class MyAnimation
{
	...
	public Image loadImage(String s) throws IOException
	{
		...
	}
}
```

如果一个方法有可能抛出抛出多个异常类型，那么就必须在方法的首部列出所有的异常类。每个异常类之间用逗号隔开。

```java
class myAnimation
{
	void drawImage(int i)throws FileNotFoundException,EOFException
	{
		...
	}
}
```

Attention:

* 如果在子类中覆盖了超类的一个方法，子类方法中声明的检查型异常不能比超类方法中声明的异常更通用(子类方法可以抛出更特定的异常，或者根本不抛出任何异常)
* 如果超类方法没有抛出任何检查型异常，子类也不能抛出任何检查型异常

##### 4.1.3 如何抛出异常

语法格式：

```java
throw new Exception();
//或者
var e = new Exception();
throw e;
```

如果已有一个异常类满足我们的要求，那么抛出这个异常非常容易，步骤如下:

1. 找到一个合适的异常类
2. 创建这个类的一个对象
3. 将对象抛出

一旦方法抛出了异常，这个方法就不会返回到调用者。

```java
String readData(Scanner in) throws EOFException
{
	...
	while(...)
	{
		if(!in.hasNext())//EOF encountered
		{
			if(n<len)
			{
				throw new EOFException();
			}
			...
		}
		return s;
	}
}
```

##### 4.1.4 创建异常类

我们需要做的只是定义一个派生于Exception的类，或者派生于EXception的某个子类。

自定义的这个类应该包含两个构造器：

1. 默认构造器
2. 包含详细描述信息的构造器(超类Throwable的toString方法会返回一个字符串，其中包含这个详细信息，这在调试中非常有用)

```java
class FileFormatException extends IOException
{
	public FileFormatException(){};
	public FileFormatException(String gripe)
	{
		super(gripe);
	}
}
String readData(BufferedReader in) throws FileformatException
{
	...
	while(...)
	{
		if(ch == 1)//EOF encountered
		{
			if(n < len)
			{
				throw new FileFormatException();
			}
		}
	}
}
```

#### 4.2 捕获异常

##### 4.2.1 捕获异常

如果发生了某个异常，但没有在任何地方捕获这个异常，程序就会终止，并在控制台上打印这个信息。

要想捕获一个异常，需要设置try/catch语句块。

```java
try
{
	code
	more code
	more code
}
catch (Exception Type e)
{
	handle for this type
}
```

如果try语句块中的任何代码抛出了catch子句中指定的一个异常类，那么

1. 程序将跳过try语句块中的其余代码
2. 程序将执行catch子句中的处理器代码

如果try语句块中的代码没有抛出任何异常，那么程序将跳过catch子句
如果方法中的任何代码抛出了catch子句中没有声明的一个异常类型，那么这个方法就会立即退出(希望它的调用者为这种类型的异常提供了catch子句)

一般来说，我们如果调用了一个抛出检查型异常的方法，就必须处理这个异常，或者继续传递这个异常。

一般经验是，我们要捕获哪些我们知道如何去处理的异常，继续传播那些不知道如何处理的异常。

Attention:

* 不允许子类的throws说明符中出现超类方法为列出的异常类

##### 4.2.2 捕获多个异常

我们要为每个异常类型使用一个单独的catch子句

```java
try
{
	//code that might throw exceptions
}
catch (FileNotFoundException e)
{
	//emergency action for missing files
}
catch (UnknownHostException e)
{
	//emergency action for unknown hosts
}
catch (IOException e)
{
    //emergency action for all other I/O problems
}
```

Attention:

* <font color="red">捕获多个变量时，异常变量隐含为final变量。例如，在以下子句体中不能为e赋不同的值</font>

  ```java
  catch (FileNotFoundException|UnknownHostException e){...}
  ```

##### 4.2.3 再次抛出异常与异常链

可以在catch子句中抛出一个异常。通常，在希望改变异常类型的时候会这样做。我们可以把原始异常设置为新异常的原因。

```java
try
{
	access the database
}
catch(SQLException original)
{
	var e=new ServletException("database error");
	e.initCause(orginal);
	throw e;
}
```

##### 4.2.4 finally子句

不管是否有异常被捕获，finally子句中的代码都会被执行。

```java
var in = new FileInputStream( . . . );
try
{
	// 1
	code that might throw exceptions
	// 2
}
catch
{
	//3
	show error message
	//4
}
finally
{
	//5
	in.close();
}
//6
```

这个程序可能在以下3中情况下执行finally子句

1. 代码没有抛出异常。这种情况下，执行顺序是1 2 5 6
2. 代码抛出一个异常，并在一个catch子句中被捕获。 
   如果catch子句没有抛出异常，执行顺序为1 3 4 5 6
   如果catch子句抛出了异常，异常将被抛回到这个方法的调用者，执行顺序将为1 3 5 
3. 代码抛出了一个异常，但是没有任何catch子句捕获这个异常。执行顺序将为1 5 

注意：

	当finally子句包含return 语句时，有可能产生意想不到的结果。假设利用return语句从try语句块中间退出。在方法返回之前，会执行finally子句块。如果finally块也有一个return语句，这个返回值将会遮蔽原来的返回值。

```java
public static int parseInt(String s)
{
	try
	{
		return Integer.parseInt(s);
	}
	finally
	{
		return 0;//ERROR
	}
}
```

finally子句主要用于清理资源。不要把改变控制流的语句(return,break,throw,continue)放在finally子句中。

##### 4.2.5 try-with-Resources 语句

假设类实现了AutoCloseable接口，那么我们就可以使用try-with-Resource语句

AutoCloseable接口有一个方法：

```java
void close() throws Exception
```

try-with-resource语句最简形式为:

```java
try(Resource res = ...)
{
	work with res
}
```

try语句退出时，会自动调用res.close().

##### 4.2.6 分析堆栈轨迹元素

stack trace堆栈轨迹是程序执行过程中某个特定点上所有挂起的方法调用的一个列表。当Java程序因为一个未捕获的异常而终止时，就会显示堆栈轨迹。

可以调用Throwable类的printStackTrace方法访问堆栈轨迹的文本描述信息。

#### 4.3 使用异常的技巧

1. 异常处理不能代替简单的测试
2. 不要过分的细化异常
3. 充分利用异常层次结构
4. 不要压制异常
5. 在检测错误时，苛刻比放任更好
6. 不要羞于传递异常

#### 4.4 使用断言

assert condition;

assert condition : expression;//如果结果为false,expression将传入AssertionError对象的构造器，并转换成一个消息字符串

##### 4.4.1 断言的概念

略

##### 4.4.2 启用和禁用断言

略

##### 4.4.3 使用断言完成参数检查

略

##### 4.4.4 使用断言提供假设文档

略

#### 4.5 日志

##### 4.5.1 基本日志

略

##### 4.5.2 高级日志

略

##### 4.5.3 修改日志管理器配置

略

##### 4.5.4 本地化

略

##### 4.5.5 处理器

略

##### 4.5.6 过滤器

略

##### 4.5.7 格式化器

略

##### 4.5.8 日志技巧

略

#### 4.6 调试技巧

略

### Chapter5:泛型程序设计

#### 5.1 为什么要使用泛型程序设计

泛型程序设计意味着编写的代码可以对多种不同类型的对象重用。

##### 5.1.1 类型参数的好处

拿ArrayList举例，在Java增加泛型类之前，泛型程序设计是通过继承实现的。ArrayList类只维护一个object引用的数组。

```java
public class ArrayList
{
	private Object[] elementData;
	public Object get(int i)
	{
		...
	}
	public void add(Object o)
	{
		...
	}
}
```

这种方法有两个问题

1. 当获取一个值的时候必须进行强制类型转换
2. 没有错误检查，可以向数组列表中增加任何类型的值

泛型程序设计很好地解决了这两个问题。泛型让ArrayList类有了一个类型参数用来指示元素的类型。

##### 5.1.2 谁想成为泛型型程序员

略

#### 5.2 定义简单泛型类

泛型类(generic class)就是有一个或多个类型变量的类。

泛型类引入了类型变量，用尖括号<>括起来，放在类名的后面。泛型类可以有很多个类型变量。

类型变量在整个类定义中用于指定方法的返回类型以及字段和局部变量的类型。

可以用具体的类型替换类型变量来实例化泛型类型

```java
public class Pair<T>//泛型类
{
	private T first;
	private T second;
	public Pair(){first = null;second = null;}//在写构造函数的时候不需要写Pair<T>
	public Pair(T first,T second){this.first=first;this.second=second;}
	public T getFirst(){return this.first;}
	public T getSecond(){return this.second;}
	public void setFirst(T newValue){first = newValue;}
	public void setSecond(T newValue){second = newValue;}
}
```

```java
Pair<String> p=new Pair<>("Student","XiaoMing");//实例化
var p2=new Pair<String>("Student","XiaoHong");
ArrayList<Pair<String>> list=new ArrayList<>();
list.add(p);
list.add(p2);
```

#### 5.3 泛型方法 

泛型方法的类型变量放在修饰符的后面(public static 后面)，并在返回类型的前面。

泛型方法可以在普通类中定义，也可以在泛型类中定义。

```java
class ArrayAlg
{
	public static <T> T getMiddle(T... a)
	{
		return a[a.length/2];
	}
}
```

当调用一个泛型方法时，可以把具体类型包围在尖括号中，放在方法名的前面:

```java
String middle = ArrayAlg.<String>getMiddle("John","Q.","Public");
```

如果编译器能够推断出泛型方法的类型，那么我们可以省略类型参数。

#### 5.4 类型变量的限定

下面的记法

```java
<T extends BoundingType>
//表示T应当是限定类型(BoundingType)的子类型(subType)。T和限定类型可以是类，也可以是接口。
```

一个类型变量或通配符可以有多个限定，例如:

```java
T extends Comparable & Serialzable
//限定类型用"&"分隔，而逗号用来分隔类型变量。
```

Attention:

* <font color="red">在Java的继承中，可以根据需要拥有多个接口超类型，但最多有一个限定可以是类。如果有一个类作为限定，他必须是限定列表中的第一个限定。</font>

#### 5.5 泛型代码和虚拟机

##### 5.5.1 类型擦除

无论何时定义一个泛型类型，都会自动提供一个相应的原始类型(raw type)。

这个原始类型的名字就是去掉类型参数后的泛型类型名。

类型变量会被擦除(erased)，并替换为限定类型

(对于无限定类型的变量则替换为Object，对于有多个限定类型的变量就用第一个限定类型)。

##### 5.5.2 转换泛型表达式

编写一个泛型方法调用时，如果擦除了返回类型，编译器会插入强制类型转换。例如，对于下面这个语句序列，

```java
Pair< Employee > buddies = new Pair<>(...);
Employee buddy=buddies.getFirst();
```

getFirst擦除类型后的返回类型是Object。编译器自动插入转换到Employee的强制类型转换。也就是说，编译器把这个方法调用转换为两条虚拟机指令:

1. 对原始方法Pair.getFirst的调用
2. 将返回的Object类型强制转换为Employee类型

当访问一个泛型字段时也要插入强制类型转换。

##### 5.5.3 转换泛型方法

类型擦除也会出现在泛型方法中

```java
public static <T extends Comparable> T min(T[] a)
public static Comparable min(Comparable[] a)//擦除之后的方法
```

注意：

	为了保持多态性，编译器会帮助我们生成桥方法(日后补充。4.20的睿信运动会太吵了 看不下去了)

总之，对于Java泛型的转换，需要记住以下几个事实：

1. 虚拟机中没有泛型，只有普通的类和方法。
2. 所有的类型参数都会替换为他们的限定类型。
3. 会合成桥方法来保持多态。
4. 为保持类型安全性，必要时会插入强制类型转换。

##### 5.5.4 调用遗留代码

使用@SuppressWarnings("unchecked")可以使warning消失。

#### 5.6 限制与局限性

##### 5.6.1 不能用基本类型实例化类型参数

不能用基本类型代替类型参数！我们要使用包装器类。

##### 5.6.2 运行时类型查询只适用于原始类型

虚拟机中的对象总有一个特定的非泛型类型，因此，所有的类型查询只产生原始类型。

```java
if (a instanceof Pair<String>)//Error,实际上只能测试a是否是任意类型的一个Pair
if (a instanceof Pair<T>)//Error,实际上只能测试a是否是任意类型的一个Pair
Pair<String> p = (Pair<String>) a;//warning -- can only test that a is a pair
```

同样的道理，getClass方法总是返回原始类型。

```java
Pair<String> stringPair = ...;
Pair<Employee> employeePair = ...;
if(stringpair.getClass() == employeePair.getClass())//they are equal
```

其比较的结果是true，因为两次getClass调用都返回Pair.class。

##### 5.6.3 不能创建参数化类型的数组

不能实例化参数化类型的数组！！

```java
var table=new Pair<String>[10]//error
```

但是，声明类型为Pair< String >[]的变量仍是合法的。不过不能用new Pair< String >[10]初始化这个变量。

Tips：

* 如果需要收集参数化类型的对象，简单地使用ArrayList会更好，比如上述例子用 ArrayList<Pair< String >>更安全、有效。

##### 5.6.4 Varargs 警告

上一节中已经说了，Java不支持泛型类型的数组。

但是在有些时候会不可避免，比如向参数个数可变的方法传递一个泛型类型的实例。

```java
public static <T> void addAll(Collection<T> coll,T... ts)
{
	for(T t:ts) coll.add(t);
}
```

如果我们考虑一下调用

```java
Collection<Pair< String >>table=...;
Pair< String> pair1=...;
Pair< String> pair2=...;
addAll(Collection,pair1,pair2);
```

为了调用这个方法，java虚拟机将不得不建立一个Pair< String>数组，这就违反了前面的规则。

不过，对于这种情况，规则有所放松，我们只会得到一个警告，而不是一个错误。

##### 5.6.5 不能实例化类型变量

不能再类似new T(...)的表达式中使用类型变量。例如下面的Pair< T >构造器就是非法的:

```java
public Pair(){first = new T();second = new T();}
```

##### 5.6.6 不能构造泛型数组

就像不能实例化泛型实例一样，也不能实例化泛型数组

```java
public static <T extends Comparable> T[] minmax(T... a)
{
	T[] mm=new T[2];//Error
}
```

**tips:<font color="red">但是传参传进来泛型数组可以用！</font>**

##### 5.6.7 泛型类的静态上下文中类型变量无效

<font color="red">不能在静态字段或方法中引用类型变量!!!</font>

```java
public class Singleton<T>
{
	private static T singleInstance;//error
	public static T getSingleInstance()//error
	{
		if(singleInstance==null) 
		{
			construct new instance of T;
			return singleInstance;
		}
	}
}
```

##### 5.6.8 不能抛出或捕获泛型类的实例

<font color="red">既不能抛出也不能捕获泛型类的对象。</font>

实际上，泛型类拓展throwable甚至都是不合法的。

```java
public class Problem<T> extends Exception//error--can't extend Throwable
```

catch子句中不能使用类型变量，下述例子就无法编译:

```java
public static <T extends Throwable> void doWork(Class<T> t)
{
	try
	{
		do work
	}
	catch (T e)//error-- can't catch type variable
	{
		logger.global.info(...);
	}
}
```

不过在异常规范中使用类型变量是允许的

```java
public static <T extends Throwable>void d
{
	try
	{
		do work
	}
	catch(Throwable realCause)
	{
		t.initCause(realCause);
		throw t;
	}
}
```

##### 5.6.9 可以取消对检查型异常的检查

Java异常处理的一个基本原则是，必须为所有检查型异常提供一个处理器。不过可以利用泛型取消这个机制

##### 5.6.10 注意擦除后的冲突

当泛型类型被擦除后，不允许创建引发冲突的条件。下面给出一个实例:

```java
public class Pair<T>
{
	public boolean equals(T value)
	{
		return first.equals(value)&&second.equals(value);
	}
}
//考虑一个Pair<String>。从概念上来讲，它有两个equals方法
boolean equals(String)//defined in Pair<T>
boolean equals(Object)//inherited from Object
//但是，直觉将把我们引入歧途
boolean equals(T)
----在擦除后就是----
boolean equals(Object)
//这与Object.equals方法发生冲突
```

<font color="red">**泛型的规范说明还引用了另一个原则：**</font>

<font color="blue">为了支持擦除转换，我们要施加一个限制:倘若两个接口类型是同一个接口的不同参数化，一个类或类型变量就不能同时作为这两个接口类型的子类。</font>

```java
class Employee implements Comparable<Employee>{...}
class Manager extends Employee implements Comparable<Manager>{...}//error
```

但是，非泛型版本时是合法的

```java
class Employee implements Comparable{...}
class Manager extends Employee implements Comparable{...}//OK
```

原因在于合成的桥方法起了冲突。

实现了Comparable< X >的类会获得一个桥方法:

```java
public int compareTo(Object other)
{
	return compareTo((X) other);
}
```

X 不同桥方法不同，但是桥方法签名相同，从而产生冲突。

#### 5.7 泛型类型的继承规则

<font color="red">无论S与T有什么关系，通常，Pair< S >和Pair< T >都没有任何关系。</font>

#### 5.8 通配符类型

##### 5.8.1 通配符概念

在通配符类型中，允许类型参数发生变化。

Pair< ? extends Employee >

表示任何泛型Pair类型，它的类型参数是Employee的子类。

##### 5.8.2 通配符的超类型限定

通配符限定与类型变量限定很相似，但是它还有一个附加的能力，即可以指定一个超类型限定(supertype bound)，如下所示:

? super Manager

这个通配符限制为Manager的所有超类型。

Attention:

1. 带有超类型限定的通配符允许你写入一个泛型对象，而带有子类型限定的通配符允许你读取一个泛型对象。
2. <font color="blue">使用通配符? extends ...只能调用方法返回值，不能提供方法参数</font>
3. <font color="blue">使用通配符? super ...只能为方法提供参数，不能调用方法返回值</font>

##### 5.8.3 无限定通配符

还可以使用根本无限定的通配符，但是它局限性很大对于Pair< ? >

? getFirst()

void setFirst(?)

getFirst的返回值只能赋给Object。setFirst方法甚至不能被Object调用。

Pair< ? >和Pair本质的不同在于:可以用任意Object对象调用原始的Pair类的setFirst方法。

##### 5.8.4 通配符捕获

通配符不是类型变量，因此，不能在编写代码中使用"?"作为一种类型。

```java
public static void swapHelper(Pair<?> p)
{
	? temp=p.getFirst();//error
	p.setFirst(p.getSecond());
	p.setSecond(temp);
}
```

但是，我们可以用构造方法参数进行通配符捕获:

```java
public static <T> void swapHelper(Pair<T> p)
{
	T temp=p.getFirst();
	p.setFirst(p.getSecond());
	p.setSecond(temp);
}


public static void swap(Pair<?> p)
{
	swapHelper(p);
}
```

注意，swapHelper方法是一个泛型方法，而swap不是，他有一个固定的Pair<?>类型的参数

<font color="blue">通配符捕获只有在非常限定的情况下才是合法的。编译器必须能够保证通配符表示单个确定的类型。</font>

#### 5.9 反射与泛型

##### 5.9.1 泛型class类

##### 5.9.2 使用Class< T >参数进行类型匹配

##### 5.9.3 虚拟机中的泛型类型信息

##### 5.9.4 类型字面量

### Chapter6:Java8的流库

#### 6.1 从迭代到流的操作

#### 6.2 流的创建

#### 6.3 filter、map和flatMap方法

#### 6.4 抽取子流和组合流

#### 6.5 其他流转换

#### 6.6 简单约简

#### 6.7 Optional类型

##### 6.7.1 Optional类型

 ##### 6.7.2 消费Optional值

 ##### 6.7.3 管道化Optional值

 ##### 6.7.4 不适合使用Optional值的方式

 ##### 6.7.5 创建Optional值

 ##### 6.7.6 用flatMap构建Optional值的函数

 ##### 6.7.7 将Optional转换为流

 #### 6.8 收集结果

 #### 6.9 收集到映射表中

 #### 6.10 群组和分区

 #### 6.11 下游收集器

 #### 6.12 约简操作

 #### 6.13 基本类型流

 #### 6.14 并行流

### Chapter7:输入与输出

 #### 7.1 输入/输出流

在java中，可以从其中读入一个字节序列的对象称做输入流，而可以向其中写入一个字节序列的对象称做输出流。

 ##### 7.1.1 读写字节

InputStream类有一个抽象方法:

abstract int read()

这个方法将读入一个字节，并返回读入的字节，或者在遇到输入源结尾时返回-1。

还有很多方法，常用的如下:

int read(byte[] b)

读入一个字节数组，并返回实际读入的字节数，或者在碰到输入流的结尾时返回-1。这个read方法最多读入b.length个字节。

int read(byte[] b,int off,int len)

off是偏移量，len是读取长度

long skip(long n)

在输入流中跳过n个字节，返回实际跳过的字节数(如果碰到输入流的结尾，则可能小于n)

void close()

关闭这个输入流

outputStream与InputStream方法构成类似，常用的如下:

abstract void write(int n)

写出一个字节的数据

void write(byte[] b)

void write(byte[]b ,int off,int len)

写出所有字节或者某个范围的字节到数组b中

void close()

冲刷并关闭输出流

void flush()

冲刷输出流，也就是将所有缓冲的数据发送到目的地

Attention:

* 当你完成对输入/输出流的读写时，应该通过调用close方法来关闭它。
* 关闭一个输出流的同时还会冲刷用于该输出流的缓冲区，所有被置于缓冲区中的字节在关闭输出流的时候都会被送出。<font color="red">如果不关闭输出流，那么写出字节的最后一个数据包可能永远也得不到传递</font>
* 我们还可以用flush方法来人为地冲刷这些输出。

 ##### 7.1.2 完整的流家族

我们可以把输入/输出流家族按照他们的使用方法分成两个单独的层次结构：字节流和字符流。

字节流的基类是InputStream和OutputStream

字符流的基类是Reader和Writer

然后我们还有四个附加接口Closeable,Flushable,Readable,Appendable

InputStream、OutputStream、Reader和Writer都实现了Closeable接口。

OutputStream和Writer还实现了Flushable接口。

Reader实现了Readerable接口。

Writer实现了Appendable接口。

 ##### 7.1.3 组合输入/输出流过滤器

**<font color="red">很重要！！！</font>**

**<font color="blue">Java可以将多个流过滤器组合起来。这种混合并匹配过滤器类以构建真正有用的输入/输出序列的能力，将带来极大的灵活性</font>**

```java
DataInputStream din=new DataInputStream(new BufferedInputStream(new FileInputStream(new File(...))));
```

在上述例子中，我们通过FileInputStream来对磁盘上的文件提供输入流

通过BufferedInputStream来实现缓冲机制(**输入流在默认情况下是不带缓冲区的，也就是说，每个对read的调用都会请求操作系统再分发一个字节。相比之下，请求一个数据块并将其置于缓冲区会显得更加高效。**)

通过DataInputStream来实现将字节组装到更有用的数据类型之中。

 ##### 7.1.4 文本输入与输出

OutputStreamWriter类将使用选定的字符编码方式，将Unicode码元的输出流转换为字节流。

而InputStreamReader类将包含字节(用某种字符编码方式表示的字符)的输入流转换为可以产生Unicode码元的读入器。

例如，下面的代码就展示了如何让输入读入器从控制台读入键盘敲击信息，并将其转换为Unicode:

```java
var in = new InputStreamReader(System.in);
```

同样也可以用某种指定的编码方式读入一个文件

```java
var in = new InputStreamReader(new FileInputStream("data.txt"),StandardCharsets.UTF_8);
```

 ##### 7.1.5 如何写出文本输出

对于文本输出，可以使用PrintWriter。

为了打印文件，我们需要用文件名和字符编码方式构建一个PrintStream对象

```java
var out = new PrintWriter("employee.txt",StandardCharsets.UTF_8);
```

为了输出到打印写出器，需要使用print/println/printf方法。

 ##### 7.1.6 如何读入文本输入

最简单的处理任意文本的方式就是Scanner类。我们可以从任何输入流中构建Scanner对象。

```java
Scanner cin = new Scanner(System.in);
Scanner fin = new Scanner(new FileInputStream(new File("...")));
```

在早期的java版本中，处理文本输入的唯一方式就是通过BufferedReader类。它的readLine方法会产生一行文本。或者在无法获得更多输入的时候返回null。

典型的输入循环如下:

```java
BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream(new File(...))));
String line;
While((line=in.readLine())!=null)
{
	do something with line;
}
```

注意，与Scanner不同，bufferedreader没有读入数字的方法。

 ##### 7.1.7 以文本格式存储对象

略

 ##### 7.1.8 字符编码方式

Java的编码方式是高位优先

 #### 7.2 读写二进制数据

 ##### 7.2.1 DataInput和DataOutput接口

DataOutput接口定义了下面用于以二进制格式写数组、字符、boolean值和字符串的方法:

writeChars(String s) writeByte(int b) writeBoolean(boolean b) writeChar(int c) writeDouble(double d)... WriteUTF(String s)

注：因为没有其他方法会使用UTF-8的这种修订，所以你应该只在写出用于Java虚拟机的字符串时才使用writeUTF方法。(例如需要编写一个生成字节码的程序时)对于其他场合，我们都应该使用writeChars方法。

DataInput接口行为与DataOutput接口的行为类似。

DataInputStream类实现了DataInput接口，为了从文件读入二进制数据，可以将DataInputStream与某个字节源相结合，例如FileInputStream。

```java
var in = new DataInputStream(new FileInputStream("employee.txt"));
```

与此类似，要想写出二进制数据，可以使用实现了DataOutput接口的DataOutputStream类

 ##### 7.2.2 随机访问文件

RandomAccessFile类可以在文件中的任何位置查找或写入数据。

类内定义了很多方法，下面列出常用的几个:

1. 构造器方法:
   RandomAccessFile(String file,String mode)

   RandomAccessFile(File file,String mode)

   打开给定的用于随机访问的文件。mode字符串："r"表示只读模式，"rw"表示读/写模式，"rws"表示每次更新时，都对数据和元数据的写磁盘操作进行同步的读/写模式;"rwd"表示每次更新时，只对数据的写磁盘操作进行同步的读/写模式。
   	```
   	RandomAccessFile in = new RandomAccessFile("Employee.dat","rw");
   	```

2. long getFilePointer()
   返回文件指针的当前位置

3. void seek(long pos)
   将文件指针设置到距离文件开头pos个字节处

4. long length()
   返回文件按照字节来度量的长度。

RandomAccess类同时实现了DataInput和DataOutput接口。为了随机读写文件，我们可以使用在前一小节介绍的DataInput接口和DataOutput接口中的方法，比如readChars()和WriteChars(String s)方法。
当然，我们可以通过seek方法来调整文件指针的位置来实现对任意位置的读写。

 ##### 7.2.3 ZIP文档

略

 #### 7.3 对象输入/输出流与序列化

Java语言支持一种被称为对象序列化(object serialization)的非常通用的机制，它可以将任何对象写出到输出流中，并在之后将其读回。

 ##### 7.3.1 保存和加载序列化对象

为了保存对象数据，首先需要打开一个ObjectOutputStream对象:

```java
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("employee.dat"));
```

为了保存对象，可以直接使用ObjectOutputStream的writeObject方法。

```java
var harry = new Employee("Harry Hacker",50000,1989,10,1);
out.writeObject(harry);
```

同样的，为了将这些对象读回，首先需要打开一个ObjectInputStream对象:

```java
var in = new ObjectInputStream(new FileInputStream("employee.dat"));
```

然后用readObject方法将这些对象被写出时的顺序获得它们。

```java
var e1 = (Employee) in.readObject();
```

但是，对于希望使用对象输出/输入流读写的类<font color="red">必须要实现Serializable接口</font>

Serializable接口没有任何方法，是一个类似于Cloneable接口的标记接口。

注意：

1. 我们只有在读写对象的时候要用到readObject/writeObject，对象流同样也实现了DataInput接口和DataOutput接口，读写基本类型要用readInt等方法。
2. 每个对象都是用一个序列号保存的，这就是这种机制被称为对象序列化的原因，下面是算法:
   1. 遇到的每一个对象引用都关联一个序列号
   2. 对于每个对象，当第一次遇到时，保存其对象数据到输出流中
   3. 如果某个对象之前已经被保存过，那么只写出"与之前保存过的序列号为x的对象相同"
3. 在读回对象的时候，整个过程是反过来的:
   1. 对于对象输入流中的对象，在第一次遇到其序列号时，构建它，并使用流中的数据来初始化他，然后记录这个顺序号和新对象之间的关联。
   2. 当遇到"与之前保存过的序列号为x的对象相同"这一标记时，获取与这个序列号相关联的对象的引用。

我们可以发现，正是这种机制让我们便于保存有字段引用同一个变量的对象变量。

Attention:**在writeObject时，最后要写一个null,为了方便日后反序列化!**因为readObject只有读到null才会停止读入。不这样做很容易出现EOFException。

 ##### 7.3.2 理解对象序列化的文件格式

略

 ##### 7.3.3 修改默认的序列化机制

某些数据域是不可序列化的。

Java中只要将这种与标记为transient就可以是这些域不被序列化

注意:

<font color="red">如果在一个implements Serializable的类中有字段是不可序列化的类的变量，那么必须将其标记为transient</font>

```java
pubilc class LabeledPoint implements Serializable
{
	private String label;
	private transient Point2D.Double point;
}
```

除了让序列化机制来保存和恢复对象数据，类还可以定义他们自己的机制。为了做到这一点，这个类必须实现Externalizable接口。这需要定义两个方法:

```java
public void readExternal(ObjectInputStream in)throws IOException,ClassNotFoundException
public void writeExternal(ObjectOutputStream out)throws IOException
```

Externalizable接口扩展自java.io.Serializable接口。实现java.io.Serializable即可获得对类的对象的序列化功能。而Externalizable可以通过writeExternal()和readExternal()方法可以指定序列化哪些属性。

**Externalizable与Serializable的异同**

1. 序列化内容
   Externalizable自定义序列化可以控制序列化的过程和决定哪些属性不被序列化。

2. Serializable序列化时不会调用默认的构造器，而Externalizable序列化时会调用默认构造器的

3. 使用Externalizable时，必须按照写入时的确切顺序读取所有字段状态。否则会产生异常。例如，如果更改ExternalizableDemo类中的number和name属性的读取顺序，则将抛出java.io.EOFException。而Serializable接口没有这个要求。

```java
public class ExternalizableDemo implements Externalizable {
    private static final long serialVersionUID = 1L;
    private String name;private int number;
    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeInt(number);
    }
    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        this.name = in.readUTF();
        this.number = in.readInt();
    }
    public String getName(){return this.name;}
    public void setName(String name){this.name = name;}
    public int getNumber(){return this.number;}
    public void setNumber(int number){this.number = number;}
}

public class ExternalizableTest 
{
   public static void main(String[] args) 
   {
       try{ testExternalizable("E://java_training_camp//files//my_file//demo.txt");}
       catch(IOException e){System.out.println("输入输出错误"); e.printStackTrace();}
       catch(ClassNotFoundException e){e.printStackTrace();System.out.println("找不到该类");}
   }
   public static void testExternalizable(String OUTPUT_FILE) throws IOException, ClassNotFoundException 
   {
       ExternalizableDemo demo = new ExternalizableDemo();
       demo.setNumber(1004);
       demo.setName("JIH");
       ExternalizableDemo seg = new ExternalizableDemo();
       FileOutputStream fileOutputStream = new FileOutputStream(OUTPUT_FILE);
       ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
       demo.writeExternal(objectOutputStream);
       objectOutputStream.flush();objectOutputStream.close();fileOutputStream.close();
       FileInputStream fileInputStream = new FileInputStream(OUTPUT_FILE);
       ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
       seg.readExternal(objectInputStream);
       objectInputStream.close();fileInputStream.close();
   }
}
```

 ##### 7.3.4 序列化单例和类型安全的枚举

注意，即使构造器是私有的，序列化机制也可以创建新的对象!

为了解决这个问题，我们需要定义另外一种称为readResolve的特殊序列化方法。这个方法在对象被序列化之后就会调用它。它必须返回一个对象，而该对象之后会成为readObject的返回值。

 ##### 7.3.5 版本管理

用以下代码来定义序列化版本:

```java
public static final long serialVersionUID = ...;
```

对象输入流将拒绝读入UID(指纹)不同的对象。

 ##### 7.3.6 为克隆使用序列化

序列化提供了一种克隆对象的简便途径：

直接将对象序列化到输出流中，然后将其读回。这样产生的对象是对现有对象的一个深拷贝。

 #### 7.4 操作文件

暂时使用File类即可，以后有空再学Path和Files。

##### 7.4.0 File

###### 7.4.0.1 File类定义 

1. File类主要是JAVA为文件这块的操作(如删除、新增等)而设计的相关类
2. File类的包名是java.io，其实现了Serializable, Comparable两大接口以便于其对象可序列化和比较

###### 7.4.0.2 File类实例域

1. path ：封装的String类型变量，代表了文件的路径
2. separatorChar：静态常量,代表了运行该程序的系统的路径分隔符，windows系统为"\" ,linux系统为“/”
3. separator ：静态常量，是由separatorChar扩展而来的字符串型常量，运行结果和separatorChar一样
4. pathSeparatorChar ：静态常量，代表用于分割多个路径的系统分隔符。 在UNIX系统上，此字符为'：'而 Windows系统上它是';'
5. pathSeparator： 静态常量，由pathSeparatorChar扩展而来的字符串，运行结果和pathSeparatorChar一样

###### 7.4.0.3 File类构造函数

注意：构造函数只是创建一个File实例，并没有以文件做读取等操作，因此路径即使是错误的，也可以创建实例不报错

1. 通过给定的字符串路径(一般是文件的绝对路径)转为抽象路径名用来创建File实例，当传入null时会抛出NullPointerException空异常错误
2. 从父路径名字符串和子路径名字符串(一般是相对父类的相对路径)创建新的File实例
   2.1. 若子路径child为Null，会抛出NullPointerException空异常错误
   2.2. 当父路径为Null时，会以子路径child作为绝对路径创建实例，等同于调用第一个File(String child )效果一样
   2.3. 当父路径不为空时，会以父路径作为目录，子路径作为父路径下的目录或者文件名，最后得到的实例对象的路径就是父路径和子路径的组合
3. 通过父路径File实例对象和子路径字符串创建新的File实例，等同于上面的方法中把父路径字符串创建File实例然后传入一样
4. 通过将给定的 file: URI 转换为一个抽象路径名来创建一个新的 File 实例

###### 7.4.0.4 File类中的各种常用方法

1. 获取实例对象代表的文件名字(包含文件后缀)

```java
public String getName()}
```

2. 获取实例对象代表的文件上级目录

```java
public String getParent()
```

3. 获取实例对象的父项的实例对象，如果此路径名未指定父目录，则返回null；也就是获取对象的上级目录然后再实例化一个对象

```java
public File getParentFile()
```

4. 获取实例对象代表的文件的实际路径

```java
public String getPath()
```

5. 检测该实例对象代表的文件的路径是否是绝对路径(windows系统中路径是以驱动盘开始的就是绝对路径)

```java
public boolean isAbsolute()
```

6. 获取实例对象代表的文件的绝对路径

```java
public String getAbsolutePath()
```

7. 实例对象代表的文件是否存在

```java
public boolean exists()
```

8. 检测实例对象代表的是否是文件

```java
public boolean isFile()
```

9. 检测实例对象代表的是否是目录

```java
public boolean isDirectory()
```

10. 创建新文件--当且仅当实例对象代表的文件不存在时才可以创建新文件

```java
public boolean createNewFile()  throws IOException
```

11. 删除实例对象代表的文件或目录，当代表目录时，必须目录下为空才可以删除

```java
public boolean delete()
```

12. 根据实例对象的路径名创建目录(若目录已存在，则false;若路径是文件，则fasle;若路径的上级目录不存在则false)

```java
public boolean mkdir()
```

13. 根据实例对象的路径创建目录，包括创建那些必须的且不存在的父级目录

```java
public boolean mkdirs()
```

14. 获取实例对象代表的文件下的各级文件名和目录名，返回一个字符串数组  

```java
public String[] list()
```

14.1.  当实例对象代表的是文件不是目录时，返回NUll对象
14.2.  获取的是该目录下的文件名和目录名,并不包含该目录名称和其上级目录名称
14.3.  字符串数组中都是文件名或目录名并不是路径名
14.4.  字符串中的元素并不是按照实际系统中目录下的顺序排列的

15. <font color="red">获取实例对象代表的文件下的各级文件名和目录名并指定过滤器进行过滤，返回一个字符串数组 </font>

```java
public String[] list(FilenameFilter filter)
```

15.1. 过滤器是FilenameFilter类对象，当传入null时，效果和list()方法一样
15.2. 过滤器是指过滤掉不符合名称的名字
15.3. FilenameFilter 是一个接口，因此需要自己新建一个类来实现该接口作为参数传入进去
15.4.仔细研究方法的源码可以发现，所谓过滤就是要重写FilenameFilter的accept方法并在方法中过滤

```java
//list方法源码
public String[] list(FilenameFilter filter) {
        String names[] = list();
        if ((names == null) || (filter == null)) {
            return names;
        }
        List<String> v = new ArrayList<>();
        for (int i = 0 ; i < names.length ; i++) {
            if (filter.accept(this, names[i])) {  //必须重写accept方法，并在方法内对传入的name进行判定是否合乎过滤条件
                v.add(names[i]);
            }
        }
        return v.toArray(new String[v.size()]);
    }
```

例如我们只要后缀名是txt的文件名称，那么可以按照以下逻辑来处理:

```java
public class TxtFilter implements FilenameFilter
{
    @Override
    public boolean accept(File dir, String name)
    {
        if(name.endsWith(".txt"))
        {
            return true;
        }
        return false;
    }
}
```

我们还可以用过滤条件作为构造参数传入构造方法中初始化其变量，这样就是可变的

```java
public class TxtFilter implements FilenameFilter
{
    private String FilterName; // 作为过滤器名称变量
 
    public TxtFilter(String FilterName)
    {
 
        this.FilterName = FilterName; // 初始化构造过滤器名称，这样就不用每次都创建新类
 
    }
 
    public String getFilterName()
    {
 
        return FilterName; // 公有的域访问器方法，提供接口获取
    }
 
    @Override
    public boolean accept(File dir, String name)
    {
 
        String fileterNameString = this.getFilterName(); // this代表着调用accept方法的变量
 
        if (name.endsWith(fileterNameString))
        {
            return true;
        }
        return false;
    }
}
--------main---------
        File file2 = new File("E:/test");
        TxtFilter filter=new TxtFilter(".txt"); //过滤器，只需要.txt结尾的
        String[] list = file2.list(filter);
        for (int i = 0; i < list.length; i++)
        {
            System.out.println(list[i]); // 结果为1.txt
        }

```

当然，也可以直接用lambda表达式来调

```java
File file=new File("C:\\Users\\Administrator\\Desktop\\sqlearning");
String[] list= file.list((f,name)->name.endsWith(".jar"));
assert list!=null;for(String string:list) System.out.println(string);
```

当然也可以匹配正则表达式//自己写的hh

```java
File file=new File("C:\\Users\\Administrator\\Desktop\\sqlearning");
String[] list= file.list((f,name)->
{
      Pattern pattern=Pattern.compile("[A-Za-z]*.[sjt][aqx][lrt]");
      Matcher matcher= pattern.matcher(name);
      return matcher.matches();
});
assert list!=null;for(String string:list) System.out.println(string);
```

 ##### 7.4.1 Path

新特性，以后再学。

 ##### 7.4.2 读写文件

新特性，以后再学。

 ##### 7.4.3 创建文件和目录

新特性，以后再学。

 ##### 7.4.4 复制、移动和删除文件

新特性，以后再学。

 ##### 7.4.5 获取文件信息

新特性，以后再学。

 ##### 7.4.6 访问目录中的项

新特性，以后再学。

 ##### 7.4.7 使用目录流

新特性，以后再学。

 ##### 7.4.8 ZIP文件系统

新特性，以后再学。

 #### 7.5 内存映射文件

 ##### 7.5.1 内存映射文件的性能

没必要，以后再学。

 ##### 7.5.2 缓冲区数据结构

缓冲区是由具有相同类型的数值构成的数组，Buffer类是一个抽象类，它有众多的具体子类，包括ByteBuffer、CharBuffer、DoubleBuffer、IntBuffer、LongBuffer和ShortBuffer。

注意：StringBuffer类与这些缓冲区没有关系

在实践中，最常用的将是ByteBuffer和Charbuffer。

每个缓冲区都具有:

1. 一个容量，它永远不能改变
2. 一个读写位置，下一个值将在此进行读写。
3. 一个界限，超过他进行读写时没有意义的。
4. 一个可选的标记，用于重复一个读入或者写出操作。

这些值满足下面的条件:
0<=标记<=读写位置<=界限<=容量

 #### 7.6 文件加锁机制

有空再看

 #### 7.7 正则表达式

 ##### 7.7.1正则表达式语法

常用的如下:

```java
1. [C1C2C3...Cn]  任何由C1/C2/C3.../Cn表示的字符 
	example  [0-9+-]//表示一个任意数字或加减符号
2. [^...]  某个字符类的补集
3. [...&...]  字符集的交集
4. \\d,\\D  数字[0-9],它的补集   tips(本来是\d因为java中需要转义所以用\\d)
5. \\w,\\W  单词字符[a-zA-Z],它的补集
   \\pL 匹配一个unicode字符
6. \\s,\\S  空白字符[\\n\\r\\t\\f\\x{B}],它的补集
7. XY  任何X中的字符串，后面跟随任何Y中的字符串
8. X|Y  任何X或Y中的字符串
9. X?  X出现0次或1次
10.X*  X出现0次或多次
11.X+  X出现1次或多次
12.X{n},X{n,},X{n,m}  连续n个X，连续n个或更多的X,连续n-m个X
13.Q? 其中Q是一个量词表达式 勉强量词，在尝试最长匹配之前先尝试最短匹配
14.Q+ 其中Q是一个量词表达式 占有量词，在不回溯的情况下获取最长匹配
```

 ##### 7.7.2 匹配字符串

一般用如下模板匹配:

```java
Pattern pattern = pattern.compile(patternString);
Matcher m = pattern.matcher(targetString);
if(m.matches()) ...
```

如果targetString与patternString匹配，matches方法返回true。

组的概念：

这个概念很重要，组是用括号划分的正则表达式，可以通过编号来引用组。 

组号从0开始，有几对小括号就表示有几个组，并且组可以嵌套，组号为0的表示整个表达式，组号为1的表示第一个组，依此类推。例如：A(B)C(D)E正则式中有三组，组0是ABCDE，组1是B，组2是D；
A((B)C)(D)E正则式中有四组：组0是ABCDE，组1是BC，组2是B；组3是C，组4是D。



Matcher类的一些匹配方法:

```java
boolean matches() //如果输入匹配模式，则返回true。
boolean find()//尝试查找与模式匹配的字符序列的下一个子序列。此方法从字符序列的开头开始，如果该方法的前一次调用成功了并且从那时开始匹配器没有被重置，则从以前匹配操作没有匹配到的第一个字符开始，即如果前一次找到与模式匹配的子序列则这次从这个子序列后开始查找。代码编写方式类似于iterator.next()。
int start()//返回当前匹配到的字符串在原目标字符串中的起始索引位置
int end()//返回当前匹配的字符串的最后一个字符在原目标字符串中的offset（偏移量）
String group()//返回匹配到的字符串，结合find函数使用。
-----------组的方法----------
int groupCount()//返回匹配其模式中组的数目，不包括第0组。
String group(int group)//返回前一次匹配操作期间指定的组所匹配的子序列。如果该匹配成功，但指定的组未能匹配字符序列的任何部分，则返回 null。
int start(int group)//返回前一次匹配操作期间指定的组所匹配的子序列的初始索引。
int end(int group)//返回前一次匹配操作期间指定的组所匹配的子序列的最后索引+1。
```


 ##### 7.7.3 找出多个匹配

常用模板如下:

```java
Pattern pattern = Pattern.compile(patternString);
Matcher m = pattern.matcher(targetString);
while(m.find())
{
	String temp=m.group();
	//对匹配到的temp进行操作
}

```

 ##### 7.7.4 用分隔符来分割

有时，需要将将输入按照匹配的分隔符断开，我们可以使用Pattern中的split方法。split方法返回一个字符串数组。

```java
String input=...;
Pattern commas = Pattern.compile("\\s*,\\s*");
String[] tokens=commas.split(input);
//"1, 2, 3" turns into {"1","2","3"}
```

 ##### 7.7.5 替换匹配

Matcher类中常见的替换匹配方法如下:

```java
String replace(char oldChar, char newChar)
          //返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 而生成的。
String replace(CharSequence target, CharSequence replacement)
          //使用指定的字面值替换序列替换此字符串匹配字面值目标序列的每个子字符串。
String replaceAll(String regex, String replacement)
          //使用给定的 replacement 字符串替换此字符串匹配给定的正则表达式的每个子字符串。
String replaceFirst(String regex, String replacement)
          //使用给定的 replacement 字符串替换此字符串匹配给定的正则表达式的第一个子字符串。
StringBuffer replace(int start, int end, String str)
          //使用给定 String 中的字符替换此序列的子字符串中的字符。
StringBuilder replace(int, int, java.lang.String)
          //使用给定 String 中的字符替换此序列的子字符串中的字符。
void Matcher.replaceAll(String replacement)
          //替换模式与给定替换字符串相匹配的输入序列的每个子序列。
void Matcher.replaceFirst(String replacement)
          //替换模式与给定替换字符串匹配的输入序列的第一个子序列。 
```

example:

```java
Pattern pattern = Pattern.compile("[0-9]+");
Matcher matcher = pattern.matcher(input);
String output = matcher.replaceAll("#");
//该方法把input中所有出现的pattern都换成了"#
```

 ### Chapter8:集合

 ### Chapter9:并发

 ### Chapter10:数据库编程

 ### Chapter11:网络

### 随记: 

#### 1.关于静态方法与静态变量的继承问题:

**2022.05.01**

在java中，子类可以继承父类的static变量和方法，但是不能重写。因为这是属于类本身的。但是子类是可以访问的。
子类和父类中同名的static变量和方法都是相互独立的，并不存在任何的重写的关系。

```java
class Basic
{
    public static void kkkk()
    {
        System.out.println("Basic");
    }
}
class Derived extends Basic
{
    public static void kkkk()
    {
        System.out.println("Devided");
    }
    public static void main(String[] args)
    {
        Basic test = new Derived();
        test.kkkk();
        Derived test2 = new Derived();
        test2.kkkk();
    }
}
/*
Basic
Devided
*/
```

可以看到，上述的例子并没有实现多态，因为在子类中的kkkk()方法与父类中的kkkk()方法是相互独立的。

同样的，一个类如果实现了一个接口，那么调用接口中的静态方法我们也不能够通过子类的类名来调用而应该通过接口名来调用。

#### 2.关于初始化顺序的问题

**2022.05.02**

##### 2.1 基础初始化顺序

首先，我们得知道，**在JAVA的构造函数中,this()和super()不能同时出现**

因为构造函数中调用其他构造函数的代码要写在第一行，如果同时写下了super()和this()，则两个语句都需要放在第一行，很显然不可能。

深层次的原因：

在java中，执行构造函数的机制是:

1. 如果显式地调用了构造函数(无论是父类的还是子类的构造函数)，则按照顺序执行被调用的构造函数。
2. 如果没有显式地调用构造函数，则程序在运行的时候会自动在构造函数的第一行加上调用父类无参构造函数(上一条的情况就不会加)，一直向上调用到Object的构造函数为止。

如果父类和子类中同时出现静态方法，那么构造顺序是什么样子的呢？

1. 父类静态成员变量初始化
2. 父类静态初始化块
3. 子类静态成员初始化
4. 子类静态初始化块
5. 父类非静态成员变量初始化
6. 父类初始化块
7. 父类构造器的构造方法
8. 子类非静态成员初始化
9. 子类初始化块
10. 子类构造方法

注：

1. 如果构造器第一行调用的是子类的其他构造器，则5 6 7 替换为 子类其他构造器的构造方法，其他顺序不变。
2. 如果没有在第一行声明其他构造器，则会默认调用父类的无参构造器！！！
3. 在没有静态方法的情况下，先找主函数。

##### 2.2 带main函数的初始化顺序

1. 首先，找到有public static void main(String[] args)的类

2. 执行这个类基类的静态初始化
3. 执行这个类的静态初始化
4. 执行main函数，按语句顺序执行
5. 如果在main函数中构造了新的对象，顺序按照上面基础初始化顺序来

#### 3.涉及基本类型的函数重载

**2022.05.09**

##### 小->大

首先，基本类型能从一个较小的类型自动提升至一个较大的类型

常数一般会被当成int类型 

浮点数一般会被当成double类型

特别的，如果无法恰好找到char类型参数的方法，就会把char直接提升到int。

下面是详细叙述:

* 对于int参数 ，

如果有接受int参数的方法，则调用之，

若只有比int小的类型参数，报错！

若存在比int大的类型参数则按照**long->float->double** 的顺序调用!

* 对于byte参数

如果有接受byte参数的方法，则调用之，

**若只有char类型参数，报错！**

若存在比byte大的类型参数则按照**short->int->long->float->double** 的顺序调用!

* 对于short参数

如果有接受short参数的方法，则调用之，

**若只有char类型或byte类型参数，报错！**

若存在比short大的类型参数则按照**int->long->float->double** 的顺序调用!

* 对于char参数

如果有接受char参数的方法，则调用之，

**若只有short类型或byte类型参数，则报错！**

若存在比char大的类型参数则按照**int->long->float->double** 的顺序调用!

**若short byte int同时存在，char只会转换成int类型!!!**

* 对于long参数

如果有接受long参数的方法，则调用之，

若只有比long小的类型参数，报错！

若存在比long大的类型参数则按照**float->double** 的顺序调用!

* 对于浮点数

如果浮点数没有以f作为结尾，在float参数和double参数方法同时存在的情况下，只会调用double参数的方法！

##### 大->小

这个不会自动转换，必须采用强制类型转换，如果不进行强制类型转换，则会报错。

#### 4.Java中的多态坑点

**2022.05.10**



##### 4.1.字段无法多态

在java中，只有普通的方法调用是多态的(动态绑定)，但是普通字段不多态，是在编译期就已经确定的。

```java
import static java.lang.System.out;
class Super
{
    public int a=1;
    public int getA()
    {
        return this.a;
    }
}
class Sub extends Super
{
    public int a=2;
    public int getSuperA()
    {
        return super.a;
    }
    public int getA()
    {
        return a;
    }

}
public class MyTest
{
    public static void main(String... args)
    {
        Super sup=new Sub();
        out.println("向上转型后 a ="+sup.a);
        out.println("向上转型后 getA() ="+sup.getA());
        Sub sub1=(Sub) sup;
        out.println("向下转型后 a ="+sub1.a);
        out.println("向下转型后 getA() ="+sub1.getA());
        out.println("向下转型后 getSuperA() ="+sub1.getSuperA());
        Sub sub=new Sub();
        out.println("不转型 a ="+sub.a);
        out.println("不转型 getA() ="+sub.getA());
        out.println("不转型 getSuperA() ="+sub.getSuperA());
    }
}
```

输出为

```java
/*
向上转型后 a =1
向上转型后 getA() =2
向下转型后 a =2
向下转型后 getA() =2
向下转型后 getSuperA() =1
不转型 a =2
不转型 getA() =2
不转型 getSuperA() =1
*/
```

##### 4.2.私有方法默认为final方法

私有方法不能被重写。

因此就会出现一个坑点：

在子类中可以写一个和父类中某个静态方法方法签名相同的方法。

在具体调用时，见下面例子

```java
import static java.lang.System.out;
class A
{
    private int f(int a, int b)
    {
        return a*b;
    }
    public int g(int a,int b)
    {
        return f(a,b);
    }
}
class B extends A
{
    public int f(int a,int b)
    {
        return a+b;
    }
}
public class MyTest
{
    public static void main(String... args)
    {
        A a=new B();
        B b = null;
        if(a.getClass()==B.class)
        {
            b=(B)a;
        }
        assert b != null;
        out.println(b.g(1,1));
        out.println(new B().f(2,3));
    }
}
/*
1
5
*/
```

##### 4.3.静态方法的行为不具备多态性

##### 4.4. 构造器的多态坑点

一般情况下，动态绑定的调用是在运行时才决定的，因为对象无法知道它是属于方法的所在的哪个类。

如果在构造器内部的一个动态绑定方法，一般是用到那个方法被覆盖后的定义。

但是如果在基类中的构造器调用了这个方法，他会绑定到子类上，但是这会出现一些初始化的问题。

```java
package Test12;
import static java.lang.System.out;
class Glyph
{
    void draw()
    {
        out.println("Glyph.draw()");
    }
    Glyph()
    {
        out.println("Glyph() before draw()");
        draw();
        out.println("Glyph() after draw()");
    }
}
class RoundGlyph extends Glyph
{
    private int radius = 1;
    RoundGlyph(int r)
    {
        radius = r;
        out.println("RoundGraph.RoundGlyph().radius = "+radius);
    }
    void draw()
    {
        out.println("RoundGraph.draw() , radius = "+radius);
    }
}
public class MyTest
{
    public static void main(String... args)
    {
        new RoundGlyph(5);
    }
}
/*
Glyph() before draw()
RoundGraph.draw() , radius = 0
Glyph() after draw()
RoundGraph.RoundGlyph().radius = 5
*/
```

我们会发现，由于基类构造器执行顺序优先，导致了radius并没有被初始化。

但是，为什么radius会是0呢？

因为在其他任何事物发生之前，将分配给对象的存储空间都会初始化成二级制的零。

#### 5.多参数重载决议原则

**2022.05.11**

根据传入参数进行全部同向偏序判断，如果不是全部同向偏序关系，

则会ambigous二义性报错，否则会调用离传入参数uml类图上距离最近的函数。

(若有一个能击败其他所有的函数的函数，则不考虑其他函数的二义性。）

```java
import static java.lang.System.out;

class A
{
}
class B extends A
{
}
class C extends B
{
}
public class MyTest
{
    static void f(A a,B b)
    {
        out.println("static void f(A a,B b)");
    }
    static void f(B a,A b) //这个和上面的存在二义性
    {
        out.println("static void f(A a,B b)");
    }
    static void f(B b,C a)//输出的是这个
    {
        out.println("static void f(B b,A a)");
    }
    public static void main(String... args)
    {
        f(new C(),new C());
    }
}

```

#### 6异常中的坑点

##### 6.1异常声明的原则

非检查型异常抛出后编译器会自动捕获(catch)并且自动打印栈顶轨迹。

检查型异常抛出后编译器不会自动执行上述过程，

我们要么在函数参数列表后面声明检查型异常，要么将其catch，当然最好两个都干。

如果检查型异常抛出后我们没有处理，事先也没有声明，那么就会报错。

##### 6.2重复抛出异常的例子

```java
class MyException extends Exception
{
    MyException(){super();};
    MyException(String mes){super(mes);}
}
class A
{
    public void f(String mes)throws MyException
    {
        try
        {
            System.err.println("f()抛出");
            throw new MyException(mes);
        }
        catch (MyException e)
        {
            System.err.println(1);
            e.printStackTrace();
            throw e;
        }
    }
    public void g()throws MyException
    {
        try
        {
            System.err.println("g()抛出");
            throw new MyException();
        }
        catch (MyException e)
        {
            e.printStackTrace();
            throw e;
        }
    }
    public void h()throws MyException
    {
        try
        {
            f("bit");
        }
        catch (MyException e)
        {
            System.err.println("抛出h()");
            e.printStackTrace();
            throw e;
        }

    }
}
public class MyTest
{

    public static void main(String... args)throws MyException
    {
        A a=new A();
        try
        {
            a.h();
        }
        catch (MyException r)
        {
            System.err.println("main fuc");
            r.printStackTrace();
        }
        try
        {
            a.g();
        }
        catch (MyException e)
        {
            System.err.println("main fuc");
            e.printStackTrace();
        }
    }
}
/*
f()抛出
1
Test19.MyException: bit
	at Test19.A.f(MyTest.java:19)
	at Test19.A.h(MyTest.java:45)
	at Test19.MyTest.main(MyTest.java:64)
抛出h()
Test19.MyException: bit
	at Test19.A.f(MyTest.java:19)
	at Test19.A.h(MyTest.java:45)
	at Test19.MyTest.main(MyTest.java:64)
main fuc
Test19.MyException: bit
	at Test19.A.f(MyTest.java:19)
	at Test19.A.h(MyTest.java:45)
	at Test19.MyTest.main(MyTest.java:64)
g()抛出
Test19.MyException
	at Test19.A.g(MyTest.java:33)
	at Test19.MyTest.main(MyTest.java:73)
main fuc
Test19.MyException
	at Test19.A.g(MyTest.java:33)
	at Test19.MyTest.main(MyTest.java:73)

Process finished with exit code 0
*/
```

**重抛异常会把异常抛给上一级的异常处理程序.**

##### 6.3异常链的处理问题

在Throwable的子类中，只有Error、Exceptioin、RuntimeException提供了带cause参数的构造器，其余的异常想要链接起来需要调用initCause()方法。

下面是我写的例子

```java
package Test20;

class MyException1 extends Exception
{
    MyException1(){super();}
    MyException1(String message) {super(message);}
    public String toString()
    {
        return "e1的toString";
    }
}
class MyException2 extends Exception
{
    MyException2(){super();}
    MyException2(String message){super(message);}
    public String toString()
    {
        return "e2的toString";
    }
}
class A
{
    private int i;
    void f(int i)
    {
        this.i=i;
        try
        {
            MyException1 e=new MyException1("E1");
            System.err.println("-----------"+i+"---------");
            e.initCause(new MyException2("E2"));
            throw e;
        }
        catch (MyException1 e)
        {
            System.err.println("抓住 "+e);
            e.printStackTrace();
        }
    }
}
public class MyTest
{
    public static void main(String... args)
    {
        A a = new A();
        a.f(3);
    }
}
/*
-----------3---------
抓住 e1的toString
e1的toString
	at Test20.A.f(MyTest.java:29)
	at Test20.MyTest.main(MyTest.java:46)
Caused by: e2的toString
	at Test20.A.f(MyTest.java:31)
	... 1 more
*/
```

##### 6.4异常丢失问题

**1. 如果在finally语句中重新抛出异常，那么异常可能会丢失!**

```java
package Test21;

class VeryImportantException extends Exception
{
    @Override
    public String toString()
    {
        return "A very important exception!";
    }
}
class HoHumException extends Exception
{
    @Override
    public String toString()
    {
        return "A trivial exception";
    }
}
public class MyTest
{
    public static void main(String... args)
    {
        try
        {
            try
            {
                throw new VeryImportantException();
            }
            finally {
                throw new HoHumException();
            }
        }
        catch (Exception e)
        {
            System.err.println(e);
        }
    }
}
/*
A trivial exception
*/
```

我们可以看到，在finally语句中抛出的HoHumException把VeryImportantException给盖掉了。

**2. 如果在finally语句中使用return,有可能会使异常被屏蔽**

```java
package Test22;

public class MyTest
{
    public static void main(String... args)
    {
        try
        {
            throw new RuntimeException();//runtimeException即使不捕获系统也会帮我们自动捕获
        }
        finally
        {
            
        }
    }
}
/*
Exception in thread "main" java.lang.RuntimeException
	at Test22.MyTest.main(MyTest.java:9)
*/
package Test22;

public class MyTest
{
    public static void main(String... args)
    {
        try
        {
            throw new RuntimeException();
        }
        
        finally
        {
            return;
        }
    }
}
/*无输出*/
```

##### 6.5继承中的异常问题

1. **接口不能增加在基类中的冲突方法的异常说明**

2. **派生类的构造器的异常说明必须包含基类的异常说明**

   注意：派生类构造器不能捕获基类抛出的异常

3. **对于普通的方法覆盖，派生类的异常说明必须比基类的异常说明要小(与构造器相反)**

   我们可以看出，在继承与覆盖的过程中，某个特定方法的"异常说明的接口"不是变大了反而是变小了——这恰好和类接口在继承时表现的行为相反。

4. **如果遇到了向上转型，那么必须处理父类方法中的更大的异常**

##### 6.6 处理构造阶段出现的异常坑点

如果构造失败，直接进行finally语句显然会出问题

这时我们应当使用嵌套的try语句

```java
public static void main(String... args)
    {
        try
        {
            BufferedReader bufferedReader=new BufferedReader(new FileReader("..."));
            try
            {

            }
            finally
            {
                bufferedReader.close();
            }
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
    }
```

#### 7.JAVA流处理的注意点

##### 7.1从字节流到字符流的转换

一般来说，从字节流到字符流的转换我们使用InputstreamReader和OutputStreamReader适配器。

InputStreamReader可以把InputStream转换为Reader

OutputStreamWriter可以把OutputStream转换为Writer

```java
BufferedReader br=new BufferedReader(new InputStreamReader(new FileInputStream("...")));
BufferedWriter bw=new BufferedWriter(new OutputStreaWriter(new FileOutputStream("...")));
```

##### 7.2装饰器类接口

FilterInputStream和FilterOutputStream是用来提供装饰器类接口以控制特定输入流和输出流的两个类。



其中FilterInputStream包括了

**DataInputStream**、

**BufferedInputStream**、

**LineNumberInputStream**、

**PushbackInputStream**



FilterOutputStream包括了

**DataOutputStream**、

**PrintStream**、

**BufferedOutputStream**

<font color="red">**Attention:  只有字节流支持被上述装饰器装饰!!! 字符流需要在被装饰后用InputStreamReader/OutputStreamWriter来转换 RandomAccess无法被装饰!!!**</font>

##### 7.3标准I/O

Java提供了标准I/O模型有三个:System.in,System.out,System.err

其中

**System.out是 PrintStream对象**

**System.err是 PrintStream对象**

**System.in 是 InputStream对象**

标准I/O可以进行重定向

运用

**System.setIn(  需要一个InputStream对象(或其子类)  )**

**System.setOut( 需要一个PrintStream对象)**

**System.setErr( 需要一个PrintStream对象)**

下面是例子

```java
import java.io.*;

public class MyTest
{
    public static void main(String... args)throws IOException
    {
        PrintStream console=System.out;
        try
        {
            BufferedInputStream in =new BufferedInputStream(new FileInputStream(new File("...")));
            PrintStream out = new PrintStream(new BufferedOutputStream(new FileOutputStream(new File("..."))));
            System.setIn(in);
            System.setOut(out);
            System.setErr(out);
            BufferedReader bufferedReader=new BufferedReader(new InputStreamReader(System.in));
            try
            {
                String s;
                while((s = bufferedReader.readLine())!=null)
                {
                    System.out.println(s);
                }
            }
            finally
            {
                out.close();
                in.close();
                bufferedReader.close();
            }
        }
        catch (IOException ioException)
        {
            ioException.printStackTrace();
        }
        finally
        {
            System.setOut(console);
        }
    }
}

```

#### 8.RTTI坑点

##### 8.1 JAVA加载类的步骤以及是否会加载某个类

在JAVA中，加载类的步骤主要分为三步：

1. 加载。这是由类加载器执行的。该步骤将查找字节码，并从这些字节码中创建一个class对象

2. 链接。在链接阶段将验证类中的字节码，为静态域分配存储空间，并且如果必须的话，将解析这个类创建的对其他所有类的所有引用。

3. 初始化。如果该类具有超类，则对其初始化，执行静态初始化和静态初始化块。

**<font color="red">注： 初始化被延迟到了对静态方法(构造器隐式的是静态的)或者非常数静态域进行首次引用时才执行</font>**

也就是说，如果一个static final的值是一个"编译期常量"，那么这个值不需要对类进行初始化就可以被读取。

example:

```java
package Test31;

class Initable
{
    static
    {
        System.out.println("Initializing Initable");
    }
}
public class MyTest
{
    static 
    {
        System.out.println("static statement");
    }
    public static void main(String[] args)throws Exception
    {
        Class initable1 = Initable.class;
        System.out.println("-----------分割线----------");
        Class initable2 = Class.forName("Test31.Initable");
    }
}
/*
static statement
-----------分割线----------
Initializing Initable
*/
```

我们可以发现，如果用.class来创建对class对象的引用时，不会自动地初始化该class。

如果用Class.forName()来创建对class对象的引用时，会自动地初始化该class。

#### 9.泛型中的坑点

##### 9.1 static 的方法无法访问泛型类的类型参数

##### 9.2 装饰器只有最后一层有效

在java中，装饰器模式和CPP中的混型比较相像，但是并不一样。

java中的装饰器只有最后一层能够生效，尽管我们好像可以套很多层上去。

```java
package Test34;

import java.util.Date;

class Basic
{
    private String value;
    public void set(String val)
    {
        this.value = val;
    }
    public String get()
    {
        return this.value;
    }
}

class Decorator extends Basic
{
    protected Basic basic;
    public Decorator(Basic basic)
    {
        this.basic=basic;
    }
    public void set(String val)
    {
        basic.set(val);
    }
    public String get()
    {
        return basic.get();
    }
}

class TimeStamped extends Decorator
{
    private final long timeStamp;
    public TimeStamped(Basic basic)
    {
        super(basic);
        timeStamp = new Date().getTime();
    }
    public long getStamp()
    {
        return timeStamp;
    }
}

class SerialNumbered extends Decorator
{
    private static long counter = 1;
    private final long serialNumber = counter ++ ;
    public SerialNumbered(Basic basic)
    {
        super(basic);
    }
    public long getSerialNumber()
    {
        return serialNumber;
    }
}

class Decoration
{
    public static void main(String... args)
    {
        TimeStamped t = new TimeStamped(new Basic());
        TimeStamped t2=new TimeStamped(new SerialNumbered(new Basic()));
        t2.getStamp();
        //t2.getSerialNumber();
        //不能使用，因为装饰器所产生的对象是最后被装饰的类型,SerialNumbered的特有方法已经被覆盖了。
        SerialNumbered s = new SerialNumbered(new Basic());
        SerialNumbered s2 = new SerialNumbered(new TimeStamped(new Basic()));
        s2.getSerialNumber();
        //s2.getStamp();
        //不能使用，原因同样是因为被覆盖了。
        //但是，之所以还能这么写，是因为new TimeStamped对象实例被向上转型成了Basic对象实例然后传进了构造器。
    }
}
```

如上述代码表述的一样，

**<font color="red">使用装饰器产生的对象类型是最后被装饰的类型</font>**，

也就是说，尽管可以添加很多层，但只有最后一层才是最终可视的类型。

#### 10.持有对象的坑点

##### 10.1 Arrays.asList方法

该方法的返回值是List< T >，但是有一个坑点，就是其底层是一个数组，如果直接使用他的返回值，那么就不能调用add()、delete()之类改变数组尺寸的方法。

##### 10.2 关于接口和类

###### 10.2.1 List

List是一个接口。

常用方法：

boolean contains(Object o):确定某个对象是否在列表中

boolean containsAll(Collection<?> C)

remove (Object o)

T remove(int index)

int indexOf(Object o)

List < T > subList(int fromIndex,int toIndex)

boolean retainAll(Collection< ? > C)

boolean add(T t)

void add(int index, T t)

boolean addAll(Collection<? extends T>)

boolean addAll(int index,Collection<? extends T>)

等方法



###### 10.2.2 LinkedList

LinkedList 是一个实现了LIst接口和Deque接口的类。//Deque接口继承了Queue接口



###### 10.2.3 stack

Stack是一个类，继承自Vector

常见方法:T peak()、T pop()、push(T t)、boolean empty()

###### 

###### 10.2.4 Queue

Queue是一个接口

常见方法：boolean offer(T t)、T peek()、T element()、T poll()、T remove()

//offer方法插入元素到队尾。peek()和element返回队头,poll()和remove()移除并返回队头。其中peek和poll在队列为空时返回null,element和remove在队列为空时抛出NoSuchElementException异常。



 

##### 10.3关于迭代器

迭代器模板：

1. 使用方法iterator()要求容器返回一个Iterator，Iterator将准备好返回序列的第一个元素
2. 使用next()获得序列的下一个元素
3. 使用hasNext()检查序列中是否还有元素
4. 使用remove()将迭代器新近返回的元素删除 //调用remove()之前必须先调用next()

```java
Iterator< String > it = stringList.iterator();
while(it.hasNext)
{
	String string = it.next();
	//对string进行操作
}
```



#### 11.内部类坑点

##### 11.1内部类的继承问题

1. 必须传一个外围类的引用去初始化内部类的子类

2. 而且这个外围类的引用必须要调父类构造器

```java
package Test37;
class WithInner
{
    WithInner()
    {
        
    }
    class Inner
    {
        Inner()
        {
            
        }
    }
}

class InnerInherit extends WithInner.Inner
{
    InnerInherit(WithInner withInner)
    {
        withInner.super();
    }
    public static void main(String ... args)
    {
        WithInner withInner = new WithInner();
        WithInner.Inner inner = new WithInner().new Inner();
        InnerInherit innerInherit = new InnerInherit(withInner);
    }
}
```

其实也很好理解，因为继承了内部类的话，内部类如果非静态是需要外围类的引用的，所以需要传一个外围类的引用类型变量进去充当外围类的引用。



## Java设计模式

设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。

使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。 毫无疑问，设计模式于己于他人于系统都是多赢的，设计模式使代码编制真正工程化，设计模式是软件工程的基石，如同大厦的一块块砖石一样。项目中合理的运用设计模式可以完美的解决很多问题，每种模式在现在中都有相应的原理来与之对应，每一个模式描述了一个在我们周围不断重复发生的问题，以及该问题的核心解决方案，这也是它能被广泛应用的原因。简单说：

模式：在某些场景下，针对某类问题的某种通用的解决方案。

场景：项目所在的环境

问题：约束条件，项目目标等

解决方案：通用、可复用的设计，解决约束达到目标。



### 设计模式三个分类

**创建型模式**：对象实例化的模式，创建型模式用于解耦对象的实例化过程。

**结构型模式**：把类或对象结合在一起形成一个更大的结构。

**行为型模式**：类和对象如何交互，及划分责任和算法。



![aefc6eb7f5ba13216d5f21051327816e](.\image\Design_Pattern\aefc6eb7f5ba13216d5f21051327816e.png)



**各个模式之间的关系**：

![](.\image\Design_Pattern\98a09316240cb0dff94f39874a392657.jpg)

### 设计模式六大原则

**1、开闭原则（Open Close Principle）**

开闭原则就是说对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。



详细介绍：https://blog.csdn.net/lovelion/article/details/7537584

 

**2、里氏代换原则（Liskov Substitution Principle）**

里氏代换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。 LSP是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。



详细介绍：https://blog.csdn.net/lovelion/article/details/7540445



**3、依赖倒置原则（Dependence Inversion Principle）**

这个是开闭原则的基础，具体内容：真对接口编程，依赖于抽象而不依赖于具体。



详解介绍：https://blog.csdn.net/lovelion/article/details/7562783



**4、接口隔离原则（Interface Segregation Principle）**

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。还是一个降低类之间的耦合度的意思，从这儿我们看出，其实设计模式就是一个软件的设计思想，从大型软件架构出发，为了升级和维护方便。所以上文中多次出现：降低依赖，降低耦合。



详细介绍：https://blog.csdn.net/lovelion/article/details/7562842



**5、迪米特法则（最少知道原则）（Demeter Principle）**

为什么叫最少知道原则，就是说：一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。



详细介绍：https://blog.csdn.net/lovelion/article/details/7563445



**6、单一职责原则（Single-Responsibility-Principle）**

核心：一个类只负责一个功能领域中相应的职责，或者可以定义为：就一个类而言，应该只有一个引起它变化的原因。 

思想：如果一个类承担的职责过多，就等于把这些职责耦合在一起，一个职责的变化可能会削弱或者抑制这个类完成其他职责的能力。这种耦合会导致脆弱的设计，当变化发生时，设计会遭受到意想不到的破坏。



详细介绍：https://blog.csdn.net/lovelion/article/details/7536542



### 1. 工厂方法模式

定义了一个创建对象的接口，但由子类决定要实例化哪个类。工厂方法把实例化操作推迟到子类。

![image-20221031092451805](.\image\Design_Pattern\image-20221031092451805.png)



工厂模式（Factory Pattern）的意义就跟它的名字一样，在面向对象程序设计中，工厂通常是一个用来创建其他对象的对象。工厂模式根据不同的参数来实现不同的分配方案和创建对象。

在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。


**具体应用:**

![image-20221031092934462](.\image\Design_Pattern\image-20221031092934462.png)



```java
// 二者共同的接口
interface Human{
    public void eat();
    public void sleep();
    public void beat();
}

// 创建实现类 Male
class Male implements Human{
    public void eat(){
        System.out.println("Male can eat.");
    }
    public void sleep(){
        System.out.println("Male can sleep.");
    }
    public void beat(){
        System.out.println("Male can beat.");
    }
}
//创建实现类 Female
class Female implements Human{
    public void eat(){
        System.out.println("Female can eat.");
    }
    public void sleep(){
        System.out.println("Female can sleep.");
    }
    public void beat(){
        System.out.println("Female can beat.");
    }
}

// 创建普通工厂类
class HumanFactory{
    public Human createHuman(String gender){
        if( gender.equals("male") ){
           return new Male();
        }else if( gender.equals("female")){
           return new Female();
        }else {
            System.out.println("请输入正确的类型！");
            return null;
        }
    }
}

// 工厂测试类
public class FactoryTest {
    public static void main(String[] args){
        HumanFactory factory = new HumanFactory();
        Human male = factory.createHuman("male");
        male.eat();
        male.sleep();
        male.beat();
    }
}

```



### 2.抽象工厂模式

抽象工厂模式（Abstract Factory Pattern）是一种软件开发设计模式。抽象工厂模式提供了一种方式，可以将一组具有同一主题的单独的工厂封装起来。如果比较抽象工厂模式和工厂模式，我们不难发现前者只是在工厂模式之上增加了一层抽象的概念。抽象工厂是一个父类工厂，可以创建其它工厂类。所以我们也叫它 “工厂的工厂”。
![image-20221031093344631](.\image\Design_Pattern\image-20221031093344631.png)



抽象工厂模式特别适合于这样的一种产品结构：产品分为几个系列，在每个系列中，产品的布局都是类似的，在一个系列中某个位置的产品，在另一个系列中一定有一个对应的产品。这样的产品结构是存在的，这几个系列中同一位置的产品可能是互斥的，它们是针对不同客户的解决方案，每个客户都只选择其一。

```java
public class AbstractProductA {
}
public class AbstractProductB {
}
public class ProductA1 extends AbstractProductA {
}
public class ProductA2 extends AbstractProductA {
}
public class ProductB1 extends AbstractProductB {
}
public class ProductB2 extends AbstractProductB {
}
public abstract class AbstractFactory {
    abstract AbstractProductA createProductA();
    abstract AbstractProductB createProductB();
}
public class ConcreteFactory1 extends AbstractFactory {
    AbstractProductA createProductA() {
        return new ProductA1();
    }

    AbstractProductB createProductB() {
        return new ProductB1();
    }
}
public class ConcreteFactory2 extends AbstractFactory {
    AbstractProductA createProductA() {
        return new ProductA2();
    }

    AbstractProductB createProductB() {
        return new ProductB2();
    }
}


public class Client {
    public static void main(String[] args) {
        AbstractFactory abstractFactory = new ConcreteFactory1();
      // 抽象出来的父类工厂,因为concreteFactory1与2的行为类似，如果单独使用concreateFactory1/2，那么就是工厂方法模式。
        AbstractProductA productA = abstractFactory.createProductA();
        AbstractProductB productB = abstractFactory.createProductB();
        // do something with productA and productB
    }
}

```



**对比工厂方法模式与抽象工厂模式**

先介绍两个概念：

* 产品等级结构：比如一个抽象类是食物，其子类有苹果、牛奶等等，则抽象食物与具体食物名称之间构成了一个产品等级结构。食物是抽象的父类，而具体的食物名称是其子类。

* 产品族：在抽象工厂模式中，产品族是指由同一个工厂生产的，位于不同产品等级结构中的一组产品。如 AKitchen 生产的苹果、刀子，苹果属于食物产品等级结构中，而刀子则属于餐具产品等级结构中。而 BKitchen 可能生成另一组产品，如牛奶、杯子。



因此工厂方法模式、抽象工厂模式最大的区别在于：

​	工厂方法模式：针对的是 一个产品等级结构。

​	抽象工厂模式：针对多个产品等级结构，这些产品等级结构的布局类似，可以抽象为一个父类工厂。



### 3 单例模式

确保一个类只有一个实例，并提供该实例的全局访问点。

使用一个私有构造函数、一个私有静态变量以及一个公有静态函数来实现。

私有构造函数保证了不能通过构造函数来创建对象实例，只能通过公有静态函数返回唯一的私有静态变量。

![image-20221031100119060](.\image\Design_Pattern\image-20221031100119060.png)



**1.懒汉式-线程不安全**

以下实现中，私有静态变量 uniqueInstance 被延迟实例化，这样做的好处是，如果没有用到该类，那么就不会实例化 uniqueInstance，从而节约资源。

这个实现在多线程环境下是不安全的，如果多个线程能够同时进入 if (uniqueInstance == null) ，并且此时 uniqueInstance 为 null，那么会有多个线程执行 uniqueInstance = new Singleton(); 语句，这将导致实例化多次 uniqueInstance。

```java
public class Singleton {

    private static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}

```



**2.饿汉式-线程安全**

线程不安全问题主要是由于 uniqueInstance 被实例化多次，采取直接实例化 uniqueInstance 的方式就不会产生线程不安全问题。

但是直接实例化的方式也丢失了延迟实例化带来的节约资源的好处。

```java
private static Singleton uniqueInstance = new Singleton();
```



**3.懒汉式-线程安全**

只需要对 getUniqueInstance() 方法加锁，那么在一个时间点只能有一个线程能够进入该方法，从而避免了实例化多次 uniqueInstance。

但是当一个线程进入该方法之后，其它试图进入该方法的线程都必须等待，即使 uniqueInstance 已经被实例化了。这会让线程阻塞时间过长，因此该方法有性能问题，不推荐使用。

```java
public static synchronized Singleton getUniqueInstance() {
    if (uniqueInstance == null) {
        uniqueInstance = new Singleton();
    }
    return uniqueInstance;
}

```

**4.双重校验锁-线程安全**
uniqueInstance 只需要被实例化一次，之后就可以直接使用了。加锁操作只需要对实例化那部分的代码进行，只有当 uniqueInstance 没有被实例化时，才需要进行加锁。

双重校验锁先判断 uniqueInstance 是否已经被实例化，如果没有被实例化，那么才对实例化语句进行加锁。

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```



**5.静态内部类实现**

当 Singleton 类被加载时，静态内部类 SingletonHolder 没有被加载进内存。只有当调用 getUniqueInstance() 方法从而触发 SingletonHolder.INSTANCE 时 SingletonHolder 才会被加载，此时初始化 INSTANCE 实例，并且 JVM 能确保 INSTANCE 只被实例化一次。

这种方式不仅具有延迟初始化的好处，而且由 JVM 提供了对线程安全的支持。

```java
public class Singleton {

    private Singleton() {
    }

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getUniqueInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```



**6.枚举实现**

```java
public enum Singleton {

    INSTANCE;

    private String objName;


    public String getObjName() {
        return objName;
    }


    public void setObjName(String objName) {
        this.objName = objName;
    }


    public static void main(String[] args) {

        // 单例测试
        Singleton firstSingleton = Singleton.INSTANCE;
        firstSingleton.setObjName("firstName");
        System.out.println(firstSingleton.getObjName());
        Singleton secondSingleton = Singleton.INSTANCE;
        secondSingleton.setObjName("secondName");
        System.out.println(firstSingleton.getObjName());
        System.out.println(secondSingleton.getObjName());

        // 反射获取实例测试
        try {
            Singleton[] enumConstants = Singleton.class.getEnumConstants();
            for (Singleton enumConstant : enumConstants) {
                System.out.println(enumConstant.getObjName());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



### 4 适配器模式

**定义**： 适配器模式将某个类的接口转换成客户端期望的另一个接口表示，目的是消除由于接口不匹配所造成的类的兼容性问题。

在适配器模式中，我们通过增加一个新的适配器类来解决接口不兼容的问题，使得原本没有任何关系的类可以协同工作。



**类别**：主要分为三类：

1. 类适配器模式
2. 对象适配器模式
3. 缺省适配器模式（接口适配器模式）



**类适配器模式**

通过多重继承目标接口和被适配者类方式来实现适配。

![image-20221112161150705](.\image\Design_Pattern\image-20221112161150705.png)



具体例子：

将USB接口转换为VGA接口

![image-20221112161428023](.\image\Design_Pattern\image-20221112161428023.png)

```java

public class USBImpl implements USB{
       @Override
       public void showPPT() {
              // TODO Auto-generated method stub
              System.out.println("PPT内容演示");
       }
}
//AdatperUSB2VGA 首先继承USBImpl获取USB的功能，其次，实现VGA接口，表示该类的类型为VGA。
public class AdapterUSB2VGA extends USBImpl implements VGA {
       @Override
       public void projection() {
              super.showPPT();
       }
}
//Projector将USB映射为VGA，只有VGA接口才可以连接上投影仪进行投影
public class Projector<T> {
       public void projection(T t) {
              if (t instanceof VGA) {
                     System.out.println("开始投影");
                     VGA v = new VGAImpl();
                     v = (VGA) t;
                     v.projection();
              } else {
                     System.out.println("接口不匹配，无法投影");
              }
       }
}
//测试代码
       @Test
       public void test2(){
              //通过适配器创建一个VGA对象，这个适配器实际是使用的是USB的showPPT（）方法
              VGA a=new AdapterUSB2VGA();
              //进行投影
              Projector p1=new Projector();
              p1.projection(a);
       } 

```



**对象适配器模式**

对象适配器和类适配器使用了不同的方法实现适配，对象适配器使用组合，类适配器使用继承。

![image-20221112161728111](.\image\Design_Pattern\image-20221112161728111.png)



具体例子:

![image-20221112161746020](.\image\Design_Pattern\image-20221112161746020.png)

```java
public class AdapterUSB2VGA implements VGA {
       USB u = new USBImpl();
       @Override
       public void projection() {
              u.showPPT();
       }
}
//实现VGA接口，表示适配器类是VGA类型的，适配器方法中直接使用USB对象。
```



**缺省适配器模式**

当不需要全部实现接口提供的方法时，可先设计一个抽象类实现接口，并为该接口中每个方法提供一个默认实现（空方法），那么该抽象类的子类可有选择地覆盖父类的某些方法来实现需求，它适用于一个接口不想使用其所有的方法的情况。

具体例子：

![image-20221112161923330](.\image\Design_Pattern\image-20221112161923330.png)

```java
public abstract class AdapterUSB2VGA implements VGA {
       USB u = new USBImpl();
       @Override
       public void projection() {
              u.showPPT();
       }
       @Override
       public void b() {
       };
       @Override
       public void c() {
       };
}
//AdapterUSB2VGA实现，不用去实现b()和c()方法。
public class AdapterUSB2VGAImpl extends AdapterUSB2VGA {
       public void projection() {
              super.projection();
       }
}
```



**总结：**

类适配器模式：当希望将一个类转换成满足另一个新接口的类时，可以使用类的适配器模式，创建一个新类，继承原有的类，实现新的接口即可。

对象适配器模式：当希望将一个对象转换成满足另一个新接口的对象时，可以创建一个Wrapper类，持有原类的一个实例，在Wrapper类的方法中，调用实例的方法就行。

缺省适配器模式：当不希望实现一个接口中所有的方法时，可以创建一个抽象类Wrapper，实现所有方法，我们写别的类的时候，继承抽象类即可。



**使用选择：**

根据合成复用原则，组合大于继承。因此，类的适配器模式应该少用。



### 5 装饰者模式

**定义：**动态的将新功能附加到对象上。在对象功能扩展方面，它比继承更有弹性。

**被装饰对象和修饰者继承自同一个超类**



![image-20221112164811029](.\image\Design_Pattern\image-20221112164811029.png)

**抽象组件（Component）：** 可以是一个接口或者抽象类，其充当被装饰类的原始对象，规定了被装饰对象的行为

**具体组件（ConcreteComponent）： **实现/继承Component的一个具体对象，也即被装饰对象

**抽象装饰器（Decorator）：** 通用的装饰ConcreteComponent的装饰器，其内部必然有一个属性指向Component抽象组件；其实现一般是一个抽象类，主要是为了让其子类按照其构造形式传入一个Component抽象组件，这是强制的通用行为（当然，如果系统中逻辑单一，并不需要实现许多装饰器，那么我们可以直接省略该类，而直接实现一个具体装饰器（ComcreteDecorator）即可）

**具体装饰器（ConcreteDecorator）：** Decorator的具体实现类，理论上，每个ConcreteDecorator都扩展了Component对象的一种功能



```java
//创建一个抽象组件Component来规定被装饰对象的行为
public abstract class Component 
{
    public abstract void execute();
}
public class ConcreteComponent extends Component 
{
    @Override
    public void execute() {
        System.out.println("具体组件处理业务逻辑");
    }

}
//创建具体组件ConcreteComponent
public abstract class Decorator extends Component 
{
    public Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    public void execute() {
        component.execute();
    }
}
//创建一个抽象装饰器Decorator
public class ConcreteDecorator extends Decorator 
{
    public ConcreteDecorator(Component component) {
        super(component);
    }
    public void before(){
        System.out.println("ConcreteDecorator前置操作....");
    }
    public void after(){
        System.out.println("ConcreteDecorator后置操作....");
    }
    public void execute() {
        before();
        component.execute();
        after();
    }
}


//测试
public class Test 
{
    public static void main(String[] args) 
    {
        //创建需要被装饰的组件
        Component component = new ConcreteComponent();
        //给对象透明的增加功能并调用
        Decorator decorator = new ConcreteDecorator(component);
        decorator.execute();
    }
}

```



具体实例：

![image-20221112165839411](.\image\Design_Pattern\image-20221112165839411.png)

注意：上图中Decorator对Drink既是继承关系也是组合关系





**总结：**

一般来说，我认为的装饰器模式的实现方法如下：

1. 装饰器和被装饰者继承同一个父类
2. 装饰器要将父类组合起来，作为其数据成员
3. 装饰器将父类成员在构造方法中进行初始化(从而实现可以把被装饰者作为参数传进来然后操作被装饰者)



**装饰器模式优缺点分析：**

**优点：**
装饰器是继承的有力补充，比继承灵活，不改变原有对象的情况下动态地给一个对象扩展功能，即插即用
通过使用不同装饰类以及这些装饰类的排列组合，可以实现不同效果
装饰器完全遵守开闭原则

**缺点：**
从代码层面来看，使用装饰器模式会出现更多的代码，更多的类，增加程序复杂性
动态装饰时，多层装饰时会更复杂



### 6 代理模式

**定义：**代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。通俗的来讲代理模式就是我们生活中常见的中介。

![image-20221113084614439](.\image\Design_Pattern\image-20221113084614439.png)

![image-20221113084445319](.\image\Design_Pattern\image-20221113084445319.png)

**为什么要使用代理模式:**

**中介隔离作用：**在某些情况下，一个客户类不想或者不能直接引用一个委托对象，而代理类对象可以在客户类和委托对象之间起到中介的作用，其特征是代理类和委托类实现相同的接口。

**开闭原则，增加功能：**代理类除了是客户类和委托类的中介之外，我们还可以通过给代理类增加额外的功能来扩展委托类的功能，这样做我们只需要修改代理类而不需要再修改委托类，符合代码设计的开闭原则。代理类主要负责为委托类预处理消息、过滤消息、把消息转发给委托类，以及事后对返回结果的处理等。代理类本身并不真正实现服务，而是同过调用委托类的相关方法，来提供特定的服务。真正的业务功能还是由委托类来实现，但是可以在业务功能执行的前后加入一些公共的服务。例如我们想给项目加入缓存、日志这些功能，我们就可以使用代理类来完成，而没必要打开已经封装好的委托类。



具体例子1：

![image-20221113085804904](.\image\Design_Pattern\image-20221113085804904.png)



具体例子2：

![image-20221113085851509](.\image\Design_Pattern\image-20221113085851509.png)

```java
public interface BuyHouse 
{
    void buyHosue();
}

public class BuyHouseImpl implements BuyHouse 
{
       @Override
       public void buyHosue() {
              System.out.println("我要买房");
       }
}

public class BuyHouseProxy implements BuyHouse {
       private BuyHouse buyHouse;
       public BuyHouseProxy(final BuyHouse buyHouse) {
              this.buyHouse = buyHouse;
       }
       @Override
       public void buyHosue() {
              System.out.println("买房前准备");
              buyHouse.buyHosue();
              System.out.println("买房后装修");
       }
}
```



**总结:**

优点：可以做到在符合开闭原则的情况下对目标对象进行功能扩展。

缺点： **代理对象与目标对象要实现相同的接口，我们得为每一个服务都得创建代理类，工作量太大**，不易管理。同时接口一旦发生改变，代理类也得相应修改。 



**我理解的代理模式：**

1.代理类与委托类同时实现了相同的接口

2.代理类会组合接口作为其数据成员来引用委托类对象

3.我们可以通过对代理类为委托类增加许多功能，比如数据预处理等，而不需要改变委托类的代码。



### 7 策略模式

**定义：** 策略模式定义了一系列算法，并将每个算法封装起来，使他们可以相互替换，且算法的变化不会影响到使用算法的客户。

**意图：**定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。

**主要解决：**在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。

**何时使用：**一个系统有许多许多类，而区分它们的只是他们直接的行为。

**如何解决：**将这些算法封装成一个一个的类，任意地替换。

**关键代码：**实现同一个接口。

**类图：**

![image-20221113095502285](.\image\Design_Pattern\image-20221113095502285.png)

![image-20221113092707082](.\image\Design_Pattern\image-20221113092707082.png)



具体例子：

![image-20221113095430464](.\image\Design_Pattern\image-20221113095430464.png)

加减法例子:

![image-20221113095609746](.\image\Design_Pattern\image-20221113095609746.png)

```java
public interface Strategy 
{
	public int calc(int num1,int num2);
}

public class AddStrategy implements Strategy 
{
	@Override
	public int calc(int num1, int num2) {
		// TODO Auto-generated method stub
		return num1 + num2;
	}
}

public class SubstractStrategy implements Strategy 
{
	@Override
	public int calc(int num1, int num2) {
		// TODO Auto-generated method stub
		return num1 - num2;
	}
}

public class Environment 
{
	private Strategy strategy;
	public Environment(Strategy strategy) 
  {
		this.strategy = strategy;
	}
	public int calculate(int a, int b) 
  {
		return strategy.calc(a, b);
	}
}

```



**策略模式优缺点：**

**优点：** 1、算法可以自由切换。 2、避免使用多重条件判断。 3、扩展性良好。

**缺点：** 1、策略类会增多。 2、所有策略类都需要对外暴露。



**我理解的策略模式：**

1.在需要使用策略的类中组合一个策略接口

2.为策略接口定义抽象方法

3.具体策略类实现策略接口

4.使用多态引用具体策略对象



### 8 观察者模式

**定义：** 定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

**主要解决：**一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。

**何时使用：**一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。

**如何解决：**使用面向对象技术，可以将这种依赖关系弱化。

**关键代码：**在抽象类里有一个 ArrayList 存放观察者们。

![image-20221113100732850](.\image\Design_Pattern\image-20221113100732850.png)

![image-20221113101803783](.\image\Design_Pattern\image-20221113101803783.png)



具体实例：

气象站设计

![image-20221113102207984](.\image\Design_Pattern\image-20221113102207984.png)

![image-20221113101748146](.\image\Design_Pattern\image-20221113101748146.png)

**观察者模式优缺点：**

**优点：** 1、观察者和被观察者是抽象耦合的。 2、建立一套触发机制。

**缺点： **1、如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。 2、如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。 3、观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。





**我理解的观察者模式:**

1.subject接口需要维护一个动态的ArrayList用来存放每一个观察者，同时需要定义三个抽象方法registerObserver、removeObserver和notifyObserver。

2.Observer接口需要定义一个抽象方法update()

3.具体subject需要实现三个抽象方法，其中notifyObserver需要遍历ArrayList，并执行每一个容器内的Observer对象的update方法。

4.所有具体Observer都需要引用一个具体subject，用于登记，并且需要实现update()方法，用于观察者被通知后的执行何种操作。



### 9 访问者模式

**定义：**将作用于某种数据结构中的各元素的操作分离出来封装成独立的类，使其在不改变数据结构的前提下可以添加作用于这些元素的新的操作，为数据结构中的每个元素提供多种访问方式。它将对数据的操作与数据结构进行分离。

**访问者模式角色：**

抽象访问者（Visitor）角色：定义一个访问具体元素的接口，为每个具体元素类对应一个访问操作 visit() ，该操作中的参数类型标识了被访问的具体元素。

具体访问者（ConcreteVisitor）角色：实现抽象访问者角色中声明的各个访问操作，确定访问者访问一个元素时该做什么。

抽象元素（Element）角色：声明一个包含接受操作 accept() 的接口，被接受的访问者对象作为 accept() 方法的参数。

具体元素（ConcreteElement）角色：实现抽象元素角色提供的 accept() 操作，其方法体通常都是 visitor.visit(this) ，另外具体元素中可能还包含本身业务逻辑的相关操作。

对象结构（Object Structure）角色：是一个包含元素角色的容器，提供让访问者对象遍历容器中的所有元素的方法，通常由 List、Set、Map 等聚合类实现。

![image-20221113162023809](.\image\Design_Pattern\image-20221113162023809.png)



![image-20221113103447769](.\image\Design_Pattern\image-20221113103447769.png)

```java
public interface Visitor 
{
	abstract public void Visit(Element element);
}
public class CompensationVisitor implements Visitor 
{
	@Override
	public void Visit(Element element) {
		// TODO Auto-generated method stub
		Employee employee = ((Employee) element);
 
		System.out.println(
				employee.getName() + "'s Compensation is " + (employee.getDegree() * employee.getVacationDays() * 10));
	}
 
}
public interface Element 
{
	abstract public void Accept(Visitor visitor);
 
}
public class CompensationVisitor implements Visitor 
{
	@Override
	public void Visit(Element element) {
		// TODO Auto-generated method stub
		Employee employee = ((Employee) element);
 
		System.out.println(
				employee.getName() + "'s Compensation is " + (employee.getDegree() * employee.getVacationDays() * 10));
	}
}
public class ObjectStructure 
{
	private HashMap<String, Employee> employees;
 
	public ObjectStructure() {
		employees = new HashMap();
	}
 
	public void Attach(Employee employee) {
		employees.put(employee.getName(), employee);
	}
 
	public void Detach(Employee employee) {
		employees.remove(employee);
	}
 
	public Employee getEmployee(String name) {
		return employees.get(name);
	}
 
	public void Accept(Visitor visitor) {
		for (Employee e : employees.values()) {
			e.Accept(visitor);
		}
	}
 
}
```



**访问者模式优缺点：**

访问者（Visitor）模式是一种对象行为型模式，其主要优点如下。

1.扩展性好。能够在不修改对象结构中的元素的情况下，为对象结构中的元素添加新的功能。

2.复用性好。可以通过访问者来定义整个对象结构通用的功能，从而提高系统的复用程度。

3.灵活性好。访问者模式将数据结构与作用于结构上的操作解耦，使得操作集合可相对自由地演化而不影响系统的数据结构。

4.符合单一职责原则。访问者模式把相关的行为封装在一起，构成一个访问者，使每一个访问者的功能都比较单一。

访问者（Visitor）模式的主要缺点如下。

1.增加新的元素类很困难。在访问者模式中，每增加一个新的元素类，都要在每一个具体访问者类中增加相应的具体操作，这违背了“开闭原则”。

2.破坏封装。访问者模式中具体元素对访问者公布细节，这破坏了对象的封装性。

3.违反了依赖倒置原则。访问者模式依赖了具体类，而没有依赖抽象类。



**我理解的访问者模式：**

1.首先定义抽象visitor类。抽象vistor中需要定义多个抽象方法visit (concreteElement )，从而实现对不同元素的不同访问。

2.再定义抽象Element类。抽象Element中需要定义多个抽象方法accept(concreteVistor),通过多态机制来限制不同访问者的访问权限。

3.再定义concreteElement类。需要继承抽象Element类实现accept方法。一般来说，accept方法体都是vistor.visit(this)，把this引用传给vistor。同时，我们需要定义一系列操作供vistor使用。

4.再定义concreteVistor类，我们需要具体实现方法visit。一般在visit方法中，我们会对传进来的Element引用进行一系列操作。这些操作都是Element自己定义的。

5.最后，我们会维护一个对象结构，独显结构中一般会放一个元素列表用于存放所有元素。

6.通过调用concreteElement的accept方法，将visitor引用传进去就能实现访问。



### 10 MVC模式

**定义：**MVC，即 Model 模型、View 视图，及 Controller 控制器。

**View：**视图，为用户提供使用界面，与用户直接进行交互。

**Model：**模型，承载数据，并对用户提交请求进行计算的模块。

其分为两类：

一类称为数据承载 Bean：实体类，专门用户承载业务数据的，如 Student、User 等 

一类称为业务处理 Bean：指 Service 或 Dao 对象，专门用于处理用户提交请求的。

**Controller：**控制器，用于将用户请求转发给相应的 Model 进行处理，并根据 Model 的计算结果向用户提供相应响应。

![image-20221113164323874](.\image\Design_Pattern\image-20221113164323874.png)



**MVC 架构程序的工作流程：**
（1）用户通过 View 页面向服务端提出请求，可以是表单请求、超链接请求、AJAX 请求等
（2）服务端 Controller 控制器接收到请求后对请求进行解析，找到相应的 Model 对用户请求进行处理
（3）Model 处理后，将处理结果再交给 Controller
（4）Controller 在接到处理结果后，根据处理结果找到要作为向客户端发回的响应 View 页面。页面经渲染（数据填充）后，再发送给客户端。



**常见的MVC架构1:**

JSP作为View

Javabean作为model

servlet作为controller

![image-20221113165710389](.\image\Design_Pattern\image-20221113165710389.png)

**常见的MVC架构2:**

![image-20221113165825018](.\image\Design_Pattern\image-20221113165825018.png)

**MVC模式优缺点**

**优点**
**1 低耦合**
通过将视图层和业务层分离，允许更改视图层代码而不必重新编译模型和控制器代码，同样，一个应用的业务流程或者业务规则的改变，只需要改动MVC的模型层（及控制器）即可。因为模型与控制器和视图相分离，所以很容易改变应用程序的数据层和业务规则。

模型层是自包含的，并且与控制器和视图层相分离，所以很容易改变应用程序的数据层和业务规则。如果想把数据库从 MySQL 移植到 Oracle，或者改变基于 RDBMS 的数据源到 LDAP，只需改变模型层即可。一旦正确的实现了模型层，不管数据来自数据库或是 LDAP服务器，视图层都将会正确的显示它们。由于运用 MVC 的应用程序的三个部件是相互独立，改变其中一个部件并不会影响其它两个，所以依据这种设计思想能构造出良好的松耦合的构件。

**2 重用性高**
随着技术的不断进步，当前需要使用越来越多的方式来访问应用程序了。MVC模式允许使用各种不同样式的视图来访问同一个服务端的代码，这得益于多个视图（如WEB（HTTP）浏览器或者无线浏览器（WAP））能共享一个模型。

比如，用户可以通过电脑或通过手机来订购某样产品，虽然订购的方式不一样，但处理订购产品的方式（流程）是一样的。由于模型返回的数据没有进行格式化，所以同样的构件能被不同的界面（视图）使用。例如，很多数据可能用 HTML 来表示，但是也有可能用 WAP 来表示，而这些表示的变化所需要的是仅仅是改变视图层的实现方式，而控制层和模型层无需做任何改变。

由于已经将数据和业务规则从表示层分开，所以可以最大化的进行代码重用了。另外，模型层也有状态管理和数据持久性处理的功能，所以，基于会话的购物车和电子商务过程，也能被Flash网站或者无线联网的应用程序所重用。

**3 生命周期成本低**
MVC模式使开发和维护用户接口的技术含量降低。

**4 部署快**
使用MVC模式进行软件开发，使得软件开发时间得到相当大的缩减，它使后台程序员集中精力于业务逻辑，界面程序员集中精力于表现形式上。

**5 可维护性高**
分离视图层和业务逻辑层使得WEB应用更易于维护和修改。

**6 有利软件工程化管理**
由于不同的组件（层）各司其职，每一层不同的应用会具有某些相同的特征，这样就有利于通过工程化、工具化的方式管理程序代码。控制器同时还提供了一个好处，就是可以使用控制器来联接不同的模型和视图，来实现用户的需求，这样控制器可以为构造应用程序提供强有力的手段。给定一些可重用的模型和视图，控制器可以根据用户的需求选择模型进行处理，然后选择视图将处理结果显示给用户。

**缺点**
**1 没有明确的定义**
完全理解MVC模式并不是很容易。使用MVC模式需要精心的计划，由于它的内部原理比较复杂，所以需要花费一些时间去思考软件的架构。同时由于模型和视图要严格的分离，这样也给调试应用程序带来了一定的困难。每个构件在使用之前都需要经过彻底的测试。

**2 不适合小、中型应用程序**
花费大量时间将MVC模式应用到规模并不是很大的应用程序通常会得不偿失。

**3 增加系统结构和实现的复杂性**
对于简单的界面来说，非要严格遵循MVC模式，使模型、视图与控制器分离，会增加结构的复杂性，并可能产生过多的更新操作，降低运行效率。

**4 视图对模型数据的低效率访问**
依据模型操作接口的不同，视图可能需要多次调用才能获得足够的显示数据。对未变化数据的不必要的频繁访问，也将损害操作性能。

说明：如果通过控制器访问模型层（而非视图层直接访问），则避免对未变化数据的不必要的频繁访问，从而解决此问题。



### 11 Reactor模式

**定义：**Reactor模式允许事件驱动的系统复用和分发服务请求。这些请求来自于一个或者多个客户端。

![image-20221113175954816](.\image\Design_Pattern\image-20221113175954816.png)

**类图：**

![image-20221113180020831](.\image\Design_Pattern\image-20221113180020831.png)

**我理解的reactor模式：**

分为两个过程：

首先是初始化

1.ConcreteEventHandler调用Reactor对象的register_handler方法以实现具体事件的注册。

2.Reactor调用ConcreteEventHandler对象的get_handle方法来获取Handler对象的handle号，Handler对象返回handle号给Reactor并且加入handle_set

3.当所有事件均注册之后，主程序调用handle_event()函数开始reactor的事件循环。reactor此时掌握了事件句柄以便随时激活，同时联合同步事件分发器等待事件的到来。例如，我们可以使用select等待TCP的接入。

其次是事件处理流程

1.主程序调用Reactor的handle_events方法发出事件处理请求

2.Reactor调用线程池的select方法从handle_set中选择一个可以调用的handle

3.通过handle号Reactor映射到对应的handler上并且调用handler的handle_event方法，concreteEventHandler随即开始事件处理、



**时序图：**

![image-20221113184123241](.\image\Design_Pattern\image-20221113184123241.png)

该时序图的流程：

1.服务端使用reactor注册ConcreteEventHandler，即告诉初始分配器，当某种特定类型事件发生时则回调该ConcreteEventHandler。

2.初始分配器请求所有的ConcreteEventHandler返回其handle进行记录以便于辨识。

3.当所有事件均注册之后，主程序调用handle_event()函数开始初始分配器的事件循环。初始分配器此时掌握了ConcreteEventHandler以便随时激活，同时联合同步事件分发器等待事件的到来。例如，我们可以使用select等待TCP的接入。

4.当事件发生的时候，同步事件分发器通知Reactor。

5.初始分配器触发ConcreteEventHandler的handle_event方法以处理事件。当事件发生时，初始分发器使用handle激活事件资源并分配合适的ConcreteEventHandler的handle_event方法，并使用方法内在的函数进行处理。

### 12 解释器模式

给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子

常被用在 SQL 解析、符号处理引擎

**abstract**

```kotlin
interface Expression {
    fun interpret(context: String): Boolean
}
```

**implementation**

```kotlin
class AndExpression(val exp1: Expression, val exp2: Expression): Expression {
    override fun interpret(context: String): Boolean {
        return exp1.interpret(context)&&exp2.interpret(context)
    }
}

class OrExpression(val exp1: Expression, val exp2: Expression): Expression {
    override fun interpret(context: String): Boolean {
        return exp1.interpret(context)||exp2.interpret(context)
    }
}

class NotExpression(val exp: Expression): Expression {
    override fun interpret(context: String): Boolean {
        return !exp.interpret(context)
    }
}

class MetaExpression(val data: String): Expression {
    override fun interpret(context: String): Boolean {
        return context.contains(data)
    }
}

fun interpretDemo() {
    val rick = MetaExpression("rick")
    val morty = MetaExpression("morty")
    println(NotExpression(rick).interpret("rick"))
    println(AndExpression(rick,morty).interpret("rick and morty"))
    println(AndExpression(rick,NotExpression(morty)).interpret("rick and morty"))
    // false,true,false
}
```

### 13 生成器模式(以下模式考试不要求)

**定义：**封装一个复杂对象构造过程，并允许按步骤构造。

**定义解释：** 我们可以将生成器模式理解为，假设我们有一个对象需要建立，这个对象是由多个组件（Component）组合而成，每个组件的建立都比较复杂，但运用组件来建立所需的对象非常简单，所以我们就可以将构建复杂组件的步骤与运用组件构建对象分离，使用builder模式可以建立。



生成器模式结构中包括四种角色：

（1）**产品(Product)**：具体生产器要构造的复杂对象；

（2）**抽象生成器(Bulider)**：抽象生成器是一个接口，该接口除了为创建一个Product对象的各个组件定义了若干个方法之外，还要定义返回Product对象的方法（定义构造步骤）；

（3）**具体生产器(ConcreteBuilder)**：实现Builder接口的类，具体生成器将实现Builder接口所定义的方法（生产各个组件）

（4）**指挥者(Director)：**指挥者是一个类，该类需要含有Builder接口声明的变量。指挥者的职责是负责向用户提供具体生成器，即指挥者将请求具体生成器类来构造用户所需要的Product对象，如果所请求的具体生成器成功地构造出Product对象，指挥者就可以让该具体生产器返回所构造的Product对象。（按照步骤组装部件，并返回Product）

![image-20221101185128451](.\image\Design_Pattern\image-20221101185128451.png)

例子：

![image-20221101185207354](.\image\Design_Pattern\image-20221101185207354.png)

```java
public abstract class ComputerBuilder {
   
    protected Computer computer;
   
    public Computer getComputer() {
        return computer;
    }
   
    public void buildComputer() {
        computer = new Computer();
        System.out.println("生成了一台电脑！！！");
    }
    public abstract void buildMaster();
    public abstract void buildScreen();
    public abstract void buildKeyboard();
    public abstract void buildMouse();
    public abstract void buildAudio();
}
public class HPComputerBuilder extends ComputerBuilder {
    @Override
    public void buildMaster() {
        // TODO Auto-generated method stub
        computer.setMaster("i7,16g,512SSD,1060");
        System.out.println("(i7,16g,512SSD,1060)的惠普主机");
    }
    @Override
    public void buildScreen() {
        // TODO Auto-generated method stub
        computer.setScreen("1080p");
        System.out.println("(1080p)的惠普显示屏");
    }
    @Override
    public void buildKeyboard() {
        // TODO Auto-generated method stub
        computer.setKeyboard("cherry 青轴机械键盘");
        System.out.println("(cherry 青轴机械键盘)的键盘");
    }
    @Override
    public void buildMouse() {
        // TODO Auto-generated method stub
        computer.setMouse("MI 鼠标");
        System.out.println("(MI 鼠标)的鼠标");
    }
    @Override
    public void buildAudio() {
        // TODO Auto-generated method stub
        computer.setAudio("飞利浦 音响");
        System.out.println("(飞利浦 音响)的音响");
    }
}
public class Director {
   
    private ComputerBuilder computerBuilder;
    public void setComputerBuilder(ComputerBuilder computerBuilder) {
        this.computerBuilder = computerBuilder;
    }
   
    public Computer getComputer() {
        return computerBuilder.getComputer();
    }
   
    public void constructComputer() {
        computerBuilder.buildComputer();
        computerBuilder.buildMaster();
        computerBuilder.buildScreen();
        computerBuilder.buildKeyboard();
        computerBuilder.buildMouse();
        computerBuilder.buildAudio();
    }
}
```

生成器模式的优缺点
优点
将一个对象分解为各个组件

将对象组件的构造封装起来

可以控制整个对象的生成过程

缺点

对不同类型的对象需要实现不同的具体构造器的类，这可能会大大增加类的数量

3.3 生成器模式与工厂模式的不同
生成器模式构建对象的时候，对象通常构建的过程中需要多个步骤，就像我们例子中的先有主机，再有显示屏，再有鼠标等等，生成器模式的作用就是将这些复杂的构建过程封装起来。工厂模式构建对象的时候通常就只有一个步骤，调用一个工厂方法就可以生成一个对象。

### 14 命令模式

数据驱动

以**命令**（Command）的形式包裹请求，并传给**调用对象**（Invoker）。调用对象寻找可以处理该命令的**合适的对象**（Receiver）来执行命令。

**abstract**

```kotlin
interface Command {
    fun execute(receiver: Receiver) {
        receiver.exec()
    }
}

interface Receiver {
    fun exec()
}

interface Invoker {
    val commands: ArrayList<Command>
    fun addCommand(command: Command) {
        commands.add(command)
    }
    fun executeCommands()
}
```

**implementation**

```kotlin
class Gardening: Command

class Cleaning: Command

class Cleaner: Receiver {
    override fun exec() {
        println("The house had been cleaned.")
    }
}

class Gardener: Receiver {
    override fun exec() {
        println("The garden had been taken care of.")
    }

}

class Butler(override val commands: ArrayList<Command>) : Invoker {
    private val gardener = Gardener()
    private val cleaner = Cleaner()
    override fun executeCommands() {
        commands.forEach{ command ->
            when(command) {
                is Gardening -> command.execute(gardener)
                is Cleaning -> command.execute(cleaner)
            }
            println()
        }
        commands.clear()
    }
}

fun commandDemo() {
    val butler = Butler(ArrayList())
    butler.addCommand(Gardening())
    butler.addCommand(Cleaning())
    butler.executeCommands()
}
```

### 15 状态模式

将各状态对应逻辑分拆到不同的状态类中，以达到**易于拓展新状态**的目的。

存在各种状态State的和一个管理状态切换的 Context 

**abstract**

```kotlin
interface State {
    fun action()
}

interface  Context {
    var currentState: State
    fun action() {
        currentState.action()
    }
}
```

**implementation**

```kotlin
class QianJiSan(override var currentState: State) : Context

class ShieldForm: State {
    override fun action() {
        println("ShieldForm")
        println("defense")
    }
}

class SwordForm: State {
    override fun action() {
        println("SwordForm")
        println("fight")
    }
}

fun stateDemo() {
    val swordForm = SwordForm()
    val shieldForm = ShieldForm()
    val qianJiSan = QianJiSan(swordForm)
    qianJiSan.action()
    qianJiSan.currentState = shieldForm
    qianJiSan.action()
}
```

### 16 桥接模式

又称为柄体(Handle and Body)模式或接口(Interfce)模式

将抽象接口作为抽象化和实现化之间的桥接结构，把**抽象化与实现化解耦**，使得实体类的功能独立于接口实现类，使得二者可以**独立变化**。避免有多种可能会变化的情况下直接继承带来的子类爆炸问题，提高扩展的灵活性。

**abstract**

```kotlin
interface Bridge {
    fun func()
}

interface Entity {
    val bridge: Bridge
    fun func()
}
```

**implementation**

```kotlin
abstract class Mecha(override val bridge: EnergyCore) : Entity {
    internal abstract fun statement()
    override fun func() {
        statement()
        bridge.func()
        println()
    }
}

//不同机甲型号可搭配不同能源核心，使得二者均易于拓展
class MechaI(core: EnergyCore): Mecha(bridge = core) {
    override fun statement() {
        println("MechaI")
    }
}

class MechaV(core: EnergyCore): Mecha(bridge = core) {
    override fun statement() {
        println("MechaV")
    }
}

class MechaX(core: EnergyCore): Mecha(bridge = core) {
    override fun statement() {
        println("MechaX")
    }
}

interface EnergyCore: Bridge

class SolarEnergyCore: EnergyCore {
    override fun func() {
        println("solar energy is transforming into power")
    }
}

class WindEnergyCore: EnergyCore {
    override fun func() {
        println("wind energy is transforming into power")
    }
}

fun bridgeDemo() {
    MechaI(WindEnergyCore()).func()
    MechaI(SolarEnergyCore()).func()
    MechaV(WindEnergyCore()).func()
    MechaV(SolarEnergyCore()).func()
    MechaX(WindEnergyCore()).func()
    MechaX(SolarEnergyCore()).func()
}
```

### 17 组合模式

又称为**部分整体模式**，依据树形结构来组合对象，创建了一个包含自己对象组的类，把**一组**相似的对象当作一个单一的对象处理。使得用户对单个对象和组合对象的使用具有**一致性**。

**abstract**

```kotlin
interface Composite {
    val composites: ArrayList<Composite>
}
```

**implementation**

```kotlin
interface HTMLElement: Composite {
    val content: String
    fun toHTML() {
        composites.forEach { it ->
            (it as HTMLElement).toHTML()
        }
    }
}

class HTML(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<html>")
        super.toHTML()
        println("</html>")
    }
}

class Body(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<body>")
        super.toHTML()
        println("</body>")
    }
}

class H1(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<h1> $content </h1>")
    }
}

class P(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<p> $content </p>")
    }
}

fun compositeDemo() {
    val html = HTML()
    val body = Body()
    body.composites.add(H1("header"))
    body.composites.add(P("a paragraph"))
    body.composites.add(P("another paragraph"))
    html.composites.add(body)
    html.toHTML()
}
```

### 18 原型模式

用于创建重复的对象，利用已有的一个原型对象，通过**复制**快速地生成和原型对象一样的实例，同时又能保证性能，当直接创建对象的代价比较大时，则可采用这种模式。

**abstract**

```kotlin
interface Content {
    fun copy(): Content
}

interface Prototype {
    val content: Content
    fun clone(): Content
}
```

**implementation**

```kotlin
//仿生人
class Android: Content {
    override fun copy(): Content {
        println("new android had been manufactured")
        return Android()
    }
}

class AndroidPrototype(override val content: Android) : Prototype {
    override fun clone(): Content {
        return content.copy()
    }
}

fun prototypeDemo() {
    val androidPrototype = AndroidPrototype(Android())
    val nAndroid: Android = androidPrototype.clone() as Android
}
```

### 19 外观模式

对外隐藏具有诸多子系统的系统的复杂性，只提供一个可以访问系统的接口

**abstract**

```kotlin
interface Facade {
    val subsystems: ArrayList<Subsystem>
    fun react()
}

interface Subsystem{
    fun react()
}
```

**implementation**

```kotlin
class ForeignAffairsMinistry: Subsystem{
    override fun react() {
        println("The Ministry of Foreign Affairs intervened")
    }
}

class NationalDefenseMinistry: Subsystem{
    override fun react() {
        println("The Ministry of Defense conducts military mobilization")
    }
}

class Government(override val subsystems: ArrayList<Subsystem> = ArrayList()) : Facade {
    private val foreignAffairsMinistry: ForeignAffairsMinistry = ForeignAffairsMinistry()
    private val nationalDefenseMinistry: NationalDefenseMinistry = NationalDefenseMinistry()

    override fun react() {
        if((1..2).random()<2){
            foreignAffairsMinistry.react()
        }else nationalDefenseMinistry.react()
    }
}
```

### 20 享元模式

通过工厂方法尝试重用现有的同类对象以减少创建对象的数量，以减少内存占用和提高性能，如果未找到匹配的对象，则创建新对象

在实际应用中，享元模式主要应用于**缓存**

> vs. Singleton
>
> 单例模式是类级别的，一个类**只能有一个**对象实例；
>
> 享元模式是对象级别的，**可以有多个**对象实例，并允许多个变量引用同一个对象实例；

**abstract**

```kotlin
interface FlyWeight

interface FlyWeightEnum

interface FlyWeightFactory {
    val map: Map<FlyWeightEnum,FlyWeight>
    fun getFlyWeight(flyWeightEnum: FlyWeightEnum): FlyWeight
}
```

**implementation**

```kotlin
enum class FaithOfTheSeven: FlyWeightEnum {
    Father,
    Mother,
    Warrior,
    Smith,
    Maiden,
    Crone,
    Stranger
}

class Deity(private val priesthood: FaithOfTheSeven = FaithOfTheSeven.Father): FlyWeight {
    fun answerPrayer() {
        println("$priesthood answered the prayer")
        println()
    }
}

class Church(override val map: HashMap<FlyWeightEnum, Deity> = HashMap()) : FlyWeightFactory {
    override fun getFlyWeight(flyWeightEnum: FlyWeightEnum): Deity {
        if (!map.containsKey(flyWeightEnum)) {
            map[flyWeightEnum] = Deity(flyWeightEnum as FaithOfTheSeven)
            println("$flyWeightEnum was awakened")
        }
        return map.getOrDefault(flyWeightEnum, Deity())
    }

    fun pray(flyWeightEnum: FlyWeightEnum) {
        println("The believers prayed")
        getFlyWeight(flyWeightEnum).answerPrayer()
    }
}
```

### 21 责任链模式

责任链

> 使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。

责任链模式（Chain of Responsibility）是一种处理请求的模式，它让多个处理器都有机会处理该请求，直到其中某个处理成功为止。责任链模式把多个处理器串成链，然后让请求在链上传递

责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。

在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。

**abstract**

```kotlin
interface Handler {
    val nextHandler: Handler?
    val authority: Priority
    fun handle(priority: Priority)
}

interface Priority
```

**implementation**

```kotlin
enum class Vampire: Priority {
    Baron,
    Vicomte,
    Comte,
    Marquess,
    Duke,
    Infante
}

interface Priest: Handler {
    val name: String
    override val authority: Vampire
    override val nextHandler: Priest?
    override fun handle(priority: Priority) {
        if ((priority as Vampire)<=authority) {
            println("$priority wad purified")
        println()
        } else {
            println("request ${nextHandler?.name?:"god"} to process")
            nextHandler?.handle(priority)
        }
    }
}

//三种神职人员
class Monk(override val nextHandler: Priest, override val name: String = "monk", override val authority: Vampire = Vampire.Vicomte) : Priest {}

class Bishop(override val nextHandler: Priest, override val name: String = "bishop", override val authority: Vampire = Vampire.Marquess) : Priest {}

class Pope(override val name: String = "pope", override val authority: Vampire = Vampire.Duke, override val nextHandler: Priest? = null) : Priest {}


fun chainOfResponsibilityDemo(){
    val pope = Pope()
    val bishop = Bishop(pope)
    val monk = Monk(bishop)
    monk.handle(Vampire.Duke)
    monk.handle(Vampire.Infante)
}
```

### 22 迭代器模式

提供一种无需对外暴露集合对象的内部表示就能就遍历集合对象元素的方法

分离了集合对象的遍历行为，把遍历元素责任交给迭代器，而不是集合对象

**abstract**

```kotlin
interface Iterator {
    fun hasNext(): Boolean
    fun next(): Any
}

interface Container {
    fun iterator(): Iterator
}
```

**implementation**

```kotlin
class Virtue: Container {
    val virtues: ArrayList<String> = arrayListOf("Chastity","Diligence","Charity","Humility","Patience","Temperance")
    inner class VirTueIterator: Iterator {
        var cur: Int = 0
        override fun hasNext(): Boolean {
            return cur<virtues.size
        }

        override fun next(): String {
            return virtues[cur++]
        }
    }
    override fun iterator(): Iterator {
        return VirTueIterator()
    }
}
```

### 23 中介模式

用一个中介对象来处理不同类/对象之间的通信，把多方会谈变成双方会谈，以降低多个类之间的通信复杂性。

中介者使各个类不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

**abstract**

```kotlin
interface Mediator {
    val communicators: ArrayList<Communicator>
    fun deliver(message: Message)
}

interface Message

interface Communicator {
    val mediator: Mediator
    fun sendMessage(message: Message, receiver: Communicator)
    fun receiveMessage(message: Message, sender: Communicator) = {}
}
```

**implementation**

```kotlin
data class ChatMessage(val content: String) : Message

class ChatRoom(override val communicators: ArrayList<Communicator> = ArrayList()) : Mediator {
    override fun deliver(message: Message) {
        println((message as ChatMessage).content)
        println()
    }
}

class ChatUser(val name: String, override val mediator: ChatRoom) : Communicator {
    override fun sendMessage(message: Message, receiver: Communicator) {
        println("$name send a message to ${(receiver as ChatUser).name}")
        mediator.deliver(message)
    }
}
```

### 24 备忘录模式

在不破坏封装性的前提下，在某对象之外保存该对象的内部状态，以便在适当的时候恢复对象

**abstract**

```kotlin
interface Originator {
    var states: States
    fun save(): Memonto
    fun load(memonto: Memonto)
}

interface States {}

interface Memonto {
    val states: States
    fun load(): States {return states}
}
```

**implementation**

```kotlin
data class GameStates(val level: Int, val stage: Int): States

class GameArchive(override val states: States) : Memonto

class Game(override var states: States = GameStates(1,1)) : Originator {
    override fun save(): Memonto {
        println("game archive is saved")
        return GameArchive(states)
    }

    override fun load(memonto: Memonto) {
        states = memonto.states
        println("game archive is loaded")
    }
}

fun memontoDemo() {
    val game = Game()
    val archi = game.save()
    game.load(archi)
}
```

### 25 模板方法模式

一个抽象类/接口公开定义了执行它的方法的模板“骨架”。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。

**abstract**

```kotlin
abstract class Template {
    protected abstract fun first()
    protected abstract fun second()
    protected abstract fun last()
    final fun steps() {
        first()
        second()
        last()
    }
}
```

**implementation**

```kotlin
class Human: Template() {
    override fun first() {
        println("birth")
    }

    override fun second() {
        println("aging")
    }

    override fun last() {
        println("death")
    }
}

fun templateDemo() {
    val human = Human()
    human.steps()
}
```

> Reactor与MVC不属于23种经典设计模式

## Java软件体系结构

**定义：**

Software architecture is the fundamental organization of a system, embodied in its components, their relationships to each other and the environment, and the principles governing its design and evolution.

### 常见的五种结构:

![image-20221120162943743](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221120162943743.png)

### 1. DataFlowSystems

<img src=".\image\Design_Pattern\image-20221120012918463.png" alt="image-20221120012918463" style="zoom:50%;" />

数据流系统是一个

1.数据的可用性控制计算

2.设计结构主要是进程之间数据的有序流动

3.数据流的模式是显式

在一个纯数据流系统中，流程之间没有其他的交互



#### Pipes and Filters 管道过滤器风格

![image-20221120013107813](.\image\Design_Pattern\image-20221120013107813.png)



**组件和连接件**

组件： 

filters -->处理数据流，一个过滤器封装了一个处理步骤。

连接件：

pipes --> 连接一个源和一个目的过滤器。



**定义**

**过滤器**

每个过滤器都有一组输入集和输出集。

过滤器从管道中读入数据流，对输入流进行内部转换和增量计算（丰富，精炼，转换，融合，分解），然后产生输出数据流并写入管道中。

每个过滤器必须是一个独立的实体：

过滤器之间无需共享状态，即filter无需知道其输入管道和输出管道所连接的其他过滤器的存在，更不必关注相邻过滤器的实现细节。

他仅仅需要对输入数据流进行特定的内部転换和增量计算，筛选出合适的数据。

数据到来是便被处理，不是收集然后处理，即在输入被完全消费之前，输出便产生了。



**管道:**

管道是将数据从一个过滤器的输出端移动到另一个过滤器的输入端，是一个单向流。

不同的管道中流动的数据流，可能具有不同的数据格式。



**应用示例**

编译器、Unix管道、图像处理，信号处理，声音与图像处理

<img src=".\image\Design_Pattern\image-20221120013557773.png" alt="image-20221120013557773" style="zoom:50%;" />

<img src=".\image\Design_Pattern\image-20221120013613789.png" alt="image-20221120013613789" style="zoom:50%;" />

**优缺点:**

![image-20221120013701574](.\image\Design_Pattern\image-20221120013701574.png)

####  Batch Sequential Systems(批处理风格)

<img src=".\image\Design_Pattern\image-20221120103436113.png" alt="image-20221120103436113" style="zoom:50%;" />

例子:

<img src=".\image\Design_Pattern\image-20221120103521445.png" alt="image-20221120103521445" style="zoom:50%;" />

### 2. Call and return systems

#### Main program and subroutine 主程序/子程序风格

<img src=".\image\Design_Pattern\image-20221120165400175.png" alt="image-20221120165400175" style="zoom:50%;" />

![image-20221120163629635](.\image\Design_Pattern\image-20221120163629635.png)

**组件和连接件**

**组件：**

过程和明确可见的数据

**连接件：**

过程调用和显式的共享数据

**控制结构：**

单线程

**特点**

调用和定义层次结构，子系统通常通过模块化来定义。

系统通常会被组织成一个主程序和一系列子程序的集合。主程序担当子程序的驱动器，为子程序提供一个人控制环路，使子程序以某种次序顺序执行。

#### ADT & OO 面向对象风格

![image-20221120165630952](.\image\Design_Pattern\image-20221120165630952.png)

<img src=".\image\Design_Pattern\image-20221120165713951.png" alt="image-20221120165713951" style="zoom:50%;" />

<img src=".\image\Design_Pattern\image-20221120165821410.png" alt="image-20221120165821410" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221120165834470.png" alt="image-20221120165834470" style="zoom:50%;" />

**组件和连接件**

组件：对象（抽象数据类型的实例）

连接件：过程调用



**特点**
在基于面向对象的模式中，操作和数据绑定在一起，隐藏实现和其他秘密。对象通过过程调用来实现交互。有两个重要方面：

对象维护自身表示的完整性
这种表示对其他对象是隐藏的



**优点和缺点**

优点：

1. 面向对象易维护，易复用
2. 对象反映现实世界，容易分解一个系统
3. 对象对客户实现了隐藏细节，所有可以在不影响其客户的情况下改变对象的实现 。

缺点：

对象的管理比较复杂，当一个对象和其他对象交互，必须知道其他对象的标识。每当一个对象的标识改变的时候，必须修改那些显示调用它的对象。





#### Hierarchical layers 分层风格

<img src=".\image\Design_Pattern\image-20221120170429839.png" alt="image-20221120170429839"  />

**组件和连接件**

组件： 在某些层中实现虚拟机

连接件： 协议， 规定了层次之间的交互方式

拓扑结构： 限制相邻层之间的交互

**特点**

一个分成系统是按照层次结构组织的，每一层向它的上层提供服务，同时它又是下层的客户。
上层必须知道下层的身份，不能调整层次之间的顺序。
大的问题分解成若干渐进的小问题，逐步解决，隐藏了很多复杂度。
内层只对其相邻的层和某些用于输出的函数是可见的，对其他外部的层是隐藏的。
修改一层，最多影响两层， 通常只会影响上层。若层之间接口稳固，则不会造成其他影响
层层相调，影响性能。

**优点和缺点**

优点：

1.支持基于逐级抽象的系统设计，这允许设计者将一个复杂的问题分解成一系列递增的步骤。
2.支持扩展，由于分层系统每一层最多和上下两层交互，对于任意一层功能的交互最多只影响其他两层。
3.支持重用，如果能保证为相邻的层提供一致的接口，他允许系统中同一层的不同实现相互交换使用。（即给同一接口建立不同实现）

缺点

定义一个合适的抽象层次可能会非常困难。比如，实际的通信协议体就很难映射到ISO框架中，因为其中许多协议跨多个层。



##### 例子

##### 1. **Partitioning in non-overlapping units**

<img src=".\image\Design_Pattern\image-20221120170644203.png" alt="image-20221120170644203" style="zoom:50%;" />

##### 2. Communication Stack

<img src=".\image\Design_Pattern\image-20221120170909445.png" alt="image-20221120170909445" style="zoom:50%;" />

##### 3. Virtual Machine

<img src=".\image\Design_Pattern\image-20221120171006612.png" alt="image-20221120171006612" style="zoom:50%;" />

##### 4. 3-tier client-server

<img src=".\image\Design_Pattern\image-20221120171313720.png" alt="image-20221120171313720" style="zoom:50%;" />

##### 5. Quality Factors

![image-20221120171345029](.\image\Design_Pattern\image-20221120171345029.png)



### 3. Independent Compone

#### Event System

<img src=".\image\Design_Pattern\image-20221120171927902.png" alt="image-20221120171927902" style="zoom:50%;" />

<img src=".\image\Design_Pattern\image-20221120171943598.png" alt="image-20221120171943598" style="zoom:50%;" />

<img src=".\image\Design_Pattern\image-20221120172005580.png" alt="image-20221120172005580" style="zoom:50%;" />

**优缺点：**

<img src=".\image\Design_Pattern\image-20221120172029758.png" alt="image-20221120172029758" style="zoom:80%;" />

**主要特点**

1.事件的触发者并不知道哪些构件会被这些事件影响，相互保持独立。
2.不能假定构件的处理顺序，甚至不知道哪些过程会被调用。
3.各个构件之间彼此之间无连接关系，各自独立存在，通过对事件的发布和注册实现关联。

<img src=".\image\Design_Pattern\image-20221122084437391.png" alt="image-20221122084437391" style="zoom:50%;" />

##### 事件系统派遣机制

<img src=".\image\Design_Pattern\image-20221122092026704.png" alt="image-20221122092026704" style="zoom:50%;" />





###### 无独立调度模块的事件系统Observable/Observer



This module is usually called Observable/Observer (被观察者/观察者).

Each module allows other modules to declare interest in events that they are sending. (每一个模块都允许其他模块向自己所能发送的某些消息表明兴趣)

Whenever a module sends an event, it sends that event toexactly those modules that registered interest in that event.

(当某一模块发出某一事件时，它自动将这些事件发布给那些曾经向自己注册过此事件的模块)

![image-20221122092357856](.\image\Design_Pattern\image-20221122092357856.png)





###### 有独立事件派遣模块的事件系统



事件派遣模块是负责接收到来的事件并派遣它们到其它模块。

事件派遣模块分为两种:**全广播式和选择广播式**

全广播式(All broadcasting) ：派遣模块将事件广播到所有的模块，但只有感兴趣的模块才去取出事件并触发自身的行为；

选择广播式(Selected broadcasting) ：派遣模块将事件送到那些已经注册的模块中。



![image-20221122092547567](.\image\Design_Pattern\image-20221122092547567.png)



![image-20221122092609853](.\image\Design_Pattern\image-20221122092609853.png)





**选择广播式的两种策略：**基于事件被执行的方式



Point-to-Point (message queue)(点对点模式：基于消息队列)

Publish-Subscribe(发布-订阅模式)



<img src=".\image\Design_Pattern\image-20221122092706296.png" alt="image-20221122092706296" style="zoom:80%;" />

<img src=".\image\Design_Pattern\image-20221122092743185.png" alt="image-20221122092743185" style="zoom:80%;" />





**例子：**

1.调试器中的断点处理：

<img src=".\image\Design_Pattern\image-20221122091825647.png" alt="image-20221122091825647" style="zoom: 80%;" />

2.美团平台

<img src=".\image\Design_Pattern\image-20221122091954833.png" alt="image-20221122091954833" style="zoom:50%;" />

### 以下为非考纲要求:

#### 1.**C2 体系结构风格**

惯用模式：

C2结构是一个层次网络，包括构件和连接件两种软件元素。构件和连接键都是包含顶部和底部的软件元素。构件和构件之间只能通过连接件进行连接，而连接件之间则可以直接进行连接。构件的顶部、底部分别与连接件的底部、顶部连接，连接件的顶部、底部也分别与连接件的底部、顶部连接。

在C2体系结构中，构件之间的所有通信必须使用消息传递机制来实现。构件之间所有传递的信息可以分为两种，一种是向上层构件发出服务请求的请求消息，另一种是向下层构件发出指示状态变化的通知消息。连接件负责消息的过滤、路由、广播、通信和相关处理。

原理图：

<img src=".\image\Design_Pattern\image-20221122093736938.png" alt="image-20221122093736938" style="zoom: 67%;" />

优点：

1. 可以使用任何编程语言来开发构件，构件重用和替换比较容易实现
2. 具有一定的扩展能力，可以有多种不同粒度的构件
3. 构件不需要共享地址空间，避免了共享全局变量所造成的复杂关系
4. 具有良好的适应性
5. 在C2体系结构中，可以使用多个工具集和多种媒体类型，能够动态地更新系统的框架结构

缺点：

1. 构件和构件之间不允许直接相连
2. 与某一个连接件相关联的构件和连接件的数目没有限制

#### 2. 数据共享体系结构风格(仓库风格)

定义：

数据共享风格也称为仓库风格。

在这种风格中，有两种不同类型的软件元素：

一种是**中央数据单元**，也成为资源库，用于表示系统的当前状态；

另一种是**相互依赖的构件组**，这些构件可以对中央数据单元实施操作。中央数据单元和构件之间可以进行信息交换，这是数据共享体系结构的技术实现基础。

根据所使用的控制策略不同，**数据共享体系结构可以分为两种类型，一种是传统的数据库，另一种是黑板。**

如果由输入流中的事件来驱动系统进行信息处理，把执行结构存储到中央数据单元，则这个系统就是数据库应用系统。

如果由中央数据单元的当前状态来驱动系统运行，则这个系统就是黑板应用系统。

**黑板是数据共享体系结构的一个特例，用以解决状态冲突并处理可能存在的不确定性知识源。**

黑板常用于信号处理，如语音和模式识别，同时在自然语言处理领域中也有广泛的应用，如机器翻译和句法分析。

 

原理图：

一个典型的黑板系统主要包括知识源、中央控制单元、控制单元。

<img src=".\image\Design_Pattern\image-20221122093932060.png" alt="image-20221122093932060" style="zoom: 80%;" />

优点：

1. 便于多客户共享大量数据，而不必关心数据是何时产生的、由谁提供的及通过何种途径来提供
2. 便于将构件作为知识源添加到系统中来

缺点：

1. 对共享数据结构，不同知识源要达成一致
2. 需要同步机制和加锁机制来保证数据的完整性和一致性，增大了系统设计的复杂度



#### 3.**解释器 体系结构风格**

惯用模式：

解释器作为一种体系结构，主要用于构建虚拟机，用以弥合程序语义和计算机硬件之间的间隙。实际上，解释器是利用软件来创建的一种虚拟机，因此，解释器风格又被称为虚拟机风格。

原理图：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221122095530856.png" alt="image-20221122095530856" style="zoom:80%;" />

优点：

能够提高应用程序的抑制能力和变成语言的跨平台移植能力。
实际测试工作可能费城复杂，测试代价极其昂贵，具有一定的风险性。
缺点：

由于使用了特定了语言和自定义操作规则，因此增加了系统运行的开销。
解释器系统难以设计和测试。

#### 4.**反馈控制环 体系结构风格**

定义：

反馈控制环是一种特定的数据流结构。传统数据流结构是线性的，而控制连续循环过程的体系结构应该是环形的。

反馈控制环系统主要包括以下三个部分：

过程，指操纵过程变量的相关机制。
数据元素，指连续更新的过程变量，包括输入变量、控制变量、操纵变量和相关参考值。
控制器，通过控制规则来修正变量，收集过程的实际状态和目标状态，调节变量以驱动实际状态朝目标状态前进。


原理图：

<img src=".\image\Design_Pattern\image-20221122095832525.png" alt="image-20221122095832525" style="zoom:80%;" />

优点：

过程控制是连续的，可以利用各种构件和相关规则来设计反馈控制环系统，实现各种功能。
反馈控制环结构能够处理复杂的自适应问题，机器学习就是一个例子。

#### 5.**C/S 体系结构风格**

原理图：

<img src=".\image\Design_Pattern\image-20221122095952070.png" alt="image-20221122095952070" style="zoom:80%;" />



<img src=".\image\Design_Pattern\image-20221122100103666.png" alt="image-20221122100103666" style="zoom:80%;" />



优点：

客户机构件和服务器构件分别运行在不同的计算机上，有利于分布式数据的组织和处理。
构件之间的位置是相互透明的，客户机程序和服务器程序都不必考虑对方的实际存储位置。
客户机侧重数据的显示和分析，服务器则注重数据的管理。
构件之间是彼此独立和充分隔离的。
将大规模的业务逻辑分布到多个通过网络连接的低成本的计算机，降低了系统的整体开销。

缺点：

开发成本较高。
在开发C/S结构系统时，大部分工作都都集中在客户机程序的设计上，增加了设计的复杂度。
信息内容和形式单一。
如果对C/S体系结构的系统进行升级，开发人员需要到现场来更新客户机程序，同时需要对运行环境进行重新配置，增加了维护费用。
两层C/S结构采用了单一的服务器，同时以局域网为中心，因此难以扩展到Intranet和Internet。
数据安全性不高。

#### **6.B/S 体系结构风格**

惯用模式：

B/S结构是三层C/S体系结构的一种实现方式，主要包括浏览器，Web服务器和数据库服务器。B/S结构主要利用不断成熟的WWW技术，结合浏览器的多脚本语言，采用通用浏览器来实现原来需要复杂的专用软件才能实现的强大功能，节约了开发成本。

B/S体系结构的核心是Web服务器，可以将应用程序以网页的形式存放在Web服务器上。

当用户运行某个应用程序时，只需要在可以断的浏览器中输入响应的 URL，向 Web 服务器提出 HTTP 请求。

当Web 服务器接收 HTTP 请求之后，会调用相关的应用程序（Servlets），同时向数据库服务器发送数据操作请求。

数据库服务器对数据操作请求进行响应，将结果返回给Web服务器的应用程序。

Web服务器应用程序执行业务处理逻辑，利用 HTML 来封装操作结果，通过浏览器呈现给用户。在B/S结构中，数据请求、王爷生成、数据库访问和应用程序执行全部由Web 服务器来完成。

原理图：

<img src=".\image\Design_Pattern\image-20221122101714095.png" alt="image-20221122101714095" style="zoom:80%;" />

优点：

1. 客户端只需要安装浏览器，操作简单。
2. 运用HTTP标准协议和统一客户端软件，能够实现跨平台通信。
3. 开发成本比较低，只需要维护Web服务器程序和中心数据库。

缺点：

1. 个性化程度比较低，所有客户端程序的功能都是一样的。
2. 客户端数据处理能力比较差。
3. 在B/S结构的系统中，数据提交一般以页面为单位，动态交互性不强，不利于在线事务处理。
4. B/S体系结构的可扩展性比较差，系统安全性难以保障。
5. B/S结构的应用系统查询中心数据库，其速度要远低于C/S体系结构。

#### 7.**公共对象请求代理（CORBA）体系结构风格**

惯用模式：

公共对象请求代理（Common Object Request Broker Architecture，CORBA）是由对象管理组织（Object Management Group，OMG）提出来的，是一套完整的对象技术规范，其核心包括标准语言、接口和协议。

在异构分布式环境下，可以利用CORBA来实现应用程序之间的交互操作，同时，CORBA也提供了独立于开发平台的编程语言的对象重用方法。

 

原理图：

<img src=".\image\Design_Pattern\image-20221122101824999.png" alt="image-20221122101824999" style="zoom:50%;" />

优点：

1. 实现了客户端程序与服务器程序的分析。
2. 将分布式计算模式与面向对象技术结合起来，提高了软件复用率。
3. 提供了软件总线机制，软件总线是指一组定义的完整的接口规范。
4. CORBA能够支持不同的编程语言和操作系统，在更大的范围内，开发人员能够相互利用已有的开发成果。

#### 8.**正交 体系结构风格**

惯用模式：

正交体系结构是一种以 垂直线索构件族 为基础的层次化结构，包括组织层和线索。

在每一个组织层中，都包含具有相同抽象级别的构件。

线索是子系统的实例，是由完成不同层次功能的构件通过相互调用而形成的，每一条线索完成系统的一部分相对独立的功能。

在正交体系结构中，每条线索的实现与其他线索的实现无关或关联很少。在同一层次中，构件之间不存在相互调用关系。

原理图：

<img src=".\image\Design_Pattern\image-20221122162054327.png" alt="image-20221122162054327" style="zoom:80%;" />

优点：

1. 结构清晰。
2. 便于修改和维护。
3. 易于重用。

#### 9. MVC 体系结构风格

惯用模式：

模型-视图-控制器（Model-View-Controller，MVC）是一种常见的体系结构风格。MVC被广泛应用与用户交互程序的设计中。

原理图：

<img src=".\image\Design_Pattern\image-20221122162922343.png" alt="image-20221122162922343" style="zoom:80%;" />

优点：

1. 多个视图与一个模型相对应
2. 具有良好的移植性。
3. 系统被分割为三个独立的部分，当功能发生变化时，改变其中的一个部分就能够满足要求。

缺点：

1. 增加了系统设计和运行复杂性。
2. 视图与控制器连接过于紧密，妨碍两者的独立复用。
3. 视图访问模型的效率比较低。



#### 10.软件体系结构建模





## RChain 区块链

### 智能合约

定义:

一个智能合约是一套以数字形式定义的承诺（commitment），包括合约参与方可以在上面执行这些承诺的协议。

区块链合约（也称为智能合同，进程或程序），包括在安装中被写入的系统合约，均使用RChain通用语言“Rholang”（高阶反射语言）编写。Rholang从rho-演算的计算形式论派生而来，支持内部程序并发。它形式化地表达了以并行组合方式执行的进程之间的通信和协调。Rholang自然地适应了代码的变迁，反射/一元 APIs，并行性，异步性和行为类型等行业趋势。

**RChain合约概述**

RChain合约是一个具有明确规定的、表现良好的并且被形式化验证过的程序，它可以与其他合约程序交互。



一个RChain智能合约是一个具有以下内容的过程:

1.持久化状态
2.相关代码
3.相关的RChain地址

要记住的一个要点是智能合约具有任意复杂度的特性。它可能是一个原子操作或是一个构成复杂协议的协议超集。

一个合约可以被外部网络代理的信息所触发，外部代理可以是一个合约或一个网络用户。



一条信息：

触发信息通过指定通道发布，该通道可以是公开的或私人的。
触发消息可能是格式范围内的任何值，从一个简单值到一个无序的字节数组、一个变量、一个数据结构、一个过程的代码以及介于该范围之间的大多数事物。
代理在指定通信信道上发送和接收消息，该信道称为“命名通道”。



一个命名通道：

是一个 “定位”, 独立进程将确保同步该位置。
用于节点之间信息发送和接受的过程。
是不可猜测和匿名的，除非在处理过程中被特意引入。
通道被实现为在“只读”和“只写”进程之间共享的变量。因此，通道的功能仅受限于解释一个变量的含义。由于通道代表了“定位”的抽象概念，因此可能会采用不同的形式。对于我们的早期解释，命名通道的功能可以从单个服务器的本地存储器地址（变量）到分布式系统中节点的网络地址。

与该解释一致，一个区块链地址是经过命名的通道，即一个代理可以到达的位置。

两个合约在名为“Address”的通道上发送和接收信息：
![image-20221123204303587](C:\Users\Administrator\Desktop\github repo\Learing_note\image\Design_Pattern\image-20221123204303587.png)

这个模型描述了两个合约，它们都可以接收和发送消息。在某个时刻，一个外部actor会提示 Contract1 在通道 address 上发送了一个值 v ，该 address 是 Contract2 的地址。同时，Contract2 监听 address 通道上的值 v。在收到 v 后， Contract2 继续将 v 作为参数并调用后续操作。最后两个步骤是按序发生的。

注意，这个模型假设发送者拥有 Contract2 的地址。还要注意的是，在发送者发送了 v后，Contract1 已经运行终止。因此，除非提示，它不能再发送任何信息。类似地，在它继续调用后续操作之后，Contract2 也运行到终止状态，因此它不能再监听任何其他消息。



RChain合约用有良好的内部并发性，这意味着这些流程以及任何不相互依赖的流程都可以并行组合执行。所以我们修改了我们的符号：
![image-20221123204346136](C:\Users\Administrator\Desktop\github repo\Learing_note\image\Design_Pattern\image-20221123204346136.png)

与其他许多进程并行执行时，外部actor会提示 Contract1 在通道 address 上发送了一个值 v，该通道即 Contract2 的地址。如果 Contract1 没有值要发送，该通道将会阻塞。如果 Contract2 没有收到任何值，它会阻塞信道且不会触发后续调用操作。

### 并发模式

并发性，是围绕移动进程演算的形式化模型而设计的，伴随可组合的命名空间应用，从而允许 每个节点实际上有多条区块链。

这种多链、独立执行的虚拟机实例与“全局计算”的设计形成鲜明对比，后者限制了交易只能在单个虚拟机上顺序执行。另外，每个节点都可以配置为订阅和处理它感兴趣的命名空间（或区块链）。

“并发”是一个结构化的属性，它允许多个独立的进程组成复杂的进程。当多个进程之间相互不竞争资源，我们称这些进程是独立的。

由于RChain致力于在Rholang和RhoVM中实现并发性，所以我们将获得并行性和异步性这一自由且重要的属性。 无论节点是在一个处理器还是在一百万个处理器上运行，RChain设计都是可扩展的。


### 哈希

散列函数（或散列算法，又称哈希函数，英语：Hash Function）是一种从任何一种数据中创建小的数字“指纹”的方法。散列函数把消息或数据压缩成摘要，使得数据量变小，将数据的格式固定下来。该函数将数据打乱混合，重新创建一个叫做散列值（hash values，hash codes，hash sums，或hashes）的指纹。散列值通常用一个短的随机字母和数字组成的字符串来代表。好的散列函数在输入域中很少出现散列冲突。在散列表和数据处理中，不抑制冲突来区别数据，会使得数据库记录更难找到。



所谓"哈希"就是计算机可以对任意内容，计算出一个长度相同的特征值。区块链的 哈希长度是256位，这就是说，不管原始内容是什么，最后都会计算出一个256位的二进制数字。而且可以保证，只要原始内容不同，对应的哈希一定是不同的。



举例来说，字符串123的哈希是（a8fdc205a9f19cc1c7507a60c4f01b13d11d7fd0）（十六进制），转成二进制就是256位，而且只有123能得到这个哈希。（理论上，其他字符串也有可能得到这个哈希，但是概率极低，可以近似认为不可能发生。）



两个重要的推论。

推论1：每个区块的哈希都是不一样的，可以通过哈希标识区块

推论2：如果区块的内容变了，它的哈希一定会改变。


### 公私钥

公私钥属于加密范畴。

安全散列算法（Secure Hash Algorithm，缩写为SHA），用改算法给任意长度的数据能计算出长度固定的字符串（又称消息摘要），并且该字符串是唯一的。

私钥其实是使用SHA-256生成的32字节（256位）的随机数，有效私钥的范围则取决于比特币使用的secp256k1 椭圆曲线数字签名标准。大小介于0x1 到0xFFFF FFFF FFFF FFFF FFFF FFFF FFFF FFFE BAAE DCE6 AF48 A03B BFD2 5E8C D036 4140之间的数几乎都是合法的私钥。

在私钥的前面加上版本号，后面添加压缩标志和附加校验码，（所谓附加校验码，就是对私钥经过2次SHA-256运算，取两次哈希结果的前四字节），然后再对其进行Base58编码，就可以得到我们常见的WIF（Wallet import Format)格式的私钥。



**私钥经过椭圆曲线乘法运算，可以得到公钥。**公钥是椭圆曲线上的点，并具有x和y坐标。公钥有两种形式：压缩的与非压缩的。早期比特币均使用非压缩公钥，现在大部分客户端默认使用压缩公钥。

由于数学原理，从私钥推算公钥是可行的，从公钥逆推私钥是不可能的。



公钥就是地址这句话是不正确的。从公钥到地址还要经过一些运算。

 由于椭圆曲线乘法以及哈希函数的特性，我们可以**从私钥推导出公钥，也可以从公钥推导出地址，而这个过程是不可逆的。**也正因如此，在整个系统中，公钥是可以公开的，也就是说钱包地址是安全的。私钥是最关键的部分。



**地址的生成过程:**

1 生成私钥与公钥

2 将公钥通过SHA256哈希算法处理得到32字节的哈希值

3 对得到的哈希值通过RIPEMD-160算法来得到20字节的哈希值 —— Hash160

4 把版本号+Hash160组成的21字节数组进行双次SHA256哈希运算，得到的哈希值的头4个字节作为校验和，放置21字节数组的末尾。

5 对组成25位数组进行Base58编码，就得到地址。



### 共识

在区块链里大家都认同的一个规则。所有的交易或者其他信息传递需要经过共识机制的确认，确认合法后才能将这个信息保存在链上。这样就保证了大家记账的一致性和准确性。

共识 （Capser 一种基于权益证明的共识协议）将确保区块链网络中所有节点状态的一致性。

RChain协议中关于副本和共识的部分被称为Casper，是一种权益证明(pos)的协议。类似于以太坊，合约在一种状态下开始，许多节点收到一个带签名的交易，然后在RhoVM实例上执行该合约并进入到下一状态。一系列节点运算符或“被绑定的验证器”将共识算法应用于加密-高效地验证RhoVM实例上状态的配置和状态转移的整个记录，并且这些都被精确的备份到分布式的数据存储中。

**在RChain网络中，所有节点都将包含最基本的使用Rholang进行开发的系统合约。系统启动过程中将使用这些系统合约来实现RhoVM实例的运行，负载均衡，dApp合约的管理，代币系统，节点信任以及其他一些功能。**




### 分布式账本

分布式账本是一种数据库类型，可在分散网络的成员之间共享，复制和同步。分布式账本记录网络参与者之间的交易，例如资产或数据交换。

网络的参与者对分布式账本中记录的更新进行管理并达成共识。不涉及中央机构或第三方调解人，例如金融机构或票据交换所。分布式账本中的每个记录都有一个时间戳和唯一的加密签名，从而使分布式账本中的所有交易都可以被审核，并不会被篡改。

RChain区块链是一种防篡改的共享数字分布式账本，可记录公共或私有对等网络中的交易。分布式账本分布到网络中的所有成员节点，以加密散列链接的块的顺序链，永久记录网络中对等点之间发生的资产交换的历史记录。

所有已确认和验证的交易区块都从链的开头链接到最新区块，因此命名为区块链。因此，区块链充当单一事实来源，并且区块链网络中的成员只能查看与之相关的那些交易。

**区块链中分布式账本的保障**

共识确保共享分布式账本是准确的副本，并降低了欺诈性交易的风险，因为篡改必须在完全相同的时间在许多地方进行。诸如SHA256计算算法之类的密码散列可确保对交易输入的任何更改（即使是最小的更改）都将导致计算出不同的哈希值，这表明潜在的交易输入受到损害。数字签名可确保交易源自发件人（使用私钥签名），而不是冒名顶替者。

分散的点对点区块链网络可防止任何单个参与者或参与者组控制基础基础架构或破坏整个系统。网络中的参与者都是平等的，遵循相同的协议。他们可以是个人，国家行为者，组织，也可以是所有这些类型的参与者的组合。

在其核心部分，系统使用选定的共识模型记录所有节点都同意交易有效性的交易时间顺序。结果是无法更改或撤消的交易，除非网络中所有成员在后续交易中都同意更改。


### 数据结构层面与以太坊的区别

**Rchain数据结构:**

一个blocks构成的图。每一个block都包含一个header指向一个或者多个previous block,事务列表和其他元信息

**以太坊数据结构:**

一个blocks构成的链。每一个block包含了一个header指向一个previous block,事务列表和异构列表

