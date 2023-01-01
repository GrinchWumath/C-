# 模板与STL

## 1.模板的概念

模板是对具有相同特性的函数或类的再抽象。模板是一种参数多态性的工具，可以为逻辑功能相同而类型不同的程序提供一种代码共享的机制，一个模板并非一个实实在在的函数或类，仅仅是一个函数或类的描述，是参数化的函数和类。

* 抽象：从多个具有相同特性的同类事物推导出对该类事物共同特征的统一描述的过程。

​		变量→类型   对象→类  函数→函数模板  类→类模板

* 实例化是抽象的逆过程，由抽象类型定义具体变量的过程。用实际数据类型代入模板每一种不同数据类型的实例化，都将生成一份不同的代码

* 模板被使用前必须实例化，函数模板实例化后得到的函数叫模板函数，类模板实例化得到的类叫模板类

![img](https://p.ananas.chaoxing.com/star3/origin/a4d4f4f07ff26f647c818b7147f96c65.png)

## 2.函数模板

函数模板提供了一种通用的函数行为，==该函数行为可以用多种不同的数据类型进行调用==，编译器会据调用类型自动将它实例化为具体数据类型的函数代码，也就是说函数模板代表了一个函数家族。

  与普通函数相比，函数模板中某些函数元素的数据类型是未确定的，这些元素的类型将在使用时被参数化；与重载函数相比，函数模板不需要程序员重复编写函数代码，它可以自动生成许多功能相同但参数和返回值类型不同的函数。

```c++
template <class T>
T max (T a, T b) { return a>b ? a : b;}
int i = max (3, 4);
char ch = max (‘a’, ‘A’);
float f = max (5.0, 1.0);
 ……
```

==可以进行完备的语法检查、适于描述复杂逻辑。相同逻辑只描述一次，针对不同类型的版本由编译器自动生成相应代码==

* 函数模板的定义

  ```c++
  template <class T1, class T2,…>
  返回类型 函数名(参数表){
  ……  //函数模板定义体
  }
  ```

  template是定义模板的关键字；写在一对<>中的T1，T2，…是模板参数，其中的class表示其后的参数可以是任意类型。模板参数常称为类型参数或类属参数，在模板实例化（即调用模板函数时）时需要传递的实参是一种数据类型，如int或double之类。在使用函数模板时，应注意以下几点:

  * 在定义模板时，==不允许template语句与函数模板定义之间有任何其他语句。==

    ```c++
    template <class T>
    int x; //错误，不允许在此位置有任何语句
    T min(T a,T b){…}
    ```

  * 函数模板可以有多个类型参数，但每个类型参数都必须用关键字class或typename限定。此外，模板参数中还可以出现确定类型参数，称为非类型参数。例：

    ```c++
    template <class T1,class T2,class T3,int T4>
    T1 fx(T1 a, T 2 b, T3 c){…}
    ```

    ==在传递实参时，非类型参数T4只能使用常量，如6。==

  * ==不要把这里的class与类的声明关键字class混淆在一起，虽然它们由相同的字母组成，但含义是不同的。==这里的class表示T是一个类型参数，可以是任何数据类型，如int、float、char等，或者用户定义的struct、enum或class等自定义数据类型。

  * 为了区别类与模板参数中的类型关键字class，==标准C++提出了用typename作为模板参数的类型关键字==，同时也支持使用class。比如，把min定义的template <class T>写成下面的形式是完全等价的：

    ```c++
    template <typename T> 
    T min(T a,T b){…} 
    ```

* 函数模板的实例化

  实例化的方式:

  * 隐式实例化：**编译器能够判断模板参数类型时，自动实例化函数模板为**模板函数**。

    ```
    template <typename T> T max (T, T);
     …
    int i = max (1, 2); 
    float f = max (1.0, 2.0);
    char ch = max (‘a’, ‘A’);
     …
    ```

  * 显式实例化

    **时机：**编译器不能判断模板参数类型或常量值、需要使用特定数据类型实例化

    **语法形式：**模板名称<数据类型,…,常量值,…> (参数)，其中数据类型提供给类型参数，常量值提供给非类型参数。

  ==当多次发生类型相同的参数调用时，只在第1次进行实例化。==

  ```c++
  int x=min(2,3);     
  int y=min(3,9);
  int z=min(8,5);
  ```

  编译器在遇到第一句中的min（2，3）调用时，会实例化成int min(int,int)模板函数，后两句会直接调用该模板函数。

* 模板参数

  * 模板参数的转换问题

    C++在实例化函数模板的过程中，只是简单地将模板参数替换成调用实参的类型，并以此生成模板函数，==不会进行参数类型的任何转换==。这种方式与普通函数的参数处理有着极大的区别，在普通函数的调用过程中，C++会对类型不匹配的参数进行隐式的类型转换。

    ```c++
    #include <iostream>
    using namespace std;
    template <class T>
    T Max(T a,T b) {
             return (a>b)?a:b;}
    void main(){
          double a=2,b=3.4;
           float  c=5.1,d=3.2;
           cout<<"2, 3.2的最大值是："<<Max(2,3.2)<<endl;
           cout<<"a c 的最大值是："<<Max(a,c)<<endl;
           cout<<"'a', 3的最大值是："<<Max('a',3)<<endl;
     }
    ```

    编译例子中的程序，将会产生3个编译错误：error C2782: “T Max(T,T)”: 模板参数“T”不明确 。产生这个错误的原因是模板实例化过程中不会进行任何形式的参数类型转换，但在普通函数的调用过程中，C++会对类型不匹配的参数进行隐式的类型转换 。从而导到模板函数的参数类型不匹配，因此产生上述编译错误。

    对于上述问题,可以采取以下方法

    1）在模板调用时进行参数类型的强制转换

    ```c++
    cout<<max(double(2),3.2)<<endl;
    ```

    2）显式指定函数模板实例化的类型参数

    ```c++
    cout<<max<double>(2,3.2)<<endl;
    cout<<max<int>('a',3)<<endl;
    ```

    3）指定多个模板参数

  * 模板函数的形参表

    ```c++
    #include <iostream>
    using namespace std;
    template <class T>
    void sort(T & a,int n) {
    	for (int i=0;i<n;i++){
    		int p=i;
    		for(int j=i;j<n;j++)
    			if(a[p]>a[j])	p=j;
    		int t=a[i];	a[i]=a[p];	a[p]=t;
    	}
    }
    template <class T> void display(T& a,int n)
    {
    	for(int i=0;i<n;i++)
    		cout<<a[i]<<"\t";
    	cout<<endl;
    }
    void main(){
    	int a[]={1,41,2,5,8,21,23};
    	char b[]={'a','x','y','e','q','g','o','u'};
    	sort(a,7);
    	sort(b,8);
    	display(a,7);
    	display(b,8);
    }
    ```

    

## 3.类模板

* 类模板的声明

  ```
  template<class T1,class T2,…>
  class 类名{
   ……// 类成员的声明与定义
  }；
  ```

  其中T1、T2是类型参数。类模板可以接受数据类型作为参数，设计出与具体类型无关的通用类。**类模板中可以有多个模板参数，包括类型参数和非类型参数。**非类型参数是指某种具体的数据类型，在调用模板时只能为其提供用相应类型的常数值。非类型参数是受限制的，通常可以是整型、枚举型、对象或函数的引用，以及对象、函数或类成员的指针，==但不允许用浮点型（或双精度型）、类对象或void作为非类型参数==。在下面的模板参数表中，T1、T2是类型参数，T3是非类型参数。

  ```
  template<class T1,class T2,int T3>
  ```

   在实例化时，必须为T1、T2提供一种数据类型，为T3指定一个整常数（如10），该模板才能被正确地实例化。

* 类模板的成员函数的定义

  **方法1：在类模板外定义，语法**

  ```c++
  template <模板参数列表> 
  返回值类型 类模板名<模板参数名称列表>::成员函数名 (参数列表)
   { ……};
  ```

  其中：

  <模板参数列表>引入的“类型标识符”作为数据类型使用

  <模板参数名称列表> 引入的“普通数据类型常量”作为常量使用

  **方法2：直接在模板内定义成员函数，与常规成员函数的定义方法相同。**

  ```c++
  //设计一个链表类模板List，实现链表的插入、删除、查找和链表数据输出等功能，在模板中用类型参数T表示链表节点中存放的数据。
  #include"List.h"
  #include<iostream>
  using namespace std;
  template<class T> 
  void sort(List<T> *L)
  {
  	ListNode<T> *p,*q;
  	p=L->getHead();
  	p=p->next;
  	while(p!=NULL)
  	{		
  		ListNode<T>* t=p;
  		q=p;
  		while(q->next!=NULL)
  		{	q=q->next;
  			if(t->data < q->data )	t=q;
  		}
  	    int x=t->data;
  		t->data =p->data;
  		p->data=x;
  		p=p->next;
  	}
  }
  
  void main()
  {
  	List<int> intList;
  	intList.insertNode (3,1);
  	intList.insertNode (4,2);
  	intList.insertNode (1,3);
  	intList.insertNode (32,4);
  	intList.print();
  	sort(&intList);
  	intList.print();
  
  	List<char> charList;
  	charList.insertNode ('a',1);
  	charList.insertNode ('w',2);
  	charList.insertNode ('100',3);
  	charList.insertNode ('o',4);
  	charList.insertNode ('b',5);
  	charList.print();
  	sort(&charList);
  	charList.print();
  }
  ```

## 4.STL

STL就是标准模板库（Standard Template Library），是C++较晚加入的基于模板技术的一个库，它提供了模板化的通用数据结构、类和算法。用他们来解决编程中的各种问题，可以减少程序测试时间，写出高质量的代码，提高编程效率。==容器用于存储数据，是由同类型的元素所构成的长度可变的序列，通过类模板实现；算法是用于对容器中的元素进行一些常用的操作，通过函数模板实现，迭代器用于访问容器中的元素，他们由具有抽象指针功能的类模板实现。==

 STL的核心内容包括**容器、迭代器、算法**三部分 ，三者常常协同工作，为各种编程问题提供有效的解决方案。

![img](https://p.ananas.chaoxing.com/star3/origin/b08ae029a398528f338a07feaeb391a4.png)

* 容器

  容器（container）是用来存储其他对象的对象，它是用模板技术实现的。 STL的容器常被分为顺序容器、关联容器和容器适配器三类。 

  * 顺序容器是将相同类型对象的有序集按顺序组织在一起的容器，用来表示线性数据结构。

  * 关联容器是非线性容器，用来根据键进行快速存储、检索数据的容器。

  * 容器适配器实际是受限制访问的顺序容器类型。

  ①C++提供的顺序类型容器有向量（vector）、链表（list）、双端队列（deque）。

  ②关联容器主要包括集合（set）、多重集合（multiset）

  ③容器适配器主要指堆栈（stack）和队列（queue） 

  **STL中的容器及头文件名：**

  ![img](https://p.ananas.chaoxing.com/star3/origin/26b84aacfc504dd4c49d8d026f08ff02.png)

  **所有容器都具有的成员函数：**

  ![img](https://p.ananas.chaoxing.com/star3/origin/df5c126861f42954739750b0da1b64d8.png)

  **顺序和关联容器共同支持的成员函数：**

  ![img](https://p.ananas.chaoxing.com/star3/origin/954788adf1d6a62d8cc6acc726b61626.png)

  * 顺序容器

    * Vector

      vector是向量容器，它具有存储管理的功能，在插入或删除数据时，vector能够自动扩展和压缩其大小。可以像数组一样使用vector，通过运算符[ ]访问其元素，但它比数组更灵活，==当添加数据时，vector的大小能够自动增加以容纳新的元素==。下图是向量的一个示意图。

      ![img](https://p.ananas.chaoxing.com/star3/origin/8a42928a98059024e47a7d68862a880d.png) 

       V是个整型向量，begin\end是向量的头尾查找函数，rbegin、rend是反向查找向量头尾的函数。Iterator代表指向某个元素的迭代器，通过它可以遍历向量

    * List

      STL中的list是一个双向链表，可以从头到尾或从尾到头访问链表中的节点，节点可以是任意数据类型。链表中节点的访问常常通过迭代器进行。下图是链表的示意图。

      ![img](https://p.ananas.chaoxing.com/star3/origin/1370050d7016e56a2f36b6ca18e84f6d.png)

      Front找到第一个元素，back找到list的最后一个元素。迭代器用于指向链表的元素，通过它可以遍历整个链表。

      * 链表的操作

        * 链表的构造（模板参数T是链表的数据类型）

          list<T> c 建立一个空链表c

          list<T> c1(c2) 建立与c2同型的链表c1（c2的每个元素都被复制）

          list<T> c(n) 建立具有n个元素的链表c，元素值由默认构造函数产生

          list<T> c(n,e)建立n个元素的链表c，每个元素的值都是e

          list<T> c(beg, end) 建立链表c，并用[beg, end]区间内的元素作初始化

          c.~list<e>() 销毁链表c，释放内存

        * 链表赋值

          c1=c2将c2链表的全部元素赋值给c1链表

          c1.assign(n,e)将元素e拷贝n次到c1链表

          c.assign(beg,end) 将区间[beg,end]的元素赋值给c

          c1.swap(c2)将链表c1和c2的全部元素互换

        * 链表存取

          c.front()返回第一个元素，不检查元素存在与否

          c.back()返回最后一个元素，不检查元素存在与否

        * 链表插入与删除

          c.insert(pos,e)在pos位置插入元素e的副本，并返回新元素的位置

          c.insert(pos,n,e)在pos位置插入元素e的n个副本，没有返回值

          c.insert(pos, beg, end)  在pos位置插入区间[ bed, end]内的全部元素

          c.push_back(e)在尾部追加一个元素e的副本

          c.pop_back(e)删除最后一个元素

          c.push_front(e)在表头插入元素e的一个副本

          c.pop_front()删除第一个元素

          c.remove(val)删除值为val的元素

          c.remove_if(op)删除所有“造成op(e)结果为true”的元素

          c.erase(pos)删除pos指向的元素，返回下一元素的位置

          c.erase(beg, end)删除区间[beg,end]内的元素，返回下一元素位置

          c.resize(n)将链表c的大小重新设置为n 

          c.clear()删除链表所有元素，将整个容器置空

        * 链表的特殊操作

          c.unique()   删除相邻重复元素，只留一个

          c.unique(op) 若存在若干相邻且使op()操作为true的元素，删除重复，只留一个

          c1.splice(pos, c2) 将c2内的所有元素转换到c1内，pos之前

          c1.splice(pos, c2, c2pos)将c2链表的c2pos所指元素移到c1内的pos指向的位置

          c1.splice(pos, c2, c2beg, c2end) 将c2内[c2beg, c2end]区间的所有元素转换到c1内pos之前

          c.sort()  以operator<为准则，对所有元素排序

          c.sort(op)  以op()为准则，对所有元素排序

          c1.merge(c2)  c2合并到c1，若合并前有序则合后仍有序

          c.reverse()  将所有元素反序

    * Stack

      堆栈（stack）是一种较简单的常用容器，它是一种受限制的向量，只允许在向量的一端存取元素，后进栈的元素先出栈，即LIFO（last in first out）。右图是一个字符堆栈的示意图。

      ![img](https://p.ananas.chaoxing.com/star3/origin/3afbd9002021ccb0f877ecc8ca9a0c9e.png)

      Top返回栈顶元素；push是入栈操作，将元素加入栈顶。Pop是出栈操作，它删除栈顶元素。Top与pop不同，top只返回栈顶元素的值，不删除元素；pop只删除栈顶元素，不返回值。

      STL中的堆栈提供的主要操作如下：

      * push() 将一个元素加入stack内，加入的元素放在栈顶

      * top() 返回栈顶元素元素的值

      * pop() 删除栈顶元素

      * top与pop是不同的，top只返回栈顶元素的值，不删除元素，而pop只删除栈顶元素，不返回值。

    * String

      string可以被看成是以字符（characters）为元素的一种容器，字符构成序列（字符串），有时需要在字符序列中进行遍历，标准string类提供了STL容器接口，具有成员函数begin()和end()，迭代器可以用这两个函数进行定位。

      STL中的string是一种特殊类型的容器，原因是它除了可作为字符类型的容器外，更多的是作为一种数据类型——字符串，可以像int、double之类的基本数据类型那样定义string类型的数据，并进行各种运算。 

      **string的重载运算符**

      ![img](https://p.ananas.chaoxing.com/star3/origin/890d299f1168a3710b6abfae313e7650.png)

      **string的常用成员函数**

      在下面的string成员函数介绍中，假设s1、s2的定义如下：

      string s1="ABCDEFH";

      string s2="0123456123";

      string s;

      **①substr(n1, n)取子串函数**，从当前字符串的n1下标开始，取出n个字符。如“s=s1.substr(2, 3)”的结果为：s="CDE"

      **②swap(s)交换字符串。**如“s1.swap(s2)”的结果为：s1="0123456123"，s2="ABCDEFH"

      **③size()/length() 计算字符串中当前存放的字符个数。**如“s1.length()”的结果为：7

      **④capacity()计算字符串的容量（可容纳的字符个数）。**如“s1.capacity()”的结果为：31

      **⑤max_size()计算string类型数据的最大容量。**如“s1.max_size()”的结果为：4294967293 

      **⑥find(s)在当前字符串中查找子串s**，如找到就返回s在当前串中的起始位置；若没有找到，返回常数string::npos。如“s1.find("EF")”的结果为：4

      **⑦rfind(s)同find，但从后向前进行查找**。如“s1.rfind("BCD")”的结果为：1

      **⑧find_first_of(s)在当前串中查找子串s第一次出现的位置。**如“s2.find_first_of("123")”的结果为：1

      **⑨find_last_of(s)在当前串中查找子串s最后一次出现的位置。**如“s2.find_last_of("123")”的结果为：9

      **⑩replace(n1, n, s) 替换当前字符串中的字符**，n1是替换的起始下标，n是要替换的字符个数，s是用来替换的字符串。如“s1.replace(2, 3, s2)”的结果为：s1="AB0123456123FH"

      **⑾replace(n1, n, s, n2, m) n1是替换的起始下****标，n是替换掉的字符个数，s是用来替换的字符串，n2是s中用来替换的起始下标，m是s中用于替换的字符个数**。如“s1.replace(2, 3, s2, 2, 3)”的结果为：s1="AB234FH"

      **⑿insert(n, s)在当前串的下标位置n之前，插入s串**。如“s1.insert(2, "88888")”的结果为：s1="AB88888CDEFH"

      **⒀insert(n1, n, s, n2,** **m) 在当前串的n1下标后插入s串，n2是s串中要插入的起始下标，m是s串中要插入的字符个数。**如“s1.insert(2, s2, 3, 2)”的结果为：s1="AB34CDEFH"

  * 关联式容器

    STL关联式容器包括**集合和映射**两大类，集合包括set和multiset，映射包括map和multimap，它们通过关键字（也称查找关键字）存储和查找元素。在每种关联容器中，关键字按顺序排列，容器遍历就以此顺序进行。

    * set和multiset

      multiset和set提供了控制数字（包括字符及串）集合的操作，集合中的数字称为关键字，不需要有另一个值与关键字相关联。set和multiset会根据特定的排序准则，自动将元素排序，两者提供的操作方法基本相同，只是multiset允许元素重复而set不允许重复。 

      ![img](https://p.ananas.chaoxing.com/star3/origin/0be6ba42eba4389e018335d64126c319.png)

      **（1）set和multiset的定义**

      set c  建立一个空的set/multiset 集合

      set c(op)  以op为排序准则建立一个空集

      set c1(c2)  建立一个集合c1，并用c2集合初始化

      set c(beg, end) 用区间[beg, end]建立一个集合c

      set可以是：

      set/multiset<T>   建立T类型的，以less<>（从小到大）的排序集合

      set/multiset<T, op> 建立T类型的，以op指定排序规则的集合

      其中，op可以是less<>或greater<>之一，应用时须在<>中写上类型，如greater<int>。

      less指定排序方式为从小到大

      greater指定排序方式为从大到小，默认排序方式为less。

      **（2）set和multiset的赋值比较运算**

      ①set和multiset支持>、>=、<、<=、!=、==比较运算。例如，若有集合c1、c2，可以用c1==c2，c1>c2对它们进行相等或大于判断。

      ②可以赋值运算符“=”进行集合赋值，如c1=c2。

      **（3）set和multiset计算容量**

      size()  计算容器的大小

      empty() 判断容器是否为空，若为空则返回0

      max_size() 返回容器能够保存的最大元素个数

      **（4）set和multiset常用操作**

      count(e) 计算集合中元素e的个数

      find(e) 查找集合中第1次出现元素e的位置

      lower_bound(e) 查找集合中第1个“元素值>=e”的位置

      upper_bound(e) 查找集合中第1个“元素值>e”的位置

      insert(e) 在当前集合中插入元素e;

      insert(pos, e) 将e插入到pos位置

      insert(beg, end) 将[beg, end]区间内的所有元素插入到当前集合中

      erase(e)  删除集合中的元素e

      erase(pos) 删除集合中指定位置pos的元素

      erase(beg,end)  删除区间[beg,end]的所有元素

      clear()   清空集合

      begin()  指向第1个元素位置，常与迭代器结合应用

      end()   指向最后元素的下一位置，常与迭代器结合应用

    * map和multimap

      map和multimap提供了操作<键,值>对的方法（其中的值也称为映射值），它们存储一对对象，即键对象和值对象，键对象是用于查找过程中的键，值是与键对应的附加数据。map中的元素不允许重复，而multimap中的元素是可以重复的。 

      ![img](https://p.ananas.chaoxing.com/star3/origin/2f45e283f7b9541d4c31b3c94af543ee.png)

      **map和multimap的常用操作：**

      **·** insert(e)  //将元素e插入到map，multimap，set，multiset

      **·** make_pair(e1,e2)//构造映射的元素

      **·** map/multimap类型的迭代器提供了两个数据成员：一个是first，用于访问键；另一个是second，用于访问值。

    

* 迭代器

  

* 算法

## 5.迭代器

## 6.算法