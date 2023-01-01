# C++基础

## 1.面向对象编程语言概述

* 对象、类

  对象的基本概念：客观存在的实体称为对象。 一个对象具有一组属性和行为。对象是现实世界中实际存在的事物。将现实中的对象经过抽象，映射为软件中的对象。

  类的基本概念：类是具有相同属性和行为的一组对象的集合，它为属于该类的全部对象提供了抽象的描述，包括属性和行为两个主要部分。

  类和对象的关系：类类似于蓝图，对象是类的实例，对象是实际存在的该类事物的个体，是由类定义所产生出来的实例

  **类（抽象定义）<=> 对象（实例）**  

  举例:

  学生 <=> 学生王强

  课程<=> 面向对象程序设计与应用

  类型 <=> 变量, 如 C 语言中的 int 和 int x                        

* 类的声明形式

  类是一种用户自定义类型，声明形式：

  ```c++
   class 类名称{
        public:
        //公有成员（外部接口）
        private:
        //私有成员
        protected:
        //保护型成员}；
  ```

  公有类型成员：在关键字public后面声明，它们是类与外部的接口，任何外部函数都可以访问公有类型数据和函数。

  私有类型成员：在关键字private后面声明,只允许本类中的函数访问,而类外部的任何函数都不能访问。如果紧跟在类名称的后面声明私有成员,则关键字private可以省略。

  保护类型成员：与private类似，其差别表现在继承与派生时对派生类的影响不同。

* 对象的声明形式 ：**类名 对象名；**

  成员的访问方式

  ①类中成员互访：直接使用成员名

  ②类外访问：使用“对象名.成员名”方式访问 public 属性的成员

* 抽象

  抽象是对具体对象（问题）进行概括，抽出这一类对象的公共性质并加以描述的过程。

  数据抽象：描述某类对象的属性或状态（对象相互区别的物理量）。

  代码抽象：描述某类对象的共有的行为特征或具有的功能。

  抽象的实现：通过类的声明。

  ```c++
  run() ,
  dispTime() ,
  setHour(),setMinute(),setSecond()
  class Clock
  {
     int Hour,Minute,Second; 
     void setHour(int h);
     void setMinute(int m);
     void setSecond(int s);
     void dispTime();
     void run();
   };
  ```

* 封装

  将抽象出的数据成员、代码成员相结合，将它们视为一个整体。

  **目的：**增强安全性和简化编程，使用者不必了解具体的实现细节，而只需要通过外部接口，以特定的访问权限，来使用类的成员。

  **实现封装**：类声明中的{}

  ```c++
  class Clock
   {
   private:
      int Hour,Minute,Second;
      void run(); 
   public:
      void setHour(int h);
      void setMinute(int m);
      void setSecond(int s);
      void dispTime();
   };
  ```

  一个class定义了一种抽象的数据类型,用户只能访问public成员,不能直接访问private成员

* 抽象与封装形成了程序接口与实现的分离![img](https://p.ananas.chaoxing.com/star3/origin/3a7fc8132a5a73320b31d9f7b7d107fc.png)

* 继承的基本概念：对象之间的相互关系,使得某类对象可以继承另外一类对象（祖先）的特征和功能。

  类间具有继承关系的特性:

  ①类间具有共享特征：遗传

  ②类间具有细微差别或新增部分：变异

  ③类间具有层次结构（同人类通过继承构成了家族关系一样）

    保持已有类的特性而构造新类的过程称为**继承**。

    在已有类的基础上新增自己的特性而产生新类的过程称为**派生**。

    被继承的已有类称为**基类**（或**父类**）。

    派生出的新类称为**派生类**（或**子类**）。

  ![img](https://p.ananas.chaoxing.com/star3/origin/2aa88b54a17c37942034a578ffee3dc9.png)

* 继承分类：从继承源上划分：单继承（一个派生类只有一个基类）、多继承（一个派生类有多个基类）

![img](https://p.ananas.chaoxing.com/star3/origin/83f36749b1372a055d494b90b2ed9c44.png)

* 继承目的：**实现代码重用**code reuse

* 派生的目的：当新的问题出现，原有程序无法解决（或不能完全解决）时，需要对原有程序进行改造。

* 多态

  * 多态：对象根据所接受的消息而做出动作，同样的消息为不同的对象接受时可导致完全不同的行动，该现象称为多态性。

  * 作用 ：方便软件功能的扩展  ***举例： F(动物 \*P）{ p->run(); }***

    注：F即为多态函数，当传递狗对象给p时，执行狗.run() ，传递猫对象给p时，执行猫.run()；当猎人的枪声响起时，不同动物开始run()，它们不同的奔跑方式就是多态。

  * 多态性的实现

    运行时多态性：虚函数

    编译时多态性：重载

     ***举例：***

      sqrt_i (int i)，sqrt_f (float f)

      sqrt (int i)，sqrt (float f)

      作用：减轻程序员负担、降低程序员出错机率

  * 多态的一个案例

     图中同名函数**area( )**作用在**Circle、Triangle**等不同类上时，将执行不同的“计算面积”的方法，这就是多态。

    ![img](https://p.ananas.chaoxing.com/star3/origin/b2981341ff866661db5629eb5c33d48b.png)

  

## 2、标准输入流输出流

I/O（input/ouput，输入/输出）数据是一些从源设备到目标设备的字节序列，称为字节流。除了图像、声音数据外，字节流通常代表的都是字符，因此在多数情况下的流（stream）是从源设备到目标设备的字符序列，![img](https://p.ananas.chaoxing.com/star3/origin/28807cbeef977b7faabc0aea5a153bb7.png)

**输入流**（input stream）是指从输入设备流向内存的字节序列。 

**输出流**（output stream）是指从内存流向输出设备的字节序列。

* C++中的输入流iostream

  输入流istream   输出流ostream

* C++中输入输出数据的流变量

  * cin：输入流对象，C++已将其与键盘关联

    * 注意事项:

      * 在一条cin语句中同时为多个变量输入数据。在输入数据的个数应当与cin语句中变量个数相同，各输入数据之间用一个或多个空白（包括空格、Tab）作为间隔符，全部数据输入完成后，按Enter键结束。 

      * 在>>后面只能出现变量名，下面的语句是错误的。

        ```c++
        cin>>"x=">>x;//错误，>>后面含有字符串"x="
        cin>>12>>x;//错误，>>后面含有常数12
        cin>>'x'>>x;
        ```

      * cin具有自动识别数据类型的能力，析取运算>>将根据它后面的变量的类型从输入流中为它们提取对应的数据。 比如： cin>>x;

        假设输入数据2，析取运算符>>将根据其后的x的类型决定输入的2到底是数字还是字符。若x是char类型，则2就是字符；若x是int，float之类的类型，则2就是一个数字。再如，若输入34，且x是char类型，则只有字符3被存储到x中，4将继续保存在流中；若x是int或float，则34就会存储x中。

    * 数值型数据的输入

        ==cin能提取无分隔符的数据。==在读取数值型数据时，析取运算符>>首先略掉数据前面的所有空白符号，如果遇到正、负号或数字，就开始读入，包括浮点型数据的小数点，并在遇到空白符或其他非数字字符时停止。例如：

      ```c+
      int x1;
      double x2;
      char x3;
      cin>>x1>>x2>>x3;
      ```

      假如输入“35.4A”并按Enter键，x1是35；x2 是0.4；x3是'A'

  * cout：输出流对象，C++已将其与显示器关联。cout可以通过输出运算符<<向输出缓冲区中插入数据

## 3、const和constexpr

* 常量定义

  ```c++
  //C： 
  #define 常量名称  常量值 
  //C++： 
  const 类型 常量名称 = 常量值;
  constexpr 类型 常量名称=常量表达式; // C++11
  ```

* 常量说明

  * 常量一经定义就不能修改，例如：

    ```c++
    const int i = 5;         // 定义常量I
    constexpr int k = 9; 　   // 定义常量k
    i = 10;               // 错误，修改常量
    k++;                // 错误，修改常量
    ```

  *  const常量必须在定义时初始化，

    ```c++
    const int n;             //错误，常量n未被初始化
    constexpr int k;          //错误，常量k未被初始化
    ```

  * 表达式可以出现在常量定义语句中,const中可以有变量名，但constexpr的表达式中不能有变量     

    ```c++
    int j,k=9;          
    const int i1=10+k+6;       //正确
    constexpr int i1=10+k+6;   //错误，k是变量
    ```

* const和constexpr的区别？

  ==constexpr 在编译时进行初始化，const 在运行时初始化==。此要求用于初始化constexpr常量的表达式中的每部分值都是程序运之前就可以确定的字面值常量。而const无此限定，它只限定了定义的常量在程序运行期间不可被修改，但其初始值即使在运行时才取得也是可以的。

  ```c++
  const int n=size();        //正确，但n值的取得是在执行函数时
  constexpr int m=size();     //错误，程序编译时不知道size()的值
  const int i = 10;      
  int j = 21;
  const int i1 = i + 10;   	//正确
  const int j1 = j + 10;   	//正确
  constexpr int i2 = i + 10;    //正确，编译时可确定i值为10
  constexpr int j2 = j + 10;    //错误，j是变量
  ```

  ## 4、命名空间

* C++引入命名空间的原因 std::cout

  C++编程环境中，系统定义了大量的变量、函数和类的名称。在编程中可能定义出系统已存在的变量、函数或类名称，产生冲突。多人合作进行软件开发时，可能定义出相同的名称，产生冲突。这些问题导致命名空间的运用：即程序员可以将自己定义的名字局限在一个自定义的命名空间中，就不会与其它人定义的名字冲突。

* 命名空间的定义

  ```c++
  namespace namespace_name
   { members;};
  ```

  其中，**namespace**是定义命名空间的**关键字**；namespace_name是程序员指定的命名空间的名字；**members** 是命名空间中包括的**成员**，可以是变量定义、函数声明、函数定义、结构声明，以及类的声明等。

  ```c++
  namespace ABC{
  	int count;
  	typedef float house_price;
  	struct student{
  		char *name;
  		int age;
  	};
  	double add(int a,int b)	{ return (double)a+b;}
  	inline int min(int a,int b);
  };
  int ABC::min(int a,int b){
  	return a<b?a:b;
  }
  ```

* 命名空间的应用

   命名空间成员的作用域局限于命名空间内部，可以通过作用域限定符（::）访问它，语法如下：**namespace_name::identifier**

  ```c++
  int main(){
  	ABC::count=1;	  //访问ABC空间中的count
  	int count=9;	  //main函数中的count与ABC中的count无关
  	ABC::student s;  //用ABC空间中的student结构定义s
  	s.age=9;
  	int x=ABC::min(4,5); //调用ABC中的min函数计算两数最小值
       return 0;
  } 
  ```

* 用using namespace访问名字空间成员

  ① 引用名字空间的单个成员。用法如下：**using namespace_name::identifier**

  ② 引入名字空间的全部成员。用法如下：**using namespace_name**

* std命名空间

   ① C++标准化过程形成了两个版本：一个是以Bjarne Stroustrup (本贾尼·斯特劳斯特卢普)最初设计的C++为基础的版本，称为传统C++；另一个是晚期（约1989年）以ANSI/ISO标准化委员会创建的C++，称为标准C++。

   ② 两种版本的C++有大量相同的库和函数。为了将两者区分：传统C++采用与C语言同样风格的头文件；标准C++的新式头文件没有扩展名，即不需要.h之类的扩展名。例如，传统C++的头文件有iostream.h、fstream.h、string.h；标准C++对应的头文件有iostream、fstream、string。

  ③ 传统C++中的内容被直接放到了全局命名空间中。标准C++将新格式头文件中的内容全部放到了std命名空间中。

  ④ 许多C++编译器都提供了对两种版本C++的支持，并允许在同一程序中同时引用两种版本的C++函数。如果程序要引用标准C++新格式头文件中的函数，就需要在程序中使用下面的语句将std命名空间中的名称引入到全局命名空间中。

  **using namespace std;**

  ⑤ using namespace std会将std命名空间中的标志符都引入到程序中，如果只需用到std命名空间中的个别标志符，可以在要使用的标志符前面加上前缀std::，不必用“using namespace std;”将std中的全部名称引入到程序中来。

  ```c++
  using std::cout;
  using std::cin;
  using std::endl;
  ```

  ## 5、类型转换

* 隐式类型转换的概念

   C++定义了一套标准数据类型转换的规则，在必要时，C++会用这套转换规则在程序员不参与进行数据类型的自动转换。四种常见的隐式类型转换如下：

  ①在混合类型的算术表达式中，最宽的数据类型成为目标类型，如图示。

  ​	![img](https://p.ananas.chaoxing.com/star3/origin/21c270910cb79a6e97e77ab0c3b63924.png)

  ②用一种类型的表达式赋予另一种类型的对象，目标类型是被赋值的类型对象。

  ③把一个表达式传给一个函数调用，表达式的类型与形参的类型不同，目标类型是形式参数的类型；

  ④从一个函数返回一个类型，表达式的类型与返回类型不符，目标类型是返回类型。

* 显式类型转换

   显式类型转换也称强制类型转换（cast)，有时需要强制类型转换，但它也是错误的根源，因为它关闭了编译器的类型检查机制。

  ```c++
  类型名称（表达式）；
  int i = 100;
  float f = float (i);
  void * p;
  char * cp = char * (p);
  ```

  2、C++中强制类型转换的几种形式

  cast-name<type>(expression);

  其中的cast-name可以为：

  static_cast  :静态转换

  dynamic_cast：动态转换

  const_cast  ：常量转换

  reinterpret_cast：用于不相关的类型转换，如将int转换成指针等。

  ```c++
  double b=-67.89;
  int c=b;
  int c=static_cast<int>(b);
  ```

  ```c++
  //利用const_cast转换去掉指针和引用的const限制。
  #include<iostream>
  using std::cout;
  using std::endl;
  void sqrt(const int *x) {
      int *p=const_cast <int *>(x); //const_cast去掉了x的const限制
      *p=(*p) * (*p);	//p和x指向同一内存地址，即*p实际修改了*x
    }
  void sqr(const &x) {
      const_cast<int &>(x)=x*x;  //const_cast去掉了x的const限制后修改了
    }
    void main(){
       int a=5;
       sqrt(&a);  //通过指针将a改为25
       cout<<a<<endl;  //输出25
       sqr(a);    //通过引用将a改为625
       cout<<a<<endl;  //输出625
    }
  ```





