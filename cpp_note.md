# Cpp_learning_note

### Section 1 CPP 面向过程

#### Chapter 1 处理数据

##### chapter 1.1 整型类型

1. 命名规范：
* 在名称中只能使用字母字符、数字和下划线
* 名称的第一个字符不能是下划线
* 不能将Cpp关键字作为名称
2. 整型：
* short 至少16位
* int至少与short 一样长
* long至少32位，且至少与int一样长
* long long 至少64位，且至少与long一样长
* 在头文件climits中包含了关于整型限制的一些信息，如INT_MAX为int的最大取值,CHAR_BIT为字节位数
* 初始化：

``` cpp

int a = 4; 
int a(4) //CPP
int a = {4} //C++11
int a{4}/C++11
```

* <font color = 'red'>对于整型，如果超过了范围限制，它的值就会成为范围另一端的取值</font>

``` cpp
short a = SHRT_MAX;
cout<<a<<endl;
a++;
cout<<a<<endl;
//32767
//-32768
```

* 整型字面值
cpp 整型有三种基数：10、8、16
当数字常量第一位为1~9时，为十进制
当数字常量第一位为0时，为八进制
当数字常量以0x开头时，为十六进制
* 在数字后面加上l指出该数字是long类型，在数字后面加上ll可以指出该数字是long long类型
3. bool类型
* 任何数字值或指针值都可以被隐式地转换为bool类型，任何非零值被转换成true,零值被转换成false

##### chapter 1.2 浮点数

* 浮点数有两种表示方法：ios_base::fixed和ios_base::scientific 也就是定点数表示法和科学表示法。
* 浮点常量默认为double类型，除非在数字后面加上f特别指出它是float类型，比如7.8f
* float至少32位，double至少48位，且不少于float，long double至少和double一样多
* float 一般系统能确保6个有效位 ，double一般系统能确保15个有效位。所以虽然long 与float一般都是四个字节，但是long->float会有精度丢失。

##### chapter 1.3 cpp数值变量的类型转换

C++通常在如下情形时进行类型转换：
* 将一种算数类型的值赋给另一种算数类型的值时
* 表达式中包含不同的类型时
* 将参数传递给函数时

1. 初始化化和赋值进行的类型转换
* 宽化转换一般不会出问题
* <font color = 'red'>窄化类型转换非常的危险，如果把浮点数转换为整型，小数部分会丢失（而不是四舍五入）；如果把较高精度的浮点数转换为较低精度的浮点数，精度将会降低；如果把大整型转换为小整型，有可能会超过原来的范围，通常只会赋值右边的字节。</font>
2. 以{}方式初始化列表时进行的转换(cpp11)
初始化列表对类型转换的要求更加的严格，不允许窄化类型转换。

``` cpp
char c1 {31325} //narrowing,not allowed
char c2 = 31325 //narrowing , but allowed

```

3. 表达式中的转换
* 整型提升：
  在计算表达式的时候，C++隐式地将bool、char、unsigned char和short值转换成int。

  ``` cpp
  short chickens = 20;
  short ducks = 15;
  short fow1 = chickens + ducks;
  //在这里,chickens和ducks先被转换成int进行计算，
  //得到的结果又被窄化转换成short赋给fow1.
  ```

* 算术运算中的类型转换校验表：
   (1) 如果有一个操作数类型是long double，则另一个操作数的类型转换为long double
   (2) 否则，如果有一个操作数的类型是double，则将另一个操作数转换为double
   (3) 否则，如果有一个操作数的类型是float，则将另一个操作数转换为float
   (4) 否则，则说明操作数的类型都是整型，进行整型提升。
   (5) 在这种情况下，如果两个操作数都是有符号的或者无符号的，且其中一个操作数的级别比另外一个低，则转换为级别高的类型
   (6) 如果一个操作数为有符号的，另一个操作数位无符号的，且无符号操作数的级别比有符号操作数的级别高，则将有符号操作数转换为无符号操作数所属的类型
   (7) 否则，如果有符号类型可表示无符号类型的所有可能值，则将无符号操作数的类型转换为有符号操作类数所属的类型
   (8) 否则，将两个操作数都转换为有符号类型的无符号版本
* 操作类型级别表：
   long long > long > int > short > signed char > bool

4. 强制类型转换
   在c++中，除了传统c风格的强制类型转换外，对于数值类型的转换还可以用static_cast<>。
   
   ``` cpp
   //c风格
   (typename) value;
   typename (value);
   //c++
   static_cast<typename> (value);
   
   ```
   
##### chapter 1.4 数组

* cpp中数组是一种数据格式，可以存储多个相同类型的值。
  
* 在声明数组时，我们通常使用如下格式：

  ``` cpp
  typename arrayName[arraySize]
  //arraysize必须是整型常数或者const值！
  ```

* <font color = 'red'>只有在定义数组的时候才能初始化</font>，数组通常初始化的方式如下：

  ``` cpp
  int arr[3] = {1, 2, 3};
  int arr2[] = {1, 2, 3};
  int arr3[3] {1, 2, 3};//cpp11
  //同样的，在运用cpp11初始化列表时候禁止窄化转换
  ```

##### chapter 1.5 c风格字符串

* C风格字符串以'\0'结尾(注：'\0'的ASCLL码是0)，常见的初始化方式有：
  ```cpp
  char dog[8] = {'b', 'e', 'a', 'u', 'x', ' ','I', '\0'};
  char bird[11] = "Mr. Cheaps";
  char fish[] = "Bubbles";
  // 上面的""括起的字符串实际上是const char*类型，常常被称为字符串字面值/字符串常量
  ```
* 字符串常量代表的是一个地址
* 常见的C风格字符串处理函数有
  ```cpp
  char* std::strcpy(char *,const char *);
  char* std::strcat(char *,const char *);
  int std::strcmp(const char *,const char *);
  size_t std::strlen(const char *);
  ```
* 字符串输入输出参见Section2中的I/O
  
##### chapter 1.6 结构体(struct)
* 在cpp中，允许声明结构变量的时候省略关键字struct
* 结构常见的初始化方式有以下两种:
  ```cpp
  struct inflatable
  {
    char name[20];
    float volume;
    double price;
  };
  ------main-------
  inflatable guest = {"XiaoMing", 1.88, 2.99};
  inflatable other{"XiaoHong", 1.55, 2.34};
  //cpp11风格，如果大括号内没有任何东西，则结构变量每个字段都被设置为0。
  //注意，cpp11风格的初始化不能缩窄转换

##### chapter 1.7 共用体(union)

* 共用体的长度为其最大成员的长度，共用体能够存储不同的数据类型，但是同时只能存储其中的一种。
* 匿名共用体(anonymous union)没有名称，其成员将成为位于相同地址处的变量。<font color = 'red'>使用匿名共用体不需要中间标识符。</font>
  ```cpp
  struct widgt1
  {
    char brand[20];
    int type;
    union id
    {
      long id_num;
      char id_char[20];
    }id_val;
  };
  struct widgt2
  {
    ...
    union
    {
      long id_num;
      char id_char[20];
    }//匿名共用体
  };
  -------------main---------------
  widgt1 prize1;
  cin>>prize1.id_val.id_num;
  widgt2 prize2;
  cin>>prize2.id_num;//不需要中间标识符
  ```
* 共用体常用于节省内存
  
##### chapter 1.8 枚举类型(enum)
* 在默认的情况下，将整数值赋予给枚举变量。第一个枚举变量的值为零，第二个枚举变量的值为1，依次类推。可以通过显式地指定整数值来覆盖默认值，后面没有初始化的枚举量的值总是比前面的大1。
* 在没有进行强制类型转换的前提下，只能将定义枚举时使用的枚举常量赋给这种枚举的变量
* 枚举量是整型，可以被提升为int类型，但是int类型不能隐式地转换为枚举类型
* 如果int值是有效的，可以通过强制类型转换，将int类型的变量赋给枚举类型的变量
* 如果只打算使用常量，而不创建枚举类型的变量，则可以省略枚举类型的名称
* 可以创建多个值相同的枚举量
  ```cpp
  enum spectum {red, orange, yellow, green, blue, violet, indigo = 100, ultraviolet};
  //red、orange等常量被称为枚举量
  band = blue; //valid, blue is an enumerator
  band = 8; //invalid
  band = spectum(8) //valid
  band = orange + yellow //invalid 在执行加法运算时出现了整型提升
  band = spectum(orange + yellow)//valid
  enum{one = 1, two = 2, zero,null = 0};//valid,省略了枚举的名称，相当于定义了一系列常量。
  ```
##### chapter 1.9 表达式与运算符
1. 赋值表达式
    cpp将赋值表达式的值定义为左侧成员的值
2. 副作用与顺序点：
    副作用是指计算表达式时对某些东西(如存储在表达式中的值)进行了修改。
    顺序点是指程序执行过程中的一个点。
    在cpp中，程序进入下一个顺序点之前必须确保对所有的副作用进行评估。
    在cpp中，语句的分号就是一个顺序点，程序在处理下一条语句之前，赋值运算符，递增递减运算符将执行的所有修改都必须完成。另外，完整的表达式的末尾也是一个顺序点。
3. <font color = 'red'>前缀版本的效率比后缀版本高</font>。
4. 前缀函数是加一之后返回引用，后缀版本是先复制一个副本，将其加一，之后再将副本返回。
5. 变量遮掩：
    与java不同，如果在一个语句块中声明一个变量，而外部语句块中也有一个这种名称的变量，则在声明位置到内部语句块结束的范围之内，新变量将掩盖旧变量。之后旧变量将再次可见。
6. 逗号运算符：
    C++中的逗号运算符有两个特性:
  * 逗号运算符确保先计算第一个表达式，再计算第二个表达式。
   ```cpp
   i = 20 ,j = i * 2;
   //上述表达式安全
   ```
  * 逗号表达式返回的是最后一部分的值：
   ```cpp
   y = (i = 30, j = 40 , k = 60);
   //结果是 y = 60
   ```
7. 关系表达式不可用于比较C风格字符串，
    需要使用int std::strcmp(const char*,const char*)函数进行比较。
8. 逻辑运算符
    逻辑运算符有|| && 和 !
  * && 运算优先级高于 ||
  * && 和 || 遵循短路原则，因为&& 和 || 都是顺序点，C++确保程序从左到右计算逻辑表达式，并且在知道结果的时候立即终止程序。
9. 三目运算符
  ```cpp
  expression1 : expression2 ? expression3
  ```
  如果第一个表达式为true，那么整个条件表达式的值为expression2的值，否则，整个条件表达式的值为expression3的值。
10. 动态内存分配运算符
   * 运算符new 和new[]分别调用如下函数:
      ```cpp
      void* operator new(std::size_t);
      void* operator new[](std::size_t);
      ```
      这些函数被称为分配函数
   * 运算符delete 和delete[]分别调用如下函数:
      ```cpp
      void operator delete(void*);
      void operator delete[](void*);
      ```
   * 定位new运算符
      通常new 运算符负责在堆中找到一个足以能够满足要求的内存块。new运算符还有一种变体，被称为定位new运算符，他能让我们指定要使用的位置。
       ```cpp
        include "new"
        char buffer1[50];
        char buffer2[100];
        p1 = new chaff;
        p3 = new int [20];
      
        p2 = new (buffer1) chaff;
        //定位new运算符,place structure in heap;
        p4 = new (buffer2) int[20]; 
        //定位int array in buffer2
       ```
      <b>动态内存分配的详细内容在section2 类与动态内存分配章节介绍。</b>

##### chapter 1.10 指针类型基础
* 指针不是整型，在不使用强制类型转换的前提下，我们无法把整型赋给指针变量。
  ```cpp
  int* pt;
  pt = 0xB8000000; //invalid,type not match
  pt = (int*) 0xB8000000;//types now match
  ```
* 声明指针：
     ```cpp
     (typename* ) pointerName;
     ```
* 给指针赋值:
    ```cpp
      double* pn;
      double* pa;
      char* pc;
      double bubble = 3.2;
      pn = &bubble; 
      pa = new double;
      pc = new char[30];
      //new 运算符返回分配的内存的地址
    ```
* 动态分配内存时，指针变量存储在栈区，new分配堆区的内存。即使栈区的变量被释放，如果不使用delete，堆区的内存也不会释放。
* 用delete释放new分配的内存，用delete[ ]释放new[ ]分配的内存
* 数组名是常量，不能修改数组名的值
* 注意结合的优先顺序:
  ```cpp
  int* pt[20] 相当于 (int*) pt[20]
  这是一个指针数组 pt[0]~pt[19]都是(int*)
  
  int (*pt)[20] 是一个指向长度为20的int数组的指针，
  这个指针类型是int(*)[20]类型。
  
  short tell[20];
  short* pt1 = tell;
  short* pt2 = &tell[0];
  //pt1 == pt2
  short(*pt3)[20] = &tell;
  //pt3是一个short(*)[20]类型的指针
  ```
* 如果对象是结构，则可以使用指针解除引用运算符(->)来访问他的成员。
  pointerName->member效果等价于(*pointerName).member

##### chapter 1.11 常量指针基础
* 在cpp中，可以使用两种不同的方式将const用于指针。第一种方法是让指针指向一个常量对象，这样可以防止使用该指针来修改所指向的值；第二种方法是将指针本身声明为常量，这样可以防止改变指针指向的位置。
   ```cpp
   const int* ptr1;//防止使用该指针来修改所指向的值
   int* const ptr2;//防止改变指针指向的位置
   const int* const ptr3;//两个都不能改变
   ```
* 不能让非const指针指向const变量。如果非要这样做，必须使用强制类型转换const_cast<>。
* 当且仅当只有一层间接关系时(如指针指向基本数据类型)，才可以让const指针指向非const变量。
   ```cpp
   const int** pp2;
   int* p1;
   const int n = 13;
   pp2 = &p1;
   *pp2 = &n;
   *p1 = 10;
   ```
   如果不为一层间接关系，就会出现如上所示的错误，p1修改了const变量，很明显是不合法的。
* 禁止将常量数组的地址赋给非常量指针，意味着不能将常量数组名传递给使用非常量形参的函数。
  
##### chapter 1.12 左值与右值
* 右值是不能取地址，出现在赋值运算符右边的值
  右值一般包括
   * 表达式
   * 字面常量(整型常量、字符常量、枚举量)
   * 非引用的函数返回值
* 左值是能取地址，出现在赋值运算符左边的值
  左值一般包括
   * 可修改的左值：
     * 变量
     * 数组元素
     * 结构成员
     * 引用
     * 解除引用的指针
   * 不可修改的左值：const变量

##### chapter 1.13 引用变量
C++新增了一种复合类型——引用变量。引用是已定义的变量的别名，主要用途是用作函数的形参，通过将引用变量用作参数，函数将使用原始数据，而不是其副本。

* 引用变量必须在声明时将其初始化，一旦与某个变量关联起来，将不能改变
* 其他的关于引用变量的用法看函数章节以及类和动态内存分配章节。


#### Chapter 2 循环与分支

##### chapter 2.1 循环结构
1.<b> for循环 </b>
  ```cpp
  for(initialization;test-expression;update-expression)
    {body}
  ```
  * C++与java不同；并没有把test-expression的值限制为真或者假。可以使用任意的表达式,c++将把结果强制转换为bool类型。
  * for循环是入口条件循环，在每轮循环之前都会测试，一旦测试为假，将不会执行循环体。
  * 更新表达式在循环结束时执行，此时body已经执行完毕。
2. <b> while循环 </b>
  ```cpp
  while(test-condition)
    {body}
  ```
  * while循环也是入口条件循环。
  * for循环与while循环可以相互转换
3. <b> do while循环 </b>
  ```cpp
  do 
  {
    body
  }while(test-condition);
  ```
  * do while是出口条件循环。这意味着这种循环将首先执行循环体，然后再判定测试，决定是否应该继续循环。出口条件循环通常至少执行一次。
4. <b> 基于范围的for循环(enhanced for) </b><br>
    C++11新增了一种基于范围的for循环。这种for循环与java十分类似，但是区别在于如果使用引用类型变量c++的enhanced for可以修改容器中的值。
   ```cpp
   for(elementType item : container)
   {body}
    
   for(elementType& item : container)
   {body}
   container通常是模板或者容器类。
   ```

##### chapter 2.2 分支结构

1. <b>if语句</b><br>
   ```cpp
   if(test-expression) {body}
   else if(test-expression) {body}
   ...
   else {body}
   ```
2. <b>switch语句</b><br>
   ```cpp
   switch (integer-expression)
   {
     case label1 : statement;break;
     case label2 : statement;break;
     ...
     default : statement;
   }
   ```
   * <font color='red'> integer-expression必须是一个结果为整数的表达式。每个label必须是整数常量表达式。最常见的标签是Int或char常量，也可以是enum量</font>
   * 程序不会执行完一个case语句后终止，必须使用break语句

#### Chapter 3 函数
##### chapter 3.1 函数的基本知识
* 一般来说 C++ 函数要被使用，需要提供函数声明和函数定义。
* 在C++ 中，函数原型是必不可少的。具体来说，函数原型的作用有以下几点：
  * 编译器正确处理函数返回值
  * 编译器检查使用的参数是否正确
  * 编译器检查使用的参数类型是否正确。如果不正确，则转换为正确的类型(如果可能的话)
* 定义函数通常在给出函数原型之后，一般函数定义如下：
   ```cpp
   typeName functionName(parameterList)
   {
     statements
     return value;
   }
    
   void functionName(parameterList)
   {
     statements;
     return;//optional
   }
   ```
  * c++对返回值的类型有一定的限制，不能是数组！但是我们可以把数组隐藏在结构或者对象中返回。
* 在编译阶段进行的原型化被称为静态类型检查。
* cpp通常按值传递参数，这意味着数值参数传递给函数。

##### chapter 3.2 函数和数组、指针
* 在cpp中，当且仅当用于函数头或者函数原型中的时候，int* arr和int arr[]的含义才是相同的。
* 如果要确保防止函数不能修改数组中的内容，可以在声明形参的时候使用关键字const。
* 如果要对数组区间进行操作，常用的方法是将指向数组起始处的指针作为一个参数，将数组长度作为第二个参数；或者指定元素区间：一个指针标识数组开头，另一个指针标识数组的尾部。
* 对于二维数组来说，数组名是一个指向数组的指针，函数原型正确的声明如下例：
   ```cpp
   int data[3][4] = {{1, 2}, {1}, {1}};
   int total = sum (data, 3);
   int sum (int(*)[3], int)//函数原型
   int sum (int[][3], int)//函数原型的另一种写法
   ```
* 函数指针是一种指向函数的指针
   ```cpp
   int (*ptr)(int, int);
   //ptr是一个指向函数的指针，指向的函数必须返回值为int,参数为两个int
   int add(int, int);
   int exception(int, int);
   ptr = add;//ptr指向函数add
   ```
##### chapter 3.3 函数与引用类型
1. <b>将引用用作函数参数</b>
   引用经常被用作函数参数，使得函数中的变量名成为调用程序中的变量的别名。这种传递参数的方法被称为按引用传递。按引用传递允许被调用的函数能够访问调用函数中的变量。C++新增的这一条特性是对C语言的超越，C语言只能按值传递
   * 临时变量、引用参数和const:
      如果实参与引用参数不匹配，C++将生成临时变量，但是生成临时变量是有规则的：
     * 当且仅当引用参数是const时候，才能创建临时变量。<font color = 'red'>非const引用不能生成临时变量！</font>
      ```cpp
      void swap(int &a,int &b)
      {
        int tmp = a;
        a = b;
        b = tmp;
      }
     
      double a=3.1,b=2.0;
      swap(a,b);//invalid
      //如果接受引用参数的函数的意图是修改作为参数传递的变量，
      //则创建临时变量将使得修改形参变得没有任何意义。
      ```
     * 实参的类型正确但不是左值的时候将创建临时变量。
     * 实参的类型不正确但可以转换成正确的类型，这是将创建符合形参类型的临时变量。
     * 这些临时变量只在函数调用期间存在，此后编译器便可将其肆意删除。
   * <font color = 'red'>引用参数应当尽可能使用const </font>
      * 使用const可以避免无意中修改数据的编程错误
      * 使用const可以使函数能够处理Const和非const形参，否则将只能接受非const数据
      * 使用const引用使函数能够正确地生成并使用临时变量
2. <b>将引用作为返回值</b>
   传统的返回机制与按值传递参数类似：<font color = 'blue'>计算关键字return后面的表达式，并将结果返回给函数，从概念上来说，这个值被复制到一个临时位置，而调用程序将使用这个值。</font>
   * 返回引用最重要的一点是：<font color = 'red'>应该避免返回函数终止时不再存在的内存单元引用。</font>
   * 若函数的返回类型为引用，我们一般只会返回如下三种引用
     * 返回一个引用类型的参数
     * 返回一个指针类型参数的解引用。
     * 返回一个在被调用函数中通过new分配存储空间的指针的解引用。
   * 将const用于引用返回类型：
     * 在赋值语句中，左边必须是可修改的左值，如果返回的是普通引用，以下语句将合法：
       ```cpp
       int& add(int a,int b,int& c)
       {
         c = a + b;
         return c;
       }
       
       int c=5;
       add(3,4,c) = 10
       //因为add返回的是可修改的左值，故上述语句合法。
       ```
     * 另一方面，常规函数返回类型是右值——不能通过地址访问的值。因为常规函数的返回值位于临时内存单元中，随时可能被释放。
     * 如果我们需要使用引用返回值，但又不允许执行类似于给add赋值这样的操作，只需将返回值声明为const引用。
3. 使用引用参数的原因
   * 使用引用参数的主要原因有两个
     1. 程序员能够通过被调用函数修改调用函数中的对象
     2. 通过传递引用而不是传递整个数据对象，可以提高程序的运行速度。当数据对象较大的时候(结构或者类对象)，第二个原因最重要。
##### chapter 3.4 内联函数(inline)
在执行函数调用指令时，程序将在函数调用后立即存储该指令的内存地址，并将函数参数复制到堆栈中，跳到标记函数起点的内存单元，执行函数代码(也许还需要将返回值放入到寄存器中)，然后跳回到地址被保存的指令处。来回跳跃并且记录跳跃位置意味着以前使用函数时，需要一定的开销。
C++的内联函数的编译代码与其他程序代码"内联"起来了。也就是说，编译器将使用的函数代码替换函数调用。对于内联代码，程序无需跳到另一个位置去执行代码，再跳回来。
* 要使用内联函数，必须采取以下措施之一：
   * 在函数声明前加上关键字inline
   * 在函数定义前加上关键字inline
* 内联函数不能递归
* 内联函数与常规函数一样，是按照值来传递参数的，但是宏定义是文本替换，因此C++内联的功能远远胜于宏定义。
   ```cpp
   #define SQUARE(X) X*X
   a = SQUARE(4.5 + 7.5)
   //等价于4.5 + 7.5 * 4.5 +7.5
   //很明显出了问题
   
   inline int square(int x){return x*x}
   a = square(4,5+7.5)//按值传递参数，没问题
   ```

##### chapter 3.5 默认参数
* 在cpp中，必须通过函数原型来设置默认参数,<font color = 'red'>一般我们不通过函数定义设置默认参数!</font>
* 不允许在声明和定义处同时出现默认参数
* 对于带参数列表的函数，<font color = 'red'>必须从右到左添加默认值</font>。
* 实参必须按照从左到右的顺序依次赋给形参，不能跳过任何一个参数
   ```cpp
   int harpo(int n ,int m = 4,int j =5);//valid
   int chico(int n, int m = 6, int j);//invalid
   
   beeps = harpo(2);//same as harpo(2,4,5);
   beeps = harpo(1,8);//same as harpo(1,8,5)
   beeps = harpo(8,7,6);//no default arguments used
   beeps = harpo(3,,8);//invalid
   ```

##### chapter 3.6 函数重载
默认参数让我们能够使用不同数目的参数调用同一个函数，而函数重载能够让我们使用多个同名的函数。
函数重载的关键是参数列表。C++允许定义同名函数，条件是他们的签名不同。当且仅当两个函数参数数目和类型相同，参数排列顺序也相同的时候两个函数的签名相同。
* 当没用匹配的原型时，并不会自动停止使用其中的某个函数，因为C++将尝试使用类型转换强制进行匹配。如果出现多个可匹配的原型但是没有优先顺序，则报错。
* 编译器在检查函数签名时候，把类型引用和类型本身视为同一个签名。
* const类型可以匹配普通类型和const类型，但是普通类型不能匹配const类型的实参。当传入一个普通类型的变量，将优先调用非const函数。
  
##### chapter 3.7 选择函数参数的原则
* 对于使用传递的值而不做修改的函数
   1. 如果数据对象很小，如内置数据类型或者小型结构，则按值传递。
   2. 如果数据对象是数组，则使用指针，因为这是唯一的选择。并将指针声明为指向const的指针。
   3. 如果数据对象是较大的结构，则使用const指针或者const引用。类设计的语义常常要求使用引用，这是c++新增这项特性的主要原因。因此，<font color = 'red'>传递类对象参数的标准方式是按引用传递。</font>
* 对于修改调用函数中数据的函数
   1. 如果数据对象是内置数据类型，则使用指针。
   2. 如果数据对象是数组，则只能使用指针。
   3. 如果数据对象是结构，则使用引用或者指针。
   4. 如果数据对象是类对象，则使用引用。

##### chapter 3.8 函数重载解析
在section 3 中的函数模板章节叙述

#### chapter 4 内存模型和命名空间

##### chapter 4.1 单独编译
和C语言一样，C++也允许甚至鼓励程序员将组件函数放在独立的文件中。我们一般把原来的程序分为三个部分：
1. 头文件：包含结构/类的声明以及成员方法/函数的原型
2. 源代码文件：包含与结构/类相关的成员方法/函数的定义
3. 源代码文件：包含调用与结构/类相关的方法/函数的代码

* <font color = 'red'>请不要把函数定义或者变量声明放到头文件中。</font>
* 头文件中常包含的内容：
   * 函数原型
   * 使用#define 或者const 定义的符号常量
   * 结构声明
   * 类声明
   * 模板声明
   * 内联函数
* 如果头文件包含在尖括号中，那么C++编译器将在存储标准头文件的主机系统的文件系统中查找；但如果头文件包含在双引号中，则编译器将优先查找当前的工作目录或源代码目录。如果没有在那里找到头文件，则将在标准位置查找。因此在包含自己的头文件时，应使用引号而不是尖括号。
* <font color = 'red'>同一文件不能重复包含同一头文件！</font>
  记住这个规则很容易，但是我们可能在不知情的情况下将头文件包含多次。因为有时候我们会使用包含另一个头文件的头文件。
  我们可以使用如下的代码来规避：
   ```cpp
   # ifndef COORDIN_H_
   ...
   #endif
   //这段代码意味着当且仅当以前没有使用预处理其编译指令#define定义名称COORDIN_H_时，
   //才处理#idndef和#endif之间的语句。
   ```
* <font color = 'red'>不同源文件的同一头文件会自动合并。</font>
  比如a.cpp和b.cpp都包含了iostream，在编译的时候不会出现错误，因为iostream被合并了。
* 不同编译器创建的二进制模块很可能无法正确地链接。因此我们在链接编译模块时候，要确保所有对象文件或者库都是由同一个编译器生成的。

##### chapter 4.2 变量的存储持续性、作用域和链接性
在C++中，使用四种不同的方案来存储数据，这些方案的区别主要在于数据保留在内存中的时间。
* <b>自动存储持续性：</b>
   在函数定义中声明的变量(包括函数参数)的存储持续性为自动的。它们在程序开始执行其所属的函数或者代码块时候被创建，在执行完函数或者代码块时，它们使用的内存被释放。C++有两种存储持续性为自动的变量。
* <b>静态存储持续性:</b>
   在函数定义外定义的变量和使用关键字static定义的变量的存储持续性都是静态。它们在程序的整个运行过程中都存在。C++有三种存储持续性为静态的变量。
* <b>线程存储持续性(C++11):</b>
   如果变量是使用关键字thread_local声明的，则其生命周期和所属的线程一样长。
* <b>动态存储持续性:</b>
   用new运算符分配的内存将一直存在，直到使用delete运算符将其释放或者到程序结束的时候为止。这种内存的存储持续性为动态，有时被称为自由存储(free store)或者堆(heap)。
   注：与java类似，一个栈中的引用类型变量引用了堆中的对象，引用变量生命周期结束不代表堆中的对象生命周期结束。
   (在类与动态内存分配章节详细介绍)
* <b>单定义规则</b>
   在cpp中，变量只能有一次定义。为满足这种要求，cpp提供了两种变量声明。一种是定义声明(或简称为定义)，他给变量分配存储空间；另外一种是引用声明(或简称为声明)，它不给变量分配存储空间，因为它引用已有的变量。
   引用声明使用关键字extern,且不进行初始化:
   ```cpp
   double up;//定义，初始化为0
   extern int blem;//声明，在其他处定义
   extern int gr = 'z';//定义，分配内存空间
   ```
1. 自动存储持续性
    在默认情况下，在函数中声明的函数参数和变量的存储持续性为自动，作用域为局部，没有链接性。
    * 与java不同，程序执行内部代码块中的语句时，内部代码块中如有和外部同名的变量，内部的定义将隐藏外部的定义。如果需要访问外部变量，需要加上作用域限定符::。
    * <font color = 'red'>自动变量管理：</font>
      自动变量管理的常用方法是留出一块内存，将其视为栈，以管理变量的增减。程序使用两个指针来管理栈，一个指针指向栈底——栈的开始位置，一个指针指向栈顶——下一个可用的内存单元。当函数被调用时，其自动变量将被加入到栈中，栈顶指针指向变量后面的下一个可用的内存单元。函数结束的时候，栈顶指针被重置为函数被调用前的值，从而释放新变量使用的内存。
    * 寄存器变量
       关键词register最先是由C语言引入的，它建议编译器使用CPU寄存器来存储自动变量
2. 静态持续变量
   和C语言一样，C++也提供了三种链接性给静态存储持续变量：外部链接性(可以在其他文件中访问)、内部链接性(只能在当前文件中访问)和无链接性(只能在当前函数或代码块中访问)。
   这三种链接性都在整个程序执行期间存在，与自动变量相比，它们的寿命更长。编译器将分配固定的内存块来存储所有的静态变量。
   1. 创建链接性为外部的静态持续变量
      必须在代码块的外面声明它
   2. 创建链接性为内部的静态持续变量
      必须要在代码块的外面加上static限定符
   3. 创建没有链接性的静态持续变量
      必须在代码块内声明它，并且使用static限定符
      ```cpp
      int global = 5000;//static duration,extern linkage
      static int one_file = 50;//static duration, internal linkage
      void func1(int n)
      {
        static int cnt = 0;//static duration,no linkage
      }
      ```
   * 所有静态持续变量都有下述初始化特征：
     * 未被初始化的静态变量的所有位都被设为0。这种变量被称为0初始化的。
     * 除了默认的零初始化外，还可以对静态变量进行常量表达式初始化和动态初始化
   * 外部链接性：
      * 在每一个使用外部变量的文件中，都必须声明它，如果要在多个文件中使用外部变量，只需要在一个文件中包含该变量的定义(单定义规则)，但在使用该变量的其他所有文件中，都必须使用extern关键字声明，<font color = 'red'>如果在不同文件中定义了两个相同名称的外部变量会报错。</font>
          ```cpp
          file01.cpp
          extern int cats = 20;
          //defination because of initialization
          int dogs = 22;
          //also a defination
          int fleas;
          //also a defination
          
          file02.cpp
          //use cats and dogs from file01.cpp
          extern int cats;
          extern int dogs;
          //not defination because they use extern and have no initialization
          
          file03.cpp
          //use cats,dogs,and fleas from file01.cpp
          extern int cats;
          extern int dogs;
          extern int fleas;
          ```
       * 如果在函数中定义了与外部变量同名的局部变量，则外部变量被隐藏，如果需要调用外部变量需要使用作用域限定符
           ```cpp
           extern double warming;
              
           void local()
           {
             using std::cout;
             using std::endl;
             double warming = 0.8;
             cout<<"local : "<<warming<<endl;
             cout<<"global : "<<::warming<<endl;
             //使用::warming来调用外部变量
           }
          ```
    * 内部链接性：
    将static限定符用于作用域为整个文件的变量时，该变量的链接性将为内部的。
      * 如果文件定义了一个静态外部变量，其名称与另一个文件中声明的常规外部变量相同，则在该文件中，静态变量将隐藏常规外部变量。
      * 当一个变量为内部链接性的时候，我们可以在一个文件中的多个函数之间共享数据。
    * 无链接性：
    将static限定符用于代码块中定义的变量，在代码块中使用static时，将导致局部变量的存储持续性为静态的。
      * 虽然局部静态变量虽然只在该代码块中可用，但是它在该代码块不活动时仍然存在。
      * <font color = 'red'>在两次函数调用之间，静态局部变量的值保持不变。</font>
      * <font color = 'red'>如果初始化了静态局部变量，则程序只在启动时候进行一次初始化。以后再次调用函数的时候，将不会像自动变量那样再次被初始化。</font>
        ```cpp
        int count(int n = 1)
        {
          static int cnt = 0;
          cnt+=n;
          return cnt;
        }
        
        using std::cout;
        using std::endl;
        cout<<count()<<count(2)<<count(3)<<endl;
        // cnt只会初始化一次，后续调用函数时cnt的值被保留。
        ```

##### chapter 4.3 说明符与限定符 
<b>C++中的存储限定符:</b>

* auto(c++11中被弃用，不再充当存储限定符)
* register
* static
* extern
* thread_local
* mutable
  

<b>C++中的CV-限定符:</b>
* const
* volatile

1. <b>mutable</b>
   mutable指出即使类对象是常对象,其某个成员也可以被修改
    ```cpp
    class Data
    {
      public:
        std::string name;
        mutable int accesses;
        Data(const char* a,int b):name(a),accesses(b) {}
    }
   
    const Data veep("Xiaoming",12);
    veep.accesses = 13;//valid
    ```
2. <b>再谈const</b>
   1. 在c++中，const限定符对默认存储类型有影响。在默认情况下全局变量的链接性是外部的，但是<font color= 'red'>const全局变量的默认链接性是内部的。也就是说，全局const定义就像使用了static说明符一样</font>
   2. 原因主要在于<font color= 'blue'>我们常常把const常量放在头文件中</font>，如果const常量是外部链接性，就会因为单变量原则报错。
   3. 如果我们希望某个常量是外部链接性的，我们需要在定义这个常量的时候加上extern。
3. <b>volatile</b>
    关键字volatile表明，即使程序代码没有对内存单元进行修改，其值也有可能发生变化。
    例如可以将一个指针指向某个硬件的位置，硬件有可能会修改其中的内容。

##### chapter 4.4 函数和链接性
* 和C语言一样，C++不允许在一个函数中定义另外一个函数，因此所有的函数的存储持续性都是静态的。
* 在默认情况下，函数的链接性是外部的，也就是说函数可以在文件中共享。
* 我们可以使用关键字static将函数的链接性设置为内部的，使之只能在一个文件中使用。和变量一样，内部链接性的函数会覆盖外部定义。
* 单定义原则使用于函数，因为非内联函数默认是外部链接性的，所以在多个文件中只能有一个函数定义，也就是说，<font color= 'red'>我们不能把非内联函数的定义放在头文件中。</font>
* 内联函数的定义可以放在头文件中，因为内联函数较为特殊，不受单定义原则约束。不过cpp要求一个函数的所有内联的定义相同。

##### chapter 4.5 名称空间

* 在默认情况下，在命名空间中声明的名称链接性是外部的。(除非他引用了常量)
* 除了用户定义的名称空间之外，还存在一个全局名称空间。它对应于文件级声明区域。
* 通过作用域解析符来限定名称空间中的名称。
* using声明使得特定的名称添加到它所属的声明区域中。
* using编译指令使名称空间中所有名称都被添加到声明区域中。
* 在函数外面使用using声明/编译指令，将会把对应的名称添加到全局名称空间中。
* 假设名称空间和声明区域定义了相同的名称。<font color = 'red'>如果试图使用using声明将名称空间的名称导入该声明区域，则这两个名称会发生冲突，从而CE；如果使用using编译指令导入的名称中有与局部名称冲突的，局部名称将会覆盖掉全局版本。</font>
   ```cpp
   using namespace Jill;
   double fetch;
   //不会报错局部自动变量fetch将会覆盖掉Jill::fetch
   
   using Jill::fetch;
   double fetch;
   //将会报错
   ```
* 未命名的名称空间不能再其他文件中使用，因此在未命名的名称空间中定义的变量实际上成了静态内部链接变量。
* 名称空间用法及其注意点。
   * 使用在已命名的名称空间中声明的变量，而不是使用外部链接全局变量。
   * 使用在未命名的名称空间中声明的变量，而不是使用内部链接全局变量。
   * 不要在头文件中使用using编译指令。
     * 首先，这样做掩盖了哪些名称可用。
     * 其次，包含头文件的顺序可能会影响程序的行为。
   * 导入名称时，首选使用作用域解析符或using声明的方法。减少使用using编译指令。
   * 对于using声明，首选将其作用域设置为局部而不是全局。

### Section 2 CPP 面向对象

#### chapter 5 对象和类
##### chapter 5.1 类的声明与定义

* 类规范有两个部分组成:
   1. 类声明: 以数据成员的方式描述数据部分，以成员函数(被称为方法)的方式描述公共接口。
   2. 类方法定义:描述如何实现类成员函数。

* 可以在类内定义(类声明的时候定义)，也可以在类外定义，在类外定义的时候必须要加上作用域限定符。一般把类声明放在头文件里面，类方法定义放在原文件里面。
* 定义位于类声明内的函数(类内定义)都将成为内联函数。
* 类方法可以访问本类的private成员。
* 类成员函数的原型一般如下:
   ```cpp
   returntpye className::methodName (arguments list);
   ```
* 典型的类声明格式如下:
   ```cpp
   class className
   {
     private:
      data member declarations;
     public:
      member methods prototypes;
   }
   ```
##### chapter 5.2 类的构造函数与析构函数
###### chapter 5.2.1 声明和定义构造函数
构造函数没有返回值
```cpp
Stock::Stock(arguments list):initial_list {}
//类外定义构造函数
```
###### chapter 5.2.2 使用构造函数
C++提供了4种使用构造函数来初始化对象的方式:
1. 显式地调用构造函数:
   ```cpp
   Stock food = Stock("World Cabbage",250,1.25);
   ```
2. 隐式地调用构造函数:
   ```cpp
   Stock food("World Cabbage",250,1.25);
   Stock food2;//default constructor
   ```
3. 类构造函数与new运算符配合使用:
    ```cpp
    Stock* pstock = new Stock("World Cabbage",250,1.25);
    ```
    这条语句创建一个Stock对象，将其初始化为参数提供的值，并且将该对象的地址赋给pstock指针。但是这种情况下对象没有名称，我们使用指针来管理对象。
4. C++11 列表初始化
    ```cpp
    Stock stock1 = {"World Cabbage",250,1.25};
    Stock stock2{"World Cabbage",250,1.25};
    ```
* 注意:赋值与构造不同！
   ```cpp
   Stock stock1 = Stock("World Cabbage",250,1.25)
   Stock stock2;
   stock2 = Stock("World Cabbage",250,1.25)
   //stock1是显式初始化
   //stock2先调用了默认构造函数，然后创建了一个匿名Stock对象并且赋值给了stock2
   ```
###### chapter 5.2.3 默认构造函数
默认构造函数是在未提供显式初始值时，用来创建对象的构造函数。
创建默认构造函数的方式有两种:
1. 给已有的构造函数的所有参数提供默认值
2. 定义一个没有参数的构造函数

* 调用默认构造函数的时候，不要加括号!
   ```cpp
   Stock* prelief = new Stock;//显式调用
   Stock third;//隐式调用
   ```

###### chapter 5.2.4 析构函数基础
* 析构函数没有参数，也没有返回值和声明类型。
```cpp
Stock::~Stock() {}
```
* 析构函数调用时机:
   * 如果创建的是静态存储类对象，析构函数将在程序结束的时候被调用；
   * 如果创建的是自动存储类变量，析构函数将在程序执行完代码块的时候被调用；
   * 如果通过new来创建的，则它将驻留在内存中，当使用delete来释放内存的时候，其析构函数将自动调用。

##### chapter 5.3 const成员函数
对于成员函数来说，都会有一个指向调用自己的对象的一个隐式参数。如果我们要声明这个隐式参数为const，那么这个函数就是const成员函数。因为隐式参数不在参数列表中指明，因此我们无法像其他参数一样直接声明为const。c++将这个const置于函数括号的后面。
``` cpp
void show()const;//函数声明

void Stock::show()const{};//函数定义
```

* 只要类方法不修改调用对象，就应该将其声明为const。
  
##### chapter 5.4 this指针
C++使用this指针来指向调用成员方法的对象。this指针被作为隐式参数传递给方法。
``` cpp
const Stock& Stock::topval(const Stock& s)const
{
  if(s.val >= this->val) return s;
  else return *this;
}
```

##### chapter 5.5 对象数组
初始化对象数组的方案是：
   * 首先使用默认构造函数创建数组元素。
   * 然后花括号中的构造函数将创建临时对象。
   * 然后临时对象中的内容将被赋值到相应的元素之中。
     ```cpp
     Stock stocks[100] = 
     {
       Stock();
       Stock("World Cabbage",250,1.25)
     }
     //剩余的98个元素将使用默认构造函数初始化。
     ```
     因此，要创建对象数组，这个类必须要有默认构造函数。

##### chapter 5.6 类作用域
作用域为整个类的名称只在该类中是已知的，在类外是不可知的。

###### chapter 5.6.1 作用域为类的常量
1. 在类中声明一个枚举
    ```cpp
    class Bakery
    {
      private:
        enum {Months = 12};
        double costs[Months];
    }
    ```
    声明枚举并不会创建数据成员。也就是说，所有对象中都不包含枚举。
2. 使用关键字static
    ```cpp
    class Bakery
    {
      private:
        static const int Months = 12;//只有整型和枚举量才能在类声明中初始化。
        double costs[Months];
    }
    ```

###### chapter 5.6.2 作用域内枚举(C++11)

* 传统的枚举可能会造成枚举量冲突的问题:
   ```cpp
   enum egg {Small,Medium}
   enum t_shrit {Small,Medium}
   ```
   这里会发生编译错误。

* C++11提供了一种新的枚举，其枚举量的作用域为类:
   ```cpp
   enum class egg {Small,Medium}
   enum class t_shrit {Small,Medium}
   
   egg choice = egg::Medium;
   t_shirt choice2 = t_shirt::Medium;
   ```
   我们需要用作用域限定符来调枚举量

##### chapter 5.7 静态类成员

* 静态类成员有一个特点：
无论创建了多少对象，程序都只创建一个静态类变量副本。

* <font color = 'red'>不能在类声明中初始化静态成员变量</font>(除非是const整型和枚举量)：
   因为声明描述了如何分配内存，但并不分配内存。
   对于静态类成员，可以在类声明外使用单独的语句进行初始化。注意，类外初始化不需要加static。
    ```cpp
    class Time
    {
      public:
          static const int N = 50;//valid
          //static double t = 0.5;//invalid,报错!
          static double eps ;
    }
   
    double Time::eps = 1e-6; // 类外初始化
    ```

##### chapter 5.8 静态类成员函数
可以将成员函数声明为静态的(函数声明必须加static，类外定义不用加)。
实际上，静态成员函数就是<font color = 'red'>没有隐式参数的成员函数，也就是静态成员函数不传递this指针</font>
1. 不能通过对象调用静态成员函数。
    原因自然就是没有this指针，函数不知道调用对象的信息。如果静态成员函数是Public的，那么我们可以使用类名和作用域解析运算符来调用它。
2. 静态成员函数只能调用静态成员。
    因为没有this指针，无法调用普通类成员。

##### chapter 5.9 总结
* 使用oop方法解决编程问题的第一步是根据它与程序之间的接口来描述数据。从而指定如何使用数据。然后设计一个类来实现该接口。一般来说，私有数据成员存储信息，公有成员函数(又称为方法)提供访问数据的唯一途径。类将数据和方法合成一个单元，其私有性实现数据隐藏。
* 通常将类声明分成两部分组成，这两部分保存在不同的文件之中。类声明(包括由函数原型表示的方法)应该放在头文件中。定义成员函数的源代码放在方法文件中。这样便将接口描述和实现细节分开了。
* 每个类对象存储自己的数据，共享类方法。
* 如果希望成员函数对多个对象进行操作，可以将额外的对象作为参数传递给它。如果方法需要显式地引用调用它的对象，则可以使用this指针。
* 由于this指针被设置为调用对象的地址，因此*this是调用对象的别名。在函数括号后面加上const可以指定this指针为const变量。

#### chapter 6 运算符重载、友元与类的类型转换
##### chapter 6.1 运算符重载
###### chapter 6.1.1 运算符重载引入
* 运算符重载一般有成员函数与友元函数两种形式。
* 不要返回自动变量的引用! 因为在代码块结束时候自动变量就会被释放。
* 在运算符表示法中，运算符左侧的对象是调用调用对象，作为隐式参数用this指针传递；运算符右侧的对象是作为显式参数被传递的对象。
   ```cpp
   Time time::operator+(const Time& other)const
   {
     Time result;
     result.minute = this->minute+other.minute;
     result.second = this->second+other.second;
     return result;
   }
   
   Time fixing(10,30);
   Time coding(10,20);
   Time res;
   res = fixing+coding;
   //等价于
   res = fixing.operator+(coding);
   ```
###### chapter 6.1.2 运算符重载的限制
   1. 重载后的运算符必须至少有一个操作数是用户定义的类型，这将防止用户为标准类型重载运算符。
   2. 不能把双目运算符重载为单目运算符，反之同理。
       注："-"运算符既是单目运算符也是双目运算符。
   3. 不能修改运算符的优先级
   4. 不能创建新的运算符
   5. 不能重载下面的运算符:
       * sizeof  sizeof运算符
       * .  成员运算符
       * ::  作用域解析符
       * ?:  三目运算符
       * typeid  
       * const_cast
       * dymanic_cast
       * reinterpret_cast
       * static_cast
   6. 下面几种运算符只能通过成员函数重载
       * =
       * ()
       * []
       * ->

###### chapter 6.1.3 运算符重载与友元函数

* 对于运算符重载来说，可以有成员函数与非成员函数的两种重载实现方式。一般来说，非成员函数应是友元函数，这样才便于其访问私有数据。
* 非成员版本的重载运算符函数所需要的形参数目与运算符使用的操作数相同；而成员函数版本所需的参数数目少一个，因为其中的一个操作数是被隐式地传递的调用对象。

###### chapter 6.1.4 运算符重载与类的类型转换

若要实现一个其他类型与类类型相加，有两种方法。

下面以Stonewt类类型和double类型作为例子:
1. 将下面的函数定义为友元函数,让Stonewt(double)构造函数将double类型的参数转换为Stonewt类型的参数
   ```cpp
   friend Stonewt operator+(const Stonewt&,const Stonewt&);
   Stonewt(double lbs = 0.0);
   ```
2. 将运算符重载为一个显式使用double类型的参数
   ```cpp
   Stonewt operator+(double);
   friend Stonewt operator+(double,const Stonewt&);
   
   stone1 = 1.1 + stone;//与友元函数匹配
   stone1 = stone + 1.1;//与成员函数匹配
   ```
   第一种方法依赖于隐式转换，优点是函数编写简单，缺点是每次需要转换时都需要调用构造函数创建临时变量、
   第二种方法优点是运行速度快，但是函数冗长。

###### chapter 6.1.5 运算符重载与动态内存分配

动态内存分配通常与运算符重载配合使用。具体来说，一般会进行赋值运算符重载和[]运算符重载实现深拷贝。
详细见第七章。

###### chapter 6.1.6 比较运算符重载

CPP常常进行比较运算符的重载，以实现自定义的大小比较。
``` cpp
bool operator<(const Time& a, const Time& b)
{
  if(a.hour < b.hour) return true;
  else if(a.hour > b.hour) return false;
  else
  {
    if(a.minute < b.minute) return true;
    else return false;
  }
}

bool Time::operator>(const Time& other)const
{
  return other < *this;
}
```

###### chapter 6.1.7 []运算符重载

* 在cpp中,[]运算符是二元运算符，一个操作数位于第一个中括号的前面，第二个操作数位于中括号的中间。
* []运算符只能用成员函数的形式重载。
   ```cpp
   char& myString::operator[](int i)
   {
     return this->str[i];
   }
   const char& myString::operator[](int i)const
   {
     return this->str[i];
   }
   ```

###### chapter 6.1.8 前缀后缀自增自减运算符重载

* 后缀运算符需要传入一个参数int来标识，本身并无意义。
* <font color = 'red'>前缀运算符返回引用，后缀运算符返回对象。</font>
  前缀运算符是先改变自身再返回，所以为了提高效率可以返回引用；但是后缀运算符是先返回后改变自身，所以返回的是临时变量，不能返回引用。

   ```cpp
   Time& Time::operator++()
   {
     this->minute = this->minute + 1;
     return *this;
   }
   Time Time::operator++(int)
   {
     Time backup(*this);
     this->minute = this->minute + 1;
     return backup;
   }
   ```
##### chapter 6.2 友元
友元有三种:
1. 友元函数
2. 友元类
3. 友元成员函数
###### chapter 6.2.1 友元函数
通过让函数成为类的友元，可以赋予该函数与类的成员函数相同的访问权限。

在为类重载二元运算符的时候，我们常常需要友元。

因为使用成员函数运算符重载，视运算符左侧的类变量为调用对象，这就造成了如果我们期望左侧不为调用对象的表达式无法通过成员函数运算符重载实现。

这时，我们可以通过定义友元函数，让所有的参数成为显式。因为友元函数可以访问私有数据成员，所以重载运算符函数定义时可以通过显式参数直接访问私有数据成员进行操作，进而达到重载的目的。

创建友元函数的步骤:
1. 将原型放在类声明中，并在原型声明前面加上关键字friend:
    ```cpp
    friend Time operator*(double m ,const Time& t);
    ```
2. 编写函数定义。编写函数定义的时候不需要使用类型限定符::，也不需要在函数前面加上friend关键字。
    ```cpp
    Time operator*(double m,const Time& t)
    {
      Time res;
      long total_minutes = m*t.hours*60+m*t.minutes;
      res.hours = total_mintues / 60;
      res.minutes = total_minutes % 60;
      return res
    }
    ```

友元函数示例:重载<<运算符

```cpp
class Time
{
private:
	int years;
	int months;
public:
	Time(int years = 2022, int months = 1);
	friend std::ostream& operator<<(std::ostream&,const Time&);
};
Time::Time(int years,int months)
{
	this->years = years;
	this->months = months;
}
std::ostream& operator<<(std::ostream& os,const Time& output)
{
	os<<output.years<<" years "<<output.months<<" months "<<std::endl;
	return os;
}

int main()
{
	Time time1 = Time(2022,8);
	Time time2;
	std::cout<<time1<<time2;
}
  
  //注意，这里必须要返回引用
  //因为ostream没有具体的拷贝构造函数
```
###### chapter 6.2.2 友元类

友元类的所有方法都可以访问原始类的私有数据成员和保护数据成员。
友元声明可以位于任何一个访问权限控制符下面，其所在的位置无关紧要。
友元类最关键的问题在于合理使用向前声明：在定义友元类的时候，我们需要在友元类中使用目标类的私有/保护数据成员，但又需要在目标类中声明友元类。这时候就需要使用向前声明技术:
```cpp
class Remote;//forward declaration;
class TV
{
  public:
      friend class Remote;
      ....
  private:
      int channel;
};
class Remote
{
  public:
      void set_channel(TV& t,int c)const
      {t.channel = c;}
}
```

###### chapter 6.2.3 友元成员函数

有时候我们不需要让类中所有成员方法都成为友元。这时候我们可以选择特定的成员函数成为友元。同样的，友元成员函数需要注意使用向前声明技术。

```cpp
class TV;
class Remote
{
  public:
      void set_channel(TV& t,int n)const;
      //该方法需要类外实现，不能内联
      //因为在声明该方法的时候我们还不知道TV数据成员的信息。
};
class TV
{
  public:
      friend void Remote::set_channel(TV& t,int n)const;//友元成员函数
  private:
      int channel;
};
void Remote::set_channel(TV&t,int n)const
{
  t.channel = n;
}
```

##### chapter 6.3 类的类型转换与转换函数
C++语言不自动转换不兼容的类型，在无法自动转换时，需要使用强制类型转换。
```cpp
int* ptr = 10;//invalid
int* ptr = (int*) 10;//valid
```

###### chaptr 6.3.1 其他类型转换为类类型

* 只有一个参数的类构造函数用于将类型与该参数相同的值转换为类类型。
   * 在构造函数中声明explicit可以防止隐式转换
     隐式状况情况(假设构造函数参数为double,类类型为Time)
     * 将Time对象初始化为double时
     * 将double的值赋给Time类对象时
     * 将double值传递给Time类型的参数时
     * 返回值类型为Time类型却试图return double时
  * 如果构造函数有两个及以上个参数的时候，要想将其做为转换函数，必须把除了第一个参数以外的其他所有参数都设置默认值。

###### chaptr 6.3.2 类类型转换为其他类型
* 转换函数可以将类类型转换为其他类型
   * 转换函数无需标识返回类型(但有返回值)，没有参数。
   * 转换函数格式如下:
      ```cpp
      operator Typename() {}
      ```
   * 转换函数必须是类成员函数
   * 使用explicit标识符将禁止隐式转换
   * 过多的转换函数可能会造成二义性
   * 尽量加上explicit标识符，避免意外的隐式转换
* 例子:
     ```cpp
     class Stonewt
     {
      private:
        double weight;
        ...
      public:
        Stonewt(double lbs = 0.0));
        ...
        explicit operator double() const;
        operator int() const;
        ...
     }
     
     Stonewt::Stonewt(double lbs)
     {
       this->weight = lbs;
       ...
     }
     
     Stonewt::operator double()const
     {
       return this->weight;
     }
     
     Stonewt::operator int()const
     {
       return int (this->weight+0.5);
     }


     Stonewt myCat;
     myCat = 19.8; //double转换为Stonewt类型
     double wt = (double) myCat;//Stonewt类型显式强制转换为double
     int wt_z = myCat;
     cout<<wt<<wt_z;
    
     ```
#### chapter 7 动态内存分配和成员初始化列表技术
##### chapter 7.1 C++中的特殊成员函数
C++自动提供了下列成员函数：
1. 默认构造函数，如果没有定义构造函数。
2. 默认析构函数，如果没有定义析构函数。
3. 复制构造函数，如果没有定义。
4. 赋值运算符，如果没有定义
5. 地址运算符，如果没有定义
6. 移动构造函数(c++11)
7. 移动复制运算符(c++11)

###### chapter 7.1.1 默认构造函数
* C++会提供一个默认构造函数，在没有定义任何一个构造函数的情况下。
* 带参数的构造函数也可以是默认构造函数，不过在构造函数声明的时候必须给出所有参数的默认值。
* 只能有一个默认构造函数
* 默认构造函数在调用的时候不要加括号 
   ```cpp
   Time time1;
   //而不是Time time1();
   ```
###### chapter 7.1.2 复制构造函数
* C++在没有定义复制构造函数的情况下会提供一个默认的。通常原型如下:
   ```cpp
   ClassName(const ClassName& other);
   ```
* 每当程序生成对象副本时，都会调用复制构造函数。
   具体而言，有如下情况：
   1. 函数按值传递对象
   2. 函数返回非引用对象
   3. 编译器生成临时对象

* 浅拷贝:
   默认的复制构造函数实现的拷贝被称为浅拷贝。
   比如有一个类Time有一个类成员变量char[20] str，在浅拷贝模式下，若 Time time1(time2) ,内部执行的是time1.str=time2.str，实际上两者指向了同一个地址。这是非常危险的，会造成两者一起变化以及重复析构的问题。

* 深拷贝:
   为了实现指针变量的拷贝，我们需要定义一个深拷贝复制构造函数。
###### chapter 7.1.3 赋值运算符重载
* C++在没有定义赋值运算符重载的情况下会提供一个默认的。通常原型如下:
   ```cpp
   ClassName& operator=(const ClassName& other);
   ```
* 浅拷贝:
   同样的，默认的赋值运算符重载实现的是浅拷贝。
* 深拷贝:
   为了实现指针变量的拷贝，我们同样需要定义一个深拷贝赋值运算符重载。

##### chapter 7.2 动态内存分配

###### chapter 7.2.1 动态内存分配注意事项
如果类在构造函数中使用了new 或者new[]来分配了类成员指向的内存，我们需要注意下列事项:
1. 如果在构造函数中使用new来初始化成员，则必须在析构函数中将其delete。
2. new 和 delete必须相互兼容 。new 对应delete。new[] 对应 delete[]。
3. 如果有多个构造函数，则必须以相同的方式使用new，要么都带上中括号，要么都不带中括号。因为只有一个析构函数，所有的构造函数都必须与其兼容。
4. 应当定义一个复制构造函数，通过深度复制将一个对象初始化为另一个对象。下面是一个例子:
    ```cpp
    string::string(const string& other)
    {
      this->len = other.len;
      this->str = new char[this->len+1];
      std::strcpy(this->str, other.str);
    }
    ```
5. 应当定义一个赋值重载运算符，通过深度复制将一个对象赋值给另一个对象。下面是一个例子:
     ```cpp
     string& string::operator=(const string& other)
     {
       if(this == &other) return *this;
       //为了防止赋值给自己时str被delete无法strcpy，必须先判断是不是自己
       delete[] this->str;
       this->len = other.len;
       this->str = other.str+1;
       std::strcpy(this->str,other.str);
       return *this;
     } 

###### 7.2.2 有关返回对象的说明
1. 返回指向const的引用
   * 使用const引用的常见原因是提高效率，但有一定的限制。具体而言，如果函数返回传递给他的对象，可以返回其引用来提高效率。
   * <font color = 'red'>如果传入的参数的const，要返回它，返回值必须为const!</font>
2. 返回指向非const对象的引用
   * 两种常见的返回非const对象的引用的情形是:重载赋值运算符、重载与cout一起使用的<<运算符。重载赋值运算符返回引用在于提高效率，而后者必须这么做。
   * 对于没有公有复制构造函数的类，若要返回这种类的对象，返回类型必须是引用，否则在返回后会因为无法调用复制构造函数报错。
3. 返回对象
   * 如果返回的对象是被调用函数中的局部变量，则不应该按引用的方式返回它，因为在被调用函数执行完毕时，局部对象将调用其析构函数。当控制权回到调用函数之后，引用指向的对象将不存在。在这种情况下，我们应当返回对象而不是对象的引用。
   * 通常被重载的算术运算符需要返回对象。
4. 返回const对象
   * 如果想要避免返回对象作为临时变量被当成可修改的左值，可以返回const对象。
    ```cpp
    net = time1 + time2;
    //valid 
    time1 + time2 = net;
    //valid if return Type is Time
    cout<<(time1+time2 = net).getYear()<<endl;
    //valid if return Type is Time
   
    //后面两个式子如果operator+函数返回的是const Time将不被允许。
    ```

5. 关于返回对象的总结：
    1. 如果方法或者函数需要返回局部对象，则应返回对象而不能返回引用。在这种情况下，将使用复制构造函数来生成返回的对象。
    2. 如果方法或者函数需要返回一个没有Public复制构造函数的类的对象，他必须返回一个指向这种对象的引用。
    3. 若一个函数既可以返回对象也可以返回引用，则应该返回引用，因为效率更高(如赋值运算符)
    4. 若返回的引用是作为一个const参数传入被调用函数的，则必须返回const引用
    5. 若不希望返回值作为可修改的左值，则需要返回const对象。

##### chapter 7.3 成员初始化列表技术
C++为类构造函数提供了一种可以用于初始化数据成员的特殊语法。这种语法包括冒号和由逗号分隔的初始化列表，被放在构造函数右括号后面，函数体左括号前面。
1. 这些初始化操作是在对象创建时候进行的，此时函数体中的语句还没有执行。
2. 对于非静态const数据成员，必须使用初始化列表技术进行初始化。(静态成员需要在类外初始化)
3. 对于被声明为引用的类成员，也必须通过成员初始化列表技术初始化。
4. 因为无论是const成员变量还是引用类型成员变量，都只能在被创建的时候进行初始化。
5. 数据成员被初始化的顺序与他们出现在类声明的顺序相同。与初始化器中排列顺序无关。
6. 只有在构造函数中能够使用这种技术。
7. 常规变量也可以用这种技术初始化。

    ```cpp
    class Time
    {
      private:
          const char* standard;
          int& belong;
          int hour;
          int minute;
      ...
      public:
          Time(int& o,int hour = 0,int minute = 0);
    }
    
    Time::Time(int& o,int hour,int minute):standard("GMT"),belong(o)
    {
      this->hour = hour;
      this->minute = minute;
    }
    ```

#### chapter 8 单继承与虚函数多态

##### chapter 8.1 公有继承

###### chapter 8.1.1 公有继承特点

* 使用公有继承可以实现"is-a"关系
* 使用公有继承，父类中的公有成员将变成子类中的公有成员，父类中的保护成员变为子类中的保护成员，<font color = 'red'>但是父类中的私有成员在子类中不作为私有成员</font>，而是直接被隐藏，除非调用继承下来的父类方法，否则完全无法访问。(tips:即使是重写父类方法，也无法访问父类中的私有成员)

###### chapter 8.1.2 公有继承的子类构造函数

* 在定义子类构造函数的时候，需要在成员初始化列表中调用父类的构造函数，特别地，如果父类没有默认构造函数，则必须要这样做；如果父类有默认构造函数，如果不在成员初始化列表中显式地调用父类构造函数，那么就会自动调用默认构造函数。
* 除非是虚基类(第九章叙述)，否则<font color = 'red'>不能调用间接基类的构造函数</font>，只能调用直接基类的构造函数。因此，如果有一条继承链，我们会发现构造函数就像递归一层一层找直接基类，找到根类之后再逐层向下构造。
* 标准格式:
   ```cpp
   Sub::Sub(type1 x, type2 y, new_para z): Super(x,y)
   {
     this->z = z;
   }
   ```

###### chapter 8.1.3 公有派生类和基类之间的关系

对于公有继承:
* 基类指针可以在不进行显式类型转换的情况下指向派生类对象；基类引用可以在不进行显式类型转换的情况下引用派生类对象。与java相同，这被称为upcasting(向上转型)。
* 与java相同，向上转型之后只能调用基类的方法，不能调用子类新声明的方法。

###### chapter 8.1.4 protected访问控制符

关键字protected与private相似，在类外只能用公有成员函数来访问protected部分中的类成员。private和protected的区别只有在基类派生的类才会表现出来。派生类的成员可以直接访问基类的保护成员，但不能直接访问基类的私有成员。因此，对于外部世界来说,protected
成员的行为和private成员相似；对于派生类来说，保护成员的行为与公有成员相似。

* 最好对类数据成员使用private控制，不要使用保护访问控制；同时通过基类方法使派生类能够访问基类数据。
* 对于成员函数来说，保护访问控制很有用，他让派生类能够访问公众不能使用的内部函数。

##### chapter 8.2 虚函数与多态

如果我们希望基类引用根据引用变量的类型调用相应的方法(也就是实现多态)，那么我们应该把这些方法声明为虚方法。
* 只有虚函数才能实现多态!如果不定义virtual，那么程序就会根据指针或者引用变量本身的类型调用方法。
* 关键字virtual通常只用于方法原型中，类外定义方法时不要加。
* 在基类声明了virtual后，后面整个继承链该方法都默认为虚方法，不需要再声明virtual。不过在子类对应的方法前加上virtual也不会报错，反而可以更加清晰。
* 在派生类中，如果想要调用基类的同名方法，需要使用作用域限定符。(与java不同，java是使用super关键字)
* 例子:
```cpp
#include "bits/stdc++.h"

class Super
{
private:
	int num1;
	int num2;
public:
	Super(int num1 = 0,int num2 = 0)
	{
		this->num1 = num1;
		this->num2 = num2;
	}
	virtual void show()const
	{
		std::cout<<this->num1<<" "<<this->num2<<std::endl;
	}
	virtual ~Super(){}
}; 

class Sub : public Super
{
private:
	int num3;
	int num4;
public:
	Sub(int num1 = 0,int num2 = 0,int num3 = 0,int num4 = 0):Super(num1,num2)
	{
		this->num3 = num3;
		this->num4 = num4;
	}
	virtual void show()const
	{
    this->Super::show();
    //通过作用域限定符调用父类同名方法。
    //不这么做会递归!
		std::cout<<this->num3<<" "<<this->num4<<std::endl;
	}
	virtual ~Sub(){}
};

int main()
{
	const Super& ptr = Super(1,2);
	//Super(1,2)返回了一个临时变量，对于引用类型，只有const引用才能引用临时变量
	//所以Super& ptr = Super(1,2)会报错 
	const Super& ptr1 = Sub(1,2,3,4);
	ptr.show();
	ptr1.show();
	return 0;
}
/*输出:
1 2
1 2
3 4
*/
```

##### chapter 8.3 虚析构函数

<font color = 'red'>基类必须使用虚析构函数!</font>
* 如果析构函数不是虚函数，那么将只会调用对应指针或者引用类型变量的析构函数。
* 只有基类定义了虚析构函数，才能实现多态析构，程序将根据指针指向的实际类型(或者引用引用的实际类型)调用相应的析构函数。
* 我们完全可以把所有析构函数都定义为虚函数，从而避免错误。
  
##### chapter 8.4 虚函数与动态绑定

将源代码中的函数调用解释为执行特定的函数代码块被称为函数绑定(函数联编)。在编译过程中进行的联编被称为静态联编；在程序运行时进行的联编被称为动态联编。

* 将派生类引用或者指针转换为基类引用或者指针被称为向上转型(upcasting)。公有继承向上转型可以隐式进行。
* 将基类指针或者引用转换为派生类指针或者引用被称为向下转型(downcasting)。如果不使用显式强制转换，向下转型时不允许的。
* 编译器对虚方法使用动态绑定，对非虚方法使用静态绑定。

##### chapter 8.5 虚函数的工作原理

* 编译器处理虚函数的方法是:给每一个对象添加一个隐藏成员。隐藏成员中保存了一个指向函数地址数组的指针。这种数组被称为虚函数表。虚函数表中存储了为类对象进行声明的虚函数的地址。
* 每一个对象无论有多少个虚函数，只会增加一个地址成员。区别是这个地址指向的数组(虚函数表)大小不同罢了。
* 调用虚函数时，程序将查看存储在对象中的虚函数表地址，然后转向相应的函数地址表，找到应该调用的函数。因此，使用虚函数会造成一定程度上的时间与空间开销。

##### chapter 8.6 虚函数注意事项

* 基类析构函数必须为虚函数。构造函数和友元函数不能为虚函数。只有成员函数才能设置为虚函数。
* 与java不同，虚函数在派生类中重新定义不会生成函数的两个重载版本，不管参数列表如何，同名的基类方法都会被掩盖。同时，如果有多个基类同名的重载方法，在派生类中只重写了一个，那么剩下几个就会被掩盖。
   ```cpp
   class A
   {
     public:
        virtual void showperks(int a)const;
        virtual void showperks(double x)const;
        virtual void showperks()const;
   }
   class B : public A
   {
     public:
        virtual void showperks(int a)const;
        virtual void showperks(double x)const;
        ...
   }
   class C : public A
   {
     public:
        virtual void showperks(int a,int b)const;
        ...
   }
   
   int main()
   {
     C c;
     c.showperks(1,2);//valid;
     c.showperks(1);//invalid;
     B b;
     b.showperks(1);//valid;
     b.showperks();//invalid;
   }
   ```
* 因此，虚函数有两个一般会被采用的规则:
   * 如果重新定义继承的方法，应当确保与原来的原型完全相同。否则原来的原型会被隐藏。
   * 如果基类声明被重载了，则应当在派生类中定义所有的基类版本。否则漏掉的版本会被隐藏。

##### chapter 8.7 纯虚函数与抽象基类

C++使用纯虚函数(pure virtual fuction)提供未实现的函数。纯虚函数声明的结尾处为=0。(相当于java中的abstract方法)

* 当类声明中包含纯虚函数的时候，不能创建该类的对象。(相当于java中abstract类不能有实例)
* 但是我们可以创建这个类的变量，比如指针变量。同样的，我们可以把该指针指向这个类能够实例化的子类，从而实现多态。(与java相同)

例子:
```cpp
#include "bits/stdc++.h"

class Super
{
private:
	int num1;
	int num2;
public:
	Super(int num1 = 0,int num2 = 0)
	{
		this->num1 = num1;
		this->num2 = num2;
	}
	virtual void show()const = 0;
	virtual ~Super(){}
}; 

class Sub : public Super
{
private:
	int num3;
	int num4;
public:
	Sub(int num1 = 0,int num2 = 0,int num3 = 0,int num4 = 0):Super(num1,num2)
	{
		this->num3 = num3;
		this->num4 = num4;
	}
	virtual void show()const
	{
		std::cout<<this->num3<<" "<<this->num4<<std::endl;
	}
	virtual ~Sub(){}
};

int main()
{
	//Super super1(1,2); 
	//报错，抽象类不能实例化。 
	const Super& ptr = Sub(1,2,3,4);
	ptr.show();
	return 0;
}
```

##### chapter 8.8 继承和动态内存分配

###### chapter 8.8.1 派生类不使用new的情况

* 当派生类不使用new运算符的时候，我们不需要定义显式析构函数，复制构造函数和赋值运算符给派生类。
* 此时派生类将自动使用基类的复制构造函数和基类的赋值运算符对继承来的基类类组件进行复制构造或者赋值。

###### chapter 8.8.2 派生类使用new的情况
* 在这种情况下，必须为派生类显式地定义析构函数，复制构造函数和赋值运算符。
  1. 派生类析构函数自动调用基类的析构函数(在函数的末尾，与构造顺序相反)，所以其自身的职责是对派生类构造函数执行工作的清理。
  2. 对于派生类复制构造函数来说
     * 必须要在初始化列表中调用基类的复制构造函数。因为派生类构造函数的参数是派生类的一个const对象，所以这里在传参给基类构造函数的时候运用了向上转型。
     *  经典模板如下:
        ```cpp
        hasDMA::hasDMA(const hasDMA& other):baseDMA(other)
        {
            this->style = new char[std::strlen(other.style)+1];
            std::strcpy(this->style,other.style);
        }
        ```
  3. 对于派生类赋值运算符来说
     * 必须显式调用基类赋值运算符来完成工作。
     * 经典模板如下:
       ```cpp
       hasDMA& hasDMA::operator=(const hasDMA& other)
       {
         if (&other == this) return *this;
         this->baseDMA::operator=(other);
         //upcasting,显式调用基类赋值运算符。
         delete[] this->style;
         this->style = new char[std::strlen(other.style)+1];
         std::strcpy(this->style,other.style);
         return *this;
       } 
       ```
* 总之，当基类和派生类都是用动态内存分配的时候，派生类的析构函数、复制构造函数、赋值运算符都必须使用相应的基类方法来处理元素。对于析构函数，自动在函数末尾调用基类析构函数；对于复制构造函数，是通过成员初始化列表技术完成的；对于赋值运算符，是通过使用作用域解析运算符显式地调用基类的赋值运算符来完成的。

##### chapter 8.9 私有继承与保护继承

###### chapter 8.9.1 私有继承

* private继承之后，基类的public和protected字段和方法在子类中变为private数据成员,基类的private数据成员在子类中完全无法直接访问。
* 私有继承无法使用隐式的向上转型。换句话说，就是私有继承无法实现多态。在不进行强制类型转换的前提下，不能将指向派生类的引用或者指针赋给基类引用或指针。

###### chapter 8.9.2 保护继承

* protected继承之后，基类的public和protected字段和方法在子类中变为protected数据成员,基类的private数据成员在子类中完全无法直接访问。
* 保护继承仅限在派生类内可以使用隐式的向上转型，在类外无法使用隐式的向上转型。

###### chapter 8.9.3 私有继承与保护继承区别

假设第一代祖先在给第一代派生类分别使用了私有继承和保护继承
* 在第一代派生类中，私有继承后派生类的行为与保护继承后派生类的行为没有明显的差别。
* 第第二代派生类中，第一代的私有继承会使得第一代的祖先的所有数据都完全无法直接访问，但是保护继承不会这样。

##### chapter 8.10 访问权限的重定义
1. 第一种方法是定义一个使用基类被继承下来的派生类方法。
    ```cpp
    class Teacher
    {
      public:
          double sum()const;
    }
    class Student : private Teacher
    {
      public:
          double sum()const;
    }
    
    double Student::sum()const
    {
       return this->Teacher::sum();
    }
    ```
2. 第二种方法是使用using声明(常用)
    注意，using声明只使用成员名，不用加圆括号、函数参数、返回类型。
   ```cpp
    class Teacher
    {
      public:
          double sum()const;
    }
    class Student : private Teacher
    {
      public:
          using Teacher::sum;
    }
   ```

#### chapter 9 虚基类多重继承与类组合、类嵌套

##### chapter 9.1 多重继承(MI)

MI描述的是有多个直接基类的类。与单继承一样，公有的MI表示的也是"is a"关系。公有MI的表达方式如下:
```cpp
class Sub : public Sup1,public Sup2
{
    ...
}
```
MI的引入给cpp带来了一些新的问题，其中两个显著的问题是:
1. 从两个不同的基类继承了同名方法
2. 从两个或者多个相关基类那里继承同一个类的多个实例
   
```cpp
class worker
{
  private:
      std::string fullname;
      long id;
  public:
      worker();
      worker(const std::string& other, long n);
      virtual ~worker()=0;
      virtual void set()=0;
      virtual void show()const = 0;
}

class waiter : public worker
{
  private:
      int panache;
  public:
      waiter();
      waiter(const std::string& other, long n,int p = 0);
      Waiter(const std::worker& other, int p =0);
      void set();
      void show()const;
}

class singer : public worker
{
  protected:
      enum{other, alto, contralto, soprano, bass, baritone, tenor};
      enum(Vtypes = 7);
  private:
      static char* pv[Vtypes];
      int voice;
  public:
      singer();
      singer(const std::string& s, long n, int v =other);
      singer(const worker& w,int v =other);
      void set();
      void show()const;
}

class singingWaiter : public singer,public waiter
{
  ...
}
```

* 上述这个例子看起来是可行的，但实际上有很多问题。
* 我们注意到,singer和waiter都有一个worker组件，这样带来一个很严重的问题，就是在singingwaiter向上转型成worker时，会引发二义性问题。这样导致了我们必须通过强制类型转换来取消二义性。
   ```cpp
   singingwaiter sw ;
   worker* w = &sw;//二义性，invalid;
   worker* w = (waiter*) &sw;//强制类型转换消除二义性。
   ```

##### chapter 9.2 虚基类

为了解决上一节MI面临的问题，cpp引入了虚基类技术。

虚基类使得从多个类(他们的基类相同)进一步派生出的对象只继承一个基类对象。我们通过在类声明时引入virtual来声明虚基类。virtual与访问权限控制符的顺序无关紧要。
```cpp
class singer: virtual public worker{...};
class waiter: public virtual worker(...);
class singingwaiter : public singer,public waiter{...};
```

###### chapter 9.2.1 引入虚基类后的构造函数规则

使用虚基类时，构造函数规则相比于普通的继承发生了一些改变。

* C++在基类是虚的时候，禁止信息通过中间类传递给基类。
  因为多重继承有多条继承链，而使用虚基类后只有一个基类对象。我们在调用构造器的时候，有可能会出现沿着不同的继承链初始化给基类对象的信息不一致(不使用虚基类不会出现这个问题，因为不使用虚基类的话就会有多个基类对象组件，具体参见上一节)。
* 如果不希望默认构造函数来构造虚基类对象，我们需要显式地在成员初始化参数列表中调用虚基类的构造函数。
   这与普通类完全不同！普通的单继承是不允许在初始化列表中调用间接基类的构造函数的！

```cpp
singingWaiter::singingWaiter(const worker& w,int p = 0 ,int v = 0):worker(w),waiter(w,p),singer(w,v){}
```

再次强调一遍，调用非直接基类的构造函数在非虚基类的情况下是非法的!

###### chapter 9.2.2 引入虚基类后的方法优先性规则

* 派生类的名称优先于直接或者间接祖先类中的相同名称。
* 不使用虚基类的话，如果类从不同的类继承了两个或者更多的同名字段或者方法，则使用成员名时，如果不使用作用域限定符将导致二义性问题。(java中也相似,但是java中有类优先原则，也就是class优先于interface)
* 使用虚基类后，如果某个名称优先于其他所有名称，则使用它的时候，不需要使用作用域限定符。
* 这个规则与访问权限控制无关。

##### chapter 9.3 类组合

通常，我们使用类组合技术来实现"has a"关系。

所谓的类组合，实际上就是指创建一个包含其他类对象的类。
```cpp
class student
{
  private:
      std::string name;
      std::valarray<double> scores;
};
```
使用类组合技术，我们可以通过被包含的类的对象调用其公有成员方法。

##### chapter 9.4 嵌套类

在c++中，可以把类的声明放在另一个类中。在另一个类中声明的类被称为嵌套类。

* 当且仅当声明位于公有部分，才能在包含类的外面使用嵌套类，而且必须要使用作用域解析符。
* 组合类与嵌套类的区别在于，组合类意味着将一个类的对象作为另外一个类的成员，而对类进行嵌套不创建类成员，而是定义了一种类型，该类型仅在包含嵌套类声明的类中有效。
* 嵌套类声明的位置决定了嵌套类的可见性，嵌套类成员声明的位置决定了嵌套类成员的访问控制权限。
```cpp
class Queue
{
  public:
      class Node
      {
        public:
            Item item;
            Node* next;
            Node(const Item& );
      }
};

Queue::Node::Node(const Item& other):item(other),next(nullptr){}

```


#### chapter 10 RTTI、类型转换运算符和异常

##### chapter 10.1 RTTI 

RTTI是运行阶段类型识别的简称。
C++ 有三个支持RTTI的元素：
1. dymanic_cast<>
2. typeid
3. type_info

###### chapter 10.1.1 dynamic_cast

dymanic_cast是最常用的RTTI组件。
```cpp
Type* new_ptr = dymanic_cast<Type* >(ptr);
```
对于dymanic_cast来说，只有ptr的类型是Type的子类型的时候才能转换成功，否则将返回0。我们可以用dymanic_cast运算符来进行安全的向上转型。
```cpp
if(ps = dynamic_cast<Super* >(ptr))
{
  ps->base_method();
  ...
}
```
也可以将dymanic_cast运算符用于引用，不过其用法有一些不同：因为没有与空指针对应的引用值。因此无法使用特殊的引用值来指示失败。当请求不正确的时候，dymanic_cast<>将引发类型为bad_cast的异常。
```cpp
#include "typeinfo"
...
try
{
  Super& rs = dymanic_cast<Super& >(rg);
  ...
}
catch(bad_cast &)
{
  ...
};
```

###### chapter 10.1.2 typeid运算符和type_info类

* typeid运算符使得能够确定两种对象是否为同一种类型。他与sizeof有些像，可以接受两个参数:类名和结果为对象的表达式。
* typeid运算符返回一个对type_info对象的引用，其中，type_info是在头文件typeinfo中定义的一个类。type_info重载了==和!=运算符，因此我们可以使用这两个运算符对类型进行比较。
   ```cpp
   if(typeid(Super) == typeid(*ptr))
   {
     ...
   }
   //如果ptr是一个空指针，将引发bad_typeid异常!
   ```
* type_info类通常包含一个name()成员方法，该方法返回一个字符串，通常是该类的名称。
   ```cpp
   std::cout<<"Now processing type "<<typeid(*pg).name()<<".\n";
   ```

##### chapter 10.2 类型转换运算符

在C++创始人看来,c风格的类型转换太过于松散，容易引发一系列安全问题。所以C++新增了四种类型转换运算符，使得转换过程更加地规范。
1. dymanic_cast
2. const_cast
3. static_cast
4. reinterpret_cast

* dymanic_cast 前面已经介绍过了，用于安全地进行向上转型。如果不为向上转型，对于指针变量将返回0，对于引用变量将抛出bad_cast异常。
* const_cast 用于const类型与vlatile之间的转换。除了这个特征之外，typename和expression的类型必须相同。
   ```cpp
   const_cast<typename>(expression);
   
   const High* pbar = &bar;
   High* ph = const_cast<High*>(pbar);
   ```
* static_cast当且仅当expression可以隐式转换到typename所属的类型时合法。
   这意味着如果我声明了转换函数或者构造函数是explicit的，static_cast将非法。
* reinterpret_cast用于危险的转换，但是也有一定的限制:其一是不能将指针转换为更小的整型和浮点型，其二是不能将函数指针转换为数据指针。

##### chapter 10.3 异常


#### chapter 11 I/O

##### chapter 11.1 C++输入输出概述

* C++程序将输入和输出看做字节流。输入时，程序从输入流中抽取字节；输出时，程序将字节插入到输出流中。
* 通常使用缓冲区可以更高校地处理输入和输出
* iostream文件自动创建了八个流对象(4个用于窄字符流，4个用于宽字符流):
   1. cin对象对应于标准输入流
   2. cout对象与标准输出流对应
   3. cerr对象与标准错误流相对应。这个流没有被缓冲，这意味着信息将被直接发送给屏幕。
   4. clog对象也对应着标准错误流。但是这个流被缓冲了。

##### chapter 11.2 ostream输出流

###### chapter 11.2.1 插入运算符

ostream类重载了<<运算符，在这种情况下，我们把<<运算符称为插入运算符。operator<<返回一个指向ostream对象的引用，使得输出可以被拼接起来。

* ostream类为如下四种指针定义了插入运算符:
   1. const signed char*
   2. const unsigned char*
   3. const char*
   4. void*
   C++使用void*来打印地址的数值表示!
   ```cpp
   using std::cout;
   int eggs = 12;
   const char* amount = "dozen";
   cout<< &eggs;
   cout<< amount;
   cout<< (void*)amount;
   ```

###### chapter 11.2.2 使用cout进行输出

1. put方法
put方法的原型如下:
```cpp
ostream& put(char)
```
因为返回值是引用，所以可以拼接输出。
```cpp
std::cout.put('a').put(66.7);
//aB
```

2. write方法
write方法的原型如下:
```cpp
basic_ostream<charT,traits>& write(const char_type* , streamsize);
```
第一个参数提供了要显示的字符串的地址，第二个参数指出要显示多少个字符。
```cpp
const char* state = "Kaneas";
for(int i=1;i<=std::strlen(state);i++)
{
  cout.write(state,i).put('\n');
}
```
   * 注意write方法并不会在遇到空字符的时候自动停止打印字符，而只是打印指定数目的字符，即使超出了字符串的边界!

3. 刷新缓冲区

我们可以用控制符flush来刷新缓冲区，用控制符endl来刷新缓冲区并插入一个换行符。

```cpp
using namespace std;
int a = 5 ,b = 9;
cout<<a<<flush<<b<<endl;
```

###### chapter 11.2.3 使用cout进行格式化

1. 修改计数系统

使用控制符hex表示16进制,oct表示八进制,dec表示十进制。
```cpp
int n;
cin<<n;
cout<<hex<<n<<"十六进制"<<endl;
cout<<dec<<n<<"十进制"<<endl;
cout<<oct<<n<<"八进制"<<endl;
```

2. 调整字段宽度

我们使用width()成员方法来调整字段宽度。

* width()成员方法只影响将显示的下一个项目，然后字段宽度设置将恢复成为默认值。
* 在默认情况下，采用右对齐的方式，填充字符使用" "。

```cpp
using std::endl;
using std::cout;
for(long i=1;i<=100;i*=10)
{
  cout.width(5);
  cout<<i<<endl;
  cout.width(10);
  cout<<i<<endl;
}
```

3. 填充字符

可以使用fill()成员函数来设置填充字符。与width()方法不同的是，fill()方法的改变将一直有效，直到下一次改变它为止。

4. 设置浮点数的显式精度。

可以使用precision成员方法来控制浮点数的输出精度。

* 在默认模式下，精度是显式的总位数。
   ```cpp
   using std::cout;
   float price1 = 20.4034f;
   cout<<"Furry Friends is $"<<price1<<'\n';
   cout.precision(2);
   cout<<"Furry Friends is $"<<price1<<'\n';
   // Furry Friends is $20.4034
   // Furry Friends is $20
   ```

###### chapter 11.2.4 cout的setf成员函数

cout的setf成员函数能够给我们提供更加强大的控制输出

fmtflags是bitmask类型的typedef名，用于存储格式标记。

setf成员函数有两个原型:一个参数的和两个参数的,setf返回值是以前的fmtflags设置


<b>1. fmtflags setf(famflags)</b>

在一个格式标记参数的原型下，我们常常传入的格式常量有:
```cpp
1. ios_base::boolalpha 
//输入输出bool值
2. ios_base::showbase
//对于输出，使用C++基数前缀(0,0x)
3. ios_base::showpoint
//显示末尾的小数点
4. ios_base::uppercase
//对于十六进制hex，使用大写字母，对于科学计数法，使用E
5. ios_base::showpos
//在正数前面加上+
```
我们可以调用unsetf方法并且在每个标记参数前面加上no来恢复原状:

````cpp
cout.unsetf(ios_base::noshowbase);
````

<b>2. fmtflags setf(famflags，fmtflags)</b>

在两个个格式标记参数的原型下，我们常常传入的格式常量有:
```cpp
1. ios_base::dec,ios_base::basefield
//使用基数10
2. ios_base::oct,ios_base::basefield
//使用基数8
3. ios_base::hex,ios_base::basefield
//使用基数16
4. ios_base::scientific,ios_base::floatfield
//使用科学计数法
5. ios_base::fixed,ios_base::floatfield
//使用定点计数法
6. ios_base::left,ios_base::adjustfield
//使用左对齐
7. ios_base::right,ios_base::adjustfield
//使用右对齐
8. ios_base::internal,ios_base::adjustfield
//符号或者基数前缀左对齐，值右对齐
9. 0,ios_base::floatfield
//使用默认计数法

````
注意，在C++标准中，定点表示法和科学表示法的精度指的是小数的位数，而默认情况下的精度指的是总位数。
我们可以通过如下的例子来实现恢复以前的调用:
```cpp
ios_base::fmtflags old = cout.setf(ios::left,ios::adjustfield);
cout.setf(old,ios_base::adjustfield);
```

##### chapter 11.2.5 iomanip头文件

C++同样在iomanip头文件中定义了一些控制函数。比如setprecision(),setfill()和setw()。不过这些函数在cout的成员函数中都有对应版本，故在这里不加以叙述。

#### chapter 11.3 istream输入流

与ostream相似，istream为signed char*、char*、unsigned char*重载了>>运算符。在这里不过多叙述。

#####  chapter 11.3.1 使用cin>>进行输入

* cin>>跳过空白(空格、换行和制表符)，直到遇到非空白字符。也就是说，它读取从非空白符开始到与目标类型不匹配的第一个字符之间的所有内容。
* 不匹配的字符将留在输入流之中，下一个cin语句将从这里开始读取。

##### chapter 11.3.2 流状态

```cpp
1. ios_base::eofbit 
//如果达到文件尾，则设置为1
2. ios_base::badbit
//如果流被破坏，则设置为1
3. ios_base::failbit
//如果输入操作未能读取预期的字符或输出操作没有写入预期的字符，则设置为1
4. ios_base::goodbit
//前面三个状态位都为零，说明一切顺利
5. cin.eof()、cin.bad()、cin.fail()
//检查对应的状态位是否被设置，如果被设置则返回true。
6. cin.rdstate()
//返回流状态
7. cin.clear(iostate s = ios_base::goodbit)
//将s状态设置为1，其他状态全部清零，默认为goodbit。
8. cin.setstate(iostate s)
//将传入的s状态位设置为1，其他不影响
```

* 只有在流状态完全完好(所有的状态位都被清除的情况下)，cin>>n 才会返回true。
* 如果希望程序在流状态位被设置之后还想继续读取后面的输入，则必须重置流状态位。
   ```cpp
   while(cin>>input){...}
   if(cin.eof()) {...}
   else if(cin.fail()) {...}
   else {...}
   cin.clear();
   //重新设置流状态，但是要注意此时坏输入依然在输入流中。
   while(!isspace(cin.get())) continue;
   //get rid of bad input
   cin>>input;
   ```

##### chapter 11.3.3 istream类方法

对于只读取字符输入，而不会跳过空白也不会进行数据转换的输入被称为非格式化输入。

我们常用如下几种非格式化输入函数：
1. istream& istream::get(char &)
该方法提供不跳过空白的单字符输入功能

常用的形式是cin.get(n),程序把输入的字符传递给参数n。

该方法会调用setstate，因此我们可以像cin>>一样如下使用cin.get(char&):
```cpp
while(cin.get(n)){...}
```

2. int istream::get()
该方法提供不跳过空白的单字符输入功能，但使用返回值来将输入传递给程序。

到达文件尾后，将返回EOF(一个符号常量)，因此我们可以如下使用:
```cpp
while((n = cin.get())!=EOF){...}
```

cin.get()相比于cin.get(char&)来说，我们一般使用后者更佳，因为后者可以设置流状态。但是cin.get()的优点在于可以和c语言中的getchar()无缝切换。

3. 字符串输入
我们常用的有如下几个成员函数:

```cpp
istream& get(char*,int,char);
istream& get(char*,int);
istream& getline(char*,int,char);
istream& getline(char*,int);
```

第一个参数是用于放置输入字符串的内存单元的地址。第二个参数比要读取的最大字符数大1(额外的一个字符用于存储结尾的空字符，以便将输入存储为一个字符串)。第三个参数用于指定用作分界符的字符，只有两个参数的版本将使用换行符作为分界符。
上述函数都在读取最大数目的字符或者遇到分界符的时候为止。
get和getline的主要区别在get将分界符留在输入流之中，而getline将分界符丢弃。

同时，我们还有一个ignore函数:
```cpp
istream& ignore(int = 1,int = EOF);
```
该函数接受两个参数，第一个参数是指定要读取的最大字符数，默认为1，第二个参数是分界符，默认为文件尾。

对于getline(char*,int)来说，如果没有读取任何字符(但换行符被视为一个字符)，则设置failbit；如果读取了最大数目的字符，且行中还有其他字符，则设置failbit。

对于get(char*,int)来说，如果没有读取任何字符，则设置failbit。

这意味着对于如下代码:
```cpp
while(cin.get(str,100)){...}
```
如果输入一个换行，循环将直接结束，因为cin.get(str,100)设置failbit。

4. read()、peek()、gcount()、putback()

read()函数读取指定数目的字节，并把它们存储在指定的位置中。与getline()和get()不同的是，read()方法不会再输入后插入'\0'。read()方法不是给键盘输入设计的。它常常与write函数相结合进行文件输入输出。

peek()函数返回输入中的下一个字符，但不抽取输入流中的字符。

gcount()方法最后一个非格式化抽取方法读取的字符数。

putback()方法将一个字符插入到输入字符串中，被插入的字符将是下一条输入语句读取的第一个字符。(实际上就是回推流)
使用peek()方法的效果相当于使用get()读取一个字符，然后使用putback()将该字符放回到输入流中。

#### chapter 11.4 文件流

##### chapter 11.4.1 文件输出与输入

要让程序写入文件，必须这样做；
1. 创建一个ofstream对象来管理输出流
2. 将该对象与特定的文件关联起来
3. 以使用cout的方式使用该对象，唯一的区别是输出将进入文件，而不是屏幕

读取文件的要求与写入文件相似:
1. 创建一个ifstream对象来管理输入流
2. 将该对象与特定的文件关联起来
3. 以使用cin的方式使用该对象

我们可以使用构造方法来连接流和文件，也可以使用open方法来连接流和文件。我们使用close方法来显式关闭流到文件的连接。
```cpp
ofstream fout;
fout.open("1.txt");
fout<<"Dull Data"<<endl;
ifstream fin;
fin.open("2.txt");
char buf[80];
fin.get(buf,80);
fin.close();
fout.close();
```

##### chapter 11.4.2 流状态检查

ifstream对象的流状态设置与检查与cin一样，直接照看前面的。
同时，新增了一个is_open()方法。我们可以用这个方法测试一个流对象是够成功连接文件。

##### chapter 11.4.3 文件模式

常用的文件模式常量如下:
```cpp
1. ios_base::in 
//打开文件，以便读取
2. ios_base::out 
//打开文件，以便写入
3. ios_base::ate 
//打开文件，并移到文件尾
4. ios_base::app 
//追加到文件尾
5. ios_base::trunc 
//如果文件存在，则截断文件
6. ios_base::binary 
//二进制文件
```

我们可以使用构造函数或者open方法的第二个参数来指定文件模式。
```cpp
ifstream fin(filename,c++mode);

ifstream fin;
fin.open(filename,c++mode);

ofstream fout(filename,c++mode);

ofstream fout;
fout.open(filename,c++mode);
```

前文介绍的ofstream的对象方法fout.open("1.txt")，其第二个参数的默认值是ios_base::out|ios_base::trunc
前文介绍的ifstream的对象方法fin.open("2.txt")，其第二个参数的默认值是ios_base::in


文件打开模式常用组合与c语言中对应
```cpp
1. ios_base::in  "r"
2. ios_base::out "w"
3. ios_base::out|ios_base::trunc "w"
4. ios_base::out|ios_base::app   "a"
5. ios_base::in|ios_base::out    "r+"
6. ios_base::in|ios_base::out|ios_base::trunc "w+"
7. ios_base::in|ios_base::out|ios_base::app "a+"
```
注意:ios_base::ate和ios_base::app都将文件指针指向打开的文件尾。二者的区别在于:ios_base::app模式只允许将数据添加到文件尾而ios_base::ate模式将指针放到文件尾。

##### chapter 11.4.4 二进制读写

我们常常使用write方法和read方法来实现二进制读写。
```cpp
ofstream fout("1.txt",ios_base::out|ios_base::app|ios_base::binary);
fout.write((char*)&p1, sizeof(p1));
ifstream fin("1.txt",ios_base::in|ios_base::binary);
fin.read((char*)&p2,sizeof(p2));
```
比较麻烦的地方是我们必须把地址转换成char*指针。
注意:
1. 上述方法不适用于有虚函数的类。
2. read()和write()的功能是相反的，请使用read()方法来恢复write()方法写入的数据。

##### chapter 11.4.5 文件随机存取

随机存取指的是直接移动(不是依次移动)到文件的任何位置。
一般使用模式:
filename,ios_base::in|ios_base::out|ios_base::binary
我们需要创建一个fstream对象。fstream类是从iostream类派生而来的，而后者基于istream和ostream两个类，因此继承了他们的方法。他还继承了两个缓冲区，一个用于输入，一个用于输出，并能同步化两个缓冲区的处理。

```cpp
fstream randomaccess(filename,ios_base::in|ios_base::out|ios_base::binary);
```

fstream类继承了两个方法seekg()和seekp(),前者将输入指针移动到指定的文件位置，后者将输出指针移动到指定的文件位置。(也可以将seekg()用于ifstream对象，将seekp()用于ofstream对象)

```cpp
istream& seekg(streamoff,ios_base::seekdir);
istream& seekg(streampos);
```
第一个原型定位到距离第二个参数指定的文件位置特定距离的位置；第二个原型定位到离文件开头的位置。(单位为字节)
常见的ios_base::seekdir常量有三种:
```cpp
1. ios_base::beg 相对于文件开始处的偏移量
2. ios_base::cur 相对于当前位置的偏移量
3. ios_base::end 相对于文件尾的偏移量
```

调用举例:
```cpp
randomaccess.seekg(30,ios_base::beg);
//30 bytes beyond the beginning
randomaccess.seekg(-1,ios_base::cur);
//back up one byte
randomaccess.seekg();
//移动到文件的开头
```

如果要检查文件指针的位置，我们有两个方法tellp()、tellg() 
tellp()返回输出指针的位置、tellg()返回输入指针的位置。他们都返回一个streampos对象。

### Section 3 CPP 泛型编程

#### chapter 12 函数模板

函数模板是通用的函数描述，也就是说，他们使用泛型来定义函数。

##### chapter 12.1 函数模板的原型与定义

* 要建立一个模板，关键字template和typename是必须的，除非使用class代替typename。但是我们不建议这样做。
* 函数模板不创建任何函数，只是告诉编译器如何定义函数。之后编译器会进行隐式具体化，生成需要使用的函数。
   ```cpp
   template<typename T> void swap(T&, T&);
   template<typename T> void swap(T*, T*, int);
   //函数原型
   
   template <typename T>
   void swap(T& a,T& b)
   {
     T temp = a;
     a = b;
     b = temp;
   }
   
   template <typename T>
   void swap(T* a,T* b,int size)
   {
     T temp;
     for(int i=0;i<size;i++)
     {
       temp = a[i];
       b[i] = a[i];
       a[i] = temp;
     }
   }
   ```
##### chapter 12.2 显式具体化

函数模板有时候存在局限性，比如说我们的交换想基于某一个类的一个成员，此时我们传入的参数仍然是类的引用，我们用上面的swap模板很明显无法完成工作。这时候我们需要使用显式具体化技术。
```cpp
class Job
{
  public:
      int id;
      int salary;
};
template <typename T> void swap(T&, T&);
template<> void swap<Job>(Job&, Job&);

template <typename T>
void swap(T& a,T& b)
{
  T tmp = a;
  a = b;
  b =tmp;
}
template<> 
void swap<Job>(Job& j1,Job& j2)
{
  int tmp = j1.salaryl;
  j1.salary = j2.salary;
  j2.salary = tmp;
}
```

##### chapter 12.3 显式实例化

显式实例化有两种方式，一种是在声明指出，一种是在使用的时候指出。
```cpp
template <typename T> void swap(T&, T&);
template <typename T> T add(T&, T&);
int main()
{
  template void swap<int>(int&, int&);
  //声明指出，编译器将生成相应的函数。

  int m = 5;
  double n = 5.2;
  double l = add<double>(m, n);
  //在使用的时候显式实例化。
}
```

##### chapter 12.4 显式实例化与显式具体化的区别

虽然完全不是一个东西，但是两者形式很像，千万不要用混！

显式具体化要在template后面加上一个<>,显式实例化不加。

```cpp
template<> void swap<Job>(Job&, Job&);
//显式具体化
template void swap<Job>(Job&, Job&);
//显式实例化
```

显式具体化是重新定义一个和模板同名的具体参数化函数，其优先级高于普通模板，而显式实例化是跟编译器说明要生成模板的某个类型参数的函数实例。

##### chapter 12.5 函数重载解析

重载解析(overloading resolution)是一项非常重要的内容。
重载解析主要分为三个步骤:
1. 创建候选函数列表。
2. 使用候选函数列表创建可行函数列表。
3. 确定是否存在最佳的可行函数。
    通常，最佳到最差的顺序如下:
    1. 完全匹配，但常规函数优先于函数模板，较具体的模板优先于不具体的模板
    2. 提升转换(如short提升为int,float提升为double)
    3. 标准转换(如int转换为char,long转换为double)
    4. 用户自定义的转换(如类声明中定义的转换)

* 完全匹配的时候，C++允许一些“无关精要”的转换:
   * 实参:Type ,形参:Type&
   * 实参:Type& ,形参:Type
   * 实参:Type[] ,形参:Type*
   * 实参:Type(argument list) ,形参:Type(*)(argument list)
   * 实参:Type ,形参:const Type
   * 实参:Type ,形参:volatile Type
   * 实参:Type* ,形参:const Type*
   * 实参:Type* ,形参:volatile Type*

* 如果有多个原型完全匹配，往往编译器会报错。但是，有一个例外:如果存在Type->const Type和Type->Type，那么Type优先与Type匹配。
* 如果有多个原型匹配，那么编译器会选择出最优解，所谓最优解是指存在这样一个函数，这个函数比其他所有函数都要合适。如果不存在最优解，则报错。

##### chapter 12.6 关键字decltype

我们在编写函数模板的时候，常常会苦恼于不知道在模板的定义中使用哪一种类型。比如如下模板:
```cpp
template <typename T1, typename T2>
void ft(T1 x,T2 y)
{
  ...
  ?Type? xpy = x + y;
  ...
}
```
此时关键字decltype便可以派上用场。

我们可以这么修复上述的函数模板:
```cpp
template <typename T1, typename T2>
void ft(T1 x,T2 y)
{
  ...
  decltype(x+y) xpy = x + y;
  ...
}
```

decltype用法如下:
```cpp
decltype(expression) var
```
decltype按照如下步骤确定var的类型；
1. 如果expression是一个没有用括号括起的标识符，则var的类型与该标识符的类型相同。
2. 如果expression是一个函数调用，则var的类型与函数的返回类型相同。
3. 如果expression是一个左值,则var为指向其类型的引用。要进入第三部,expression必须是一个用括号括起的标识符。
4. 如果前三个条件都不满足，则var的类型与expression的类型相同。  
```cpp
double x = 5.5;
double y = 7.9;
double& rx = x;
const double* pd;
decltype(x) w;//w是double
decltype(rx) u = y;//u是double&
decltype(pd) v;//v是const double*

long indeed(int);
decltype(indeed(3)) m ;//m 是 long

double xx = 4.4;
decltype ((xx)) r2 = xx;//r2是double&
decltype(xx) w = xx;//w是double

decltype(100L) i2 ;//i2是long
```

* 可以将decltype与typedef结合使用
   ```cpp
   template <typename T1,typename T2>
   void ft(T1 x,T2 y)
   {
     typedef decltype(x+y) xytype;
     xytype xpy = x+y;
     xytype arr[10];
     xytype& rxy = arr[2];
     ...
   }
   ```

##### chapter 12.7 后置返回类型
因为我们不能直接把返回类型设置为decltype(...),因为返回类型书写在形参的前面，所以可以用auto占位符将返回类型后置，具体例子如下:
```cpp
template<typename T1,typename T2>
auto ft(T1 x,T2 y)->decltype(x+y)
{
  ...
  return x+y;
}
```

#### chapter 13 类模板

##### chapter 13.1 类模板的声明与定义

同函数模板一样，类模板要求使用template与typename关键字。特别的，如果在类外定义成员函数，template <typename T,...> 不能省略并且类名和作用域限定符之间需要加上类型参数。
```cpp
template <typename T>
class Stack
{
  private；
      enum{MAX = 10};
      Type items[MAX];
      int top;
  public:
      Stack();
      bool isempty();
      bool isfull();
      bool push(const T& item);
      bool pop(T& item);
};
template <typename T> Stack<T>::Stack():top(0) {}

template <typename T> bool Stack<T>::isempty()
{
  return top == 0;
}

template <typename T> 
bool Stack<T>::push(const T& item)
{
  if(top < MAX)
  {
    items[top++] = item;
    return true;
  } 
  else return false;
}
```

##### chapter 13.2 使用类模板

与函数模板可以隐式实例化不同，类模板必须进行显式实例化。我们需要使用具体的类型去代替泛型中的类型参数:
```cpp
std::vector<int> v1(20);
Stack<int> kernels;
```

##### chapter 13.3 表达式参数

* 有时候，我们会看到类似于template<typename T,int n>诸如此类的模板，后面的参数n被称为表达式参数。
* 表达式参数可以为整型、枚举、引用或者指针。
* 模板代码不能改变参数的值，也不能使用参数地址，因此在模板声明与定义的时候，不能出现类似于n++,&n这样的表达式。
* 用作模板参数的表达式必须为常量表达式。
* 表达式参数的缺点是即使只有表达式参数不一样，都会生成不同的具体化类声明。
   ```cpp
   template <typename T, int n>
   class ArrayTP
   {
     private:
        T ar[n];
     public:
        ArrayTP();
        explicit ArrayTP(const T&)
        virtual T& operator[](int);
        virtual const T& operator[](int)const;
   }
   
   template <typename T, int n>
   const T& ArrayDP<T,n>::operator[](int i)const
   {
     if(i>=n||i<0) 
     {
       std::cerr<<"Error"<<std::endl;
       std::exit(EXIT_FAILURE);
     }
     else return this->ar[i];
   }
   ```

##### chapter 13.4 默认类型模板参数

* 我们可以为类型参数提供默认值:
   ```cpp
   template <typename T1,typename T2 = int>
   class Example{...};
   
   class Example<double,double> class1;
   class Example<double> class2;
   //默认第二个类型参数使用int
   ```
* 虽然可以为类模板类型参数提供默认值，但是不能给函数模板类型参数提供默认值!
* 我们也可以对非类型参数提供默认值。这对函数模板和类模板都是通用的。

##### chapter 13.5 模板的具体化

###### chapter 13.5.1 隐式实例化

在声明对象的时候指出所需的类型，类模板根据类型参数生成具体的类定义，这个过程即为隐式实例化:
```cpp
ArrayDP<int, 100>* pt;
pt = new ArrayDP<int,100>(10);
```
在编译期需要对象之前，不会进行隐式实例化。
所以上述例子在第二行才会生成隐式实例化。

###### chapter 13.5.2 显式实例化

当使用关键词template并指出所需类型来声明类的时候，编译器将生成类声明的显式实例化。
声明必须位于模板定义的名称空间中。
```cpp
template class ArrayDP<int,100>;
```

###### chapter 13.5.3 显式具体化

显式具体化使用特定类型来替换一般模板中的定义。
具体化类模板的定义如下:
```cpp
template<> class class_name<specilaized_typename> {...};
```
举一个ArrayDP的例子:
```cpp
template<> class ArrayDB<int,100>{...};
```
现在如果请求<int,100>类型的ArrayDB对象，那么将使用上面的定义而不是通用的模板定义。

###### chapter 13.5.4 部分具体化

C++允许使用部分具体化，也就是部分地限制模板的通用性。

关键词template后面的<>列表指出没有被具体化的类型参数。
```cpp
template <typename T1, typename T2> 
class Pair
{
  ...
};

template<typename T1> 
class Pair<T1,int>
{
  ...
};
//部分具体化

template<> 
class Pair<int,int>
{
  ...
};
//显式具体化。
```

从上述例子可以看出，显式具体化实质上就是所有类型参数都指定的部分具体化。

##### chapter 13.6 嵌套模板

我们可以在模板中嵌套模板，比如在类模板中定义一个嵌套类模板类和一个成员函数模板
```cpp
template <typename T>
class beta
{
  private:
      template<typename V>
      class hold;
      hold<T> q;
      hold<int> n;
  pubilc:
      beta(const T&,int i):q(t),n(i) {}
      template<typename U>
      U blab(const U&,const T&)const;
      void show()const;
}

template<typename T>
  template<typename V>
    class beta<T>::hold
    {
      private:
          V val;
      public:
          hold(V v = 0):val(v){}
          void show()const{std::cout<<this->val<<std::endl;}
          V Value()const {return this->val;}
    }
template<typename T>
  template<typename U>
    U beta<T>::blab(const U& u,const T& t)const
    {
      return(n.Value() + q.Value())*u/t;
    }
```
注意两点:
1. template类型参数是嵌套的，不要写在一起
2. 在类外使用类名必须加上类型参数，比如beta<T>

##### chapter 13.7 将模板用作参数

模板本身可以作为类型参数。

并且可以与非模板的普通类型参数混用，详细可见下例:
```cpp
template <template <typename T> class thing,typename U, typename V>
class Crab
{
  private:
      thing<U> s1;
      thing<V> s2;
  public:
      bool push(U, V);
      bool pop(U&, V&);
};

template<template <typename T>class thing,typename U,typenameV>
bool Crab<T,U,V>::push(U u,V v)
{
  return s1.push(u)&&s2.push(v);
}

int main()
{
  Crab<std::stack,int,double> crab;
  crab.push(1,2.5);
}
```

##### chapter 13.8 模板类和友元

模板类声明也可以有友元，通常分为3类:
1. 非模板友元。
2. 约束模板友元:友元的类型取决于类被实例化的时候的类型。
3. 非约束模板友元:友元的所有具体化都是类的每一个具体化的友元。

###### chapter 13.8.1 非模板类友元

非模板类友元使用起来很麻烦，如果这类友元中有模板类类型的参数，我们需要去定义每一个显式具体化。
```cpp
template <typname T>
class Hasfriend
{
  private:
      T item;
      static int ct;
  public:
      Hasfriend(const T& i):item(i){ct++;}
      virtual ~Hasfriend(){ct--;}
      friend void counts();
      friend void reports(Hasfriend<T>&);
}

template <typename T>
int Hasfriend<T>::ct = 0;
//类外静态成员初始化

void counts()
{
  ...
}
void reports(Hasfriend<int>& hi)
{
  ...
}
//对Hasfriend<int>的显式具体化
void reports(Hasfriend<double>& hd)
{
  ..
}
//对Hasfirend<double>的显式具体化
```

###### chapter 13.8.2 约束模板友元

我们可以是友元函数本身成为模板，让其在类中显式实例化。
```cpp
template <typname T>
class Hasfriend
{
  private:
      T item;
      static int ct;
  public:
      Hasfriend(const T& i):item(i){ct++;}
      virtual ~Hasfriend(){ct--;}
      friend void counts<T>();//显式实例化
      friend void reports<Hasfriend<T>>(Hasfriend<T>&);
}

template <typename T>
int Hasfriend<T>::ct = 0;
//类外静态成员初始化

template <typename T>
void counts()
{
  ...
}//类外定义友元函数模板就是普通的函数模板
template <typename T>
void reports(T& t)
{
  ...
}
```

约束函数模板友元会造成类的参数类型与友元函数的参数类型相同。

###### chapter 13.8.3 非约束模板友元

前一节中的约束模板友元函数是在类外声明模板的具体化。我们还可以通过类内部声明模板，创建非约束友元函数，也即是每个函数的具体化都是每个类具体化的友元。对于非约束友元，友元模板类型参数与模板类型参数是不同的。

```cpp
template<typename T>
class ManyFriend
{
  private:
      T item;
  public:
      ManyFriend(const T& i):item(i){}
      template<typename U,typename V>
      friend void show(U&, V&)；
}
template<typename U,typename V>
void show(U& u,V& v)
{
  using std::cout;
  using std::endl;
  cout<<u.item<<endl<<v.item<<endl;
}
```

#### chapter 14 智能指针模板类

##### chapter 14.1 使用智能指针

* 三个智能指针模板auto_ptr、unique_ptr、shared_ptr都定义了类似于指针的对象，可以将new获得(直接或间接)的地址赋给这种对象。
* 当智能指针过期的时候，其析构函数将使用delete来释放内存。
* 智能指针构造函数的参数是指针类型的构造器。
   ```cpp
   std::auto_ptr<std::string> ptr (new std::string("bit"));
   ```
* 所有智能指针类都有一个explicit构造函数，该构造函数将指针作为参数。
   ```cpp
   shared_ptr<double> pd;
   double* p_reg = new double;
   pd = shared_ptr<double> (p_reg);
   shared_ptrZ<double> pd2(p_reg);
   ```

##### chapter 14.2 智能指针注意事项

* 智能指针的构造参数不能是非堆内存，因为智能指针的析构使用delete运算符，而delete运算符不能运用于非堆内存。
* 我们注意到，使用智能指针可能存在一个问题:
   ```cpp
   auto_ptr<string> pd(new string("bit"));
   auto_ptr<string> pd2;
   pd2 = pd;
   ```
   这里会出现一个严重的问题，就是pd2与pd指向的是同一个字符串对象，在代码块结束调用析构函数的时候这个字符串对象将被析构两次。为了避免这种问题，我们有多种解决方法:
   1. 在前面的章节我们学过deep copy,我们可以重载赋值运算符，使得pd2 = pd这一个赋值过程实现深拷贝。
   2. 我们可以建立所有权概念。对于一个特定的对象，只有一个智能指针可以拥有它，这样只有拥有对象的智能指针才能够调用析构函数。然后当进行赋值操作的时候，我们会转让所有权。这种思想正是auto_ptr和unique_ptr的思想。
   3. 可以使用引用技术技术，只有党引用技术归为零的时候才调用析构函数。这种思想正是shared_ptr的思想。

##### chapter 14.3 智能指针之间的区别

1. auto_ptr与unique_ptr
   
    ```cpp
    using namespace std;
    auto_ptr<string> ptr1(new string("bit"));
    auto_ptr<string> ptr2;
    ptr2 = ptr1;
    unique_ptr<string> ptr3(new string("biter"));
    unique_ptr<string> ptr4;
    ptr4 = ptr3;//invalid
    ```
    * 在前两句语句中，auto_ptr允许将所有权从ptr1转移到ptr2，但是在此之后ptr1将不指向任何对象，使用ptr1将引发空指针异常。而unique_ptr则更为安全，因为unique_ptr禁止了ptr3到ptr4的所有权转换。
    * 程序试图将一个unique_ptr赋给另一个的时候，如果源unique_ptr是一个临时右值，那么编译器允许这么做，如果源unique_ptr将存在一段时间，那么编译器将禁止这么做。
      <font color = 'blue'>(在传参的时候需要特别注意，避免CE)</font>
       ```cpp
       using namespace std;
       unique_ptr<string> ptr1 = unique_ptr(new string("biter"));
       unique_ptr<string> ptr2;
       ptr2 = ptr1;//invalid
       unique_ptr<string> ptr3;
       ptr3 = unique_ptr<string> (new string("bit"));//valid
       ```
    * 相比于auto_ptr，unique_ptr还可以用于数组。模板auto_ptr只有使用new/delete的版本，而模板unique_ptr有使用new/delete和new[ ]/delete[ ]的两种版本。
       ```cpp
       std::unique_ptr<double[]>pda(new double(5)) ;
       //will use delete[];
       ```

2. shared_ptr
    * shared_ptr使用引用计数技术，如果我们要使用指向同一个对象的多个指针，应当选用shared_ptr。
    * 当unique_ptr为右值的时候，我们可以将其赋给shared_ptr，这与unique_ptr所有权转换的条件相同。

#### chapter 15 迭代器、容器与函数对象

##### chapter 15.1 迭代器

###### chapter 15.1.1 迭代器的定义与类型

泛型编程旨在使用同一个函数来处理各种容器类型。使得函数不但独立于容器中存储的数据类型，而且还独立于容器本身的数据结构。模板提供了存储在容器中的数据类型的通用表示，因此还需要遍历容器中的值的通用表示，迭代器正是这样的表示。

* 迭代器类型:
   1. 输入迭代器
   2. 输出迭代器
   3. 正向迭代器
   4. 双向迭代器
   5. 随机访问迭代器
* 对于以上五种迭代器，都可以执行解除引用的操作，也可以进行比较(==与!=),并且要求(iter1 == iter2)与(*iter1 == *iter2)同时等价。
* 迭代器性能:
    1. 输入迭代器能够读、执行自增
    2. 输出迭代器能够写、执行自增
    3. 正向迭代器具有输入输出迭代器的全部功能，并且能够保证固定与可重复排序(输入输出迭代器不能保证前后两次读写遍历元素的顺序相同，正向迭代器可以)
    4. 双向迭代器具有正向迭代器所有功能，并且能执行自减。
    5. 随机访问迭代器具有双向迭代器的所有功能，并且能执行[]随机访问以及指针+、-、+=、-=运算。
* 迭代器是广义指针，意味着基于迭代器的算法也可以传入普通的指针。普通指针相当于随机访问迭代器。

###### chapter 15.1.2 迭代器的应用

1. copy函数
   ```cpp
   int casts[10]={6,7,2,...,5};
   vector<int> dice(10);
   copy(casts,casts+10,dice.begin());
   ```
   copy函数的前两个参数表示要复制的范围，必须至少是输入迭代器(读迭代器)，第三个参数是表示要将第一个元素复制到什么位置，至少是一个输出迭代器(写迭代器)。目标容器必须足够大，因此不能把上述例子中的10个元素放入空vector中。
2. ostream_iterator类模板
    ```cpp
    ostream_iterator<int,char> out_iter(cout,"");
    copy(dice.begin(),dice.end(),out_iter);
    ```
    第一个类型参数指出被发送给输出流的数据类型、第二个类型参数指出输出流使用的字符类型。构造函数的第一个参数指出要使用的输出流，构造函数的第二个参数指出发送给输出流的每个数据项后的分隔符。
3. istream_iterator类模板
    ```cpp
    istream_iterator<int,char> in_iter1(cin);
    istream_iterator<int,char> in_iter2();
    copy(in_iter1,in_iter2,dice.begin());
    ```
    模板参数的意义与ostream_iterator相同，使用一个参数的构造函数指出要使用的输入流，省略参数的构造函数意味着表示输入失败。上述copy将在输入失败的时候停止读取。
4. insert_interator类模板
    ```cpp
    vector<int> words(4); 
    insert_iterator<vector<int> > it(words,words.begin());
    copy(casts,casts+10,it);
    ```
    insert_iterator可以调整容器的大小，避免了因为容器容量不够出现内存异常。
    insert_iterator的类型参数接受一个容器类型，构造函数的第一个参数是容器对象，第二个参数是插入位置。
5. transform函数
    ```cpp
    const int LIM = 5;
    double arr1[LIM] = {36,39,42,45,49};
    double arr2[LIM] = {32,33,43,44,47}
    vector<double> gr8(arr1,arr1 + LIM);
    vector<double> m8(arr2,arr2 + LIM);
    ostream_iteraator<double,char> out_iter(cout," ");
    transform(gr8.begin(),gr8.end(),out,sqrt);
    transform(gr8.begin(),gr8.end(),m8.begin(),out,plus<double>);
    ```
    transform有两个版本:
    1. 第一个版本有四个参数,第一个参数和第二个参数是输入迭代器区间，第三个参数是输出迭代器，第四个参数是一元函数。
    2. 第二个版本有五个参数，第一个参数和第二个参数是输入迭代器区间，第三个参数是另外一个容器对象的迭代器以表明起始位置，第四个参数是输出迭代器，第五个参式是二元函数。

还有很多迭代器的引用，可以去调api。

##### chapter 15.2 容器

容器分为序列容器和关联容器。

常见的序列容器有七种:vector、list、deque、queue、stack、priority_queue、forward_list。

常见的关联容器有八种:map、multimap、set、multiset、unordered_map、unordered_multimap、unordered_set、unordered_multiset

这些容器在打算法竞赛的时候用的已经够多的了，建议直接查api，这里不码字了。

##### chapter 15.3 函数对象

###### chapter 15.3.1 函数对象的类型

C++的函数对象包括三种:
1. 函数名(常对象)
2. 函数指针 (比如int(*pt)(int,int))
3. 重载了operator()运算符的类对象
    ```cpp
    class Linear
    {
      private:
          double slope;
          double y0;
      public:
          Linear(double s1_ = 1,doubel y_=0):slope(s1_),y0(y_) {}
          double operator() (const double x)
    }
    double Linear::operator() (const double x)
    {
      return this->y0 + this->slope * x;
    }
    
    int main()
    {
      Linear line1(2.0,1.0);
      double res = line1(15.2);
      //重载了()运算符之后，我们可以像调用函数一样调用类对象。
    }
    ```

###### chapter 15.3.2 函数符

函数符主要有下述五种；
1. 生成器(generator) 不需要参数就可以调用的函数符
2. 一元函数(unary function) 用一个参数就可以调用的函数符
3. 二元函数(binary function) 用两个参数可以调用的函数符
4. 谓词 返回bool值的一元函数
5. 二元谓词 返回bool值的二元函数

举个例子，比如说我们常用的sort函数，他的第三个参数可以接受一个二元谓词作为比较器:
```cpp
bool WorseThan(const Student&,const Student&);
vector<Student> s1(10);
sort(s1.begin(),s1.end(),WorseThan);
```
在举一个一元谓词的例子:
```cpp
template <typename T>
class tooBig
{
  private:
      T cutoff;
  public:
      tooBig(const T&)
      bool operator() (const T&);
};

template <typename T>
tooBig<T>::tooBig(const T& t):cutoff(t) {}

template <template T>
bool tooBig<T>::operator() (const T& t)
{
  return t > cutoff;
}

tooBig<int> v(100);
list<int> scores;
...
scores.remove_if(v);
//v是一元谓词
```

###### chapter 15.3.3 预定义的函数符

C++头文件functional定义了多个模板类函数对象，大致有如下几种:
1. plus<> 相当于+
2. minus<> 相当于-
3. multiplies<> 相当于*
4. divides<> 相当于/
5. modulus<> 相当于%
6. negete<> 相当于-
7. equal_to<> 相当于==
8. not_equal_to<> 相当于!=
9. greater<> 相当于>
10. less<> 相当于<
11. greater_equal<> 相当于>=
12. less_equal<> 相当于<=
13. logical_and<> 相当于&&
14. logical_or<> 相当于||
15. logical_not<> 相当于!

用法举例:
```cpp
#include "functional"
...
plus<double> add;//隐式实例化并声明对象
double y = add(2.5,3.5);

std::priority_queue<int,vector<int>,greater<int>> pq;//作为模板参数
```

###### chapter 15.3.4 自适应函数符与函数适配器

C++ 使用类binder1st和binder2nd来把自适应二元函数转换为一元函数。

```cpp
binder1st(plus<double>,2.5) f1;
vector<double> v(10);
transform(v.begin(),v.end(),ostream_iterator<double,char> (cout," "),f1);
```

上述例子中二元函数符plus被转换为一元函数符f1。
binder1st把常数赋给第一个参数,binder2nd把常数赋给第二个参数。

#### chapter 16 string类、STL与算法

没有什么新的语法知识点，建议直接看api文档调参。

### Section 4 C++ 新标准





by lqgdwind_bit
