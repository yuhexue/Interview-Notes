### vector（扩容和释放）

连续存储的容器，动态数组，在堆上分配空间

底层实现：数组

扩容：两倍或1.5倍容量扩容，如未超过当前容量，直接添加到最后并调整迭代器。如果没有剩余空间，则重新配置两倍或1.5倍空间，然后将原空间元素通过复制的方式初始化新空间，再向新空间增加元素。最后析构并释放原空间，之前的迭代器会失效。

释放：clear方法会删除所有的元素（被销毁），留下 size 为 0 的容器

性能：

访问： O(1)
插入：在最后插入（空间够）：很快
在最后插入（空间不够）：需要内存申请和释放，以及对之前数据进行拷贝。
在中间插入（空间够）：内存拷贝
在中间插入（空间不够）：需要内存申请和释放，以及对之前数据进行拷贝。
删除：在最后删除：很快
在中间删除：内存拷贝

适用场景：经常随机访问，且不经常对非尾节点进行插入删除。

### list

动态链表，在堆上分配空间，不连续存储。每插入一个元数都会分配空间，每删除一个元素都会释放空间。
底层：双向链表

性能：

访问：随机访问性能很差，只能快速访问头尾节点。
插入：很快，一般是常数开销
删除：很快，一般是常数开销

适用场景：经常插入删除大量数据

### vector和list区别和应用：

1）vector 底层实现是数组；list 是双向链表。
2）vector 支持随机访问，list 不支持。
3）vector 是顺序内存，list 不是。
4）vector 在中间节点进行插入删除会导致内存拷贝，list 不会。
5）vector 一次性分配好内存，不够时才进行2 倍扩容；list 每次插入新节点都会进行内存申请。
6）vector 随机访问性能好，插入删除性能差；list 随机访问性能差，插入删除性能好。

vector 适用需要高效随机访问，而不在乎插入和删除的效率。

list 适用需要高效的插入和删除，而不关心随机访问。

### map和set，multi 和unordered等等

map 和set 都是关联容器，其底层实现都是红黑树（RB-Tree）。

map 和set 区别在于：
（1）map 中的元素是key-value（关键字—值）对：关键字起到索引的作用，值则表示与索引相关联的数据；Set 与之相对就是关键字的简单集合，set 中每个元素只包含一个关键字。
（2）set 的迭代器是const 的，不允许修改元素的值；map 允许修改value，但不允许修改key。其原因是因为map 和set 是根据关键字排序来保证其有序性的，如果允许修改key 的话，那么首先需要删除该键，然后调节平衡，再插入修改后的键值，调节平衡，如此一来，严重破坏了map 和set 的结构，导致iterator 失效，不知道应该指向改变前的位置，还是指向改变后的位置。所以STL 中将set 的迭代器设置成const，不允许修改迭代器的值；而map 的迭代器则不允许修改key 值，允许修改value 值。
（3）map 支持下标操作，set 不支持下标操作。map 可以用key 做下标，map 的下标运算符[ ]将关键码作为下标去执行查找，如果关键码不存在，则插入一个具有该关键码和mapped_type类型默认值的元素至map 中，因此下标运算符[ ]在map 应用中需要慎用，const_map 不能用，只希望确定某一个关键值是否存在而不希望插入元素时也不应该使用，mapped_type 类型没有默认值也不应该使用。如果find 能解决需要，尽可能用find。

multi在此基础上允许key重复，底层实现不变

### STL的allocaotr （分配器）

STL 的分配器用于封装STL 容器在内存管理上的底层细节。在C++中，其内存配置和释放如下：
new 运算分两个阶段：

(1)调用::operator new 配置内存;

(2)调用对象构造函数构造对象内容delete 运算分两个阶段：(1)调用对象希构函数；(2)掉员工::operator delete 释放内存为了精密分工，STL allocator 将两个阶段操作区分开来：内存配置有alloc::allocate()负责，内存释放由alloc::deallocate()负责；对象构造由::construct()负责，对象析构由::destroy()负责。
同时为了提升内存管理的效率，减少申请小内存造成的内存碎片问题，SGI STL 采用了两级配置器，当分配的空间大小超过128B 时，会使用第一级空间配置器；当分配的空间大小小于128B时，将使用第二级空间配置器。第一级空间配置器直接使用malloc()、realloc()、free()函数进行内存空间的分配和释放，而第二级空间配置器采用了内存池技术，通过空闲链表来管理内存。

### 迭代器

#### 概念

Iterator（迭代器）模式又称Cursor（游标）模式，用于提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示。或者这样说可能更容易理解：Iterator 模式是运用于聚合对象的一种模式，通过运用该模式，使得我们可以在不知道对象内部表示的情况下，按照一定顺序（由iterator 提供的方法）访问聚合对象中的各个元素。
由于Iterator 模式的以上特性：与聚合对象耦合，在一定程度上限制了它的广泛运用，一般仅用于底层聚合支持类，如STL 的list、vector、stack 等容器类及ostream_iterator 等扩展iterator。

#### 迭代器和指针的区别

迭代器不是指针，是类模板，表现的像指针。他只是模拟了指针的一些功能，通过重载了指针的一些操作符，->、、++、--等。迭代器封装了指针，是一个“可遍历STL（ Standard Template Library）容器内全部或部分元素”的对象， 本质是封装了原生指针，是指针概念的一种提升（lift），提供了比指针更高级的行为，相当于一种智能指针，他可以根据不同类型的数据结构来实现不同的++，--等操作。
迭代器返回的是对象引用而不是对象的值，所以cout 只能输出迭代器使用取值后的值而不能直接输出其自身。*

#### 迭代器作用

Iterator 类的访问方式就是把不同集合类的访问逻辑抽象出来，使得不用暴露集合内部的结构而达到循环遍历集合的效果。

这个主要考察的是迭代器失效的问题。

1.对于序列容器vector,deque 来说，使用erase(itertor)后，后边的每个元素的迭代器都会失效，但是后边每个元素都会往前移动一个位置，但是erase 会返回下一个有效的迭代器；

2.对于关联容器map set 来说，使用了erase(iterator)后，当前元素的迭代器失效，但是其结构是红黑树，删除当前元素的，不会影响到下一个元素的迭代器，所以在调用erase 之前，记录下一个元素的迭代器即可。

3.对于list来说，它使用了不连续分配的内存，并且它的erase 方法也会返回下一个有效的iterator，因此上面两种正确的方法都可以使用。

### 请你回答一下STL里resize和reserve的区别

resize()：改变当前容器内含有元素的数量(size())，eg: vector<int>v; v.resize(len);v的size 变为len,如果原来v 的size 小于len，那么容器新增（len-size）个元素，元素的值为默认为0.当v.push_back(3);之后，则是3 是放在了v 的末尾，即下标为len，此时容器是size为len+1；
reserve()：改变当前容器的最大容量（capacity）,它不会生成元素，只是确定这个容器允许放入多少对象，如果reserve(len)的值大于当前的capacity()，那么会重新分配一块能存len 个对象的空间，然后把之前v.size()个对象通过copy construtor 复制过来，销毁之前的内存；

```C++
#include<iostream>
#include<vector>
using namespace std;

int main() {
    vector<int> a;
    a.reserve(100);
    a.resize(50);
    cout<<a.size()<<" "<<a.capacity()<<endl;
    //50 100
    a.resize(150);
    cout<<a.size()<<" "<<a.capacity()<<endl;
    //150 200

    a.reserve(50);
    cout<<a.size()<<" "<<a.capacity()<<endl;
    //150 200
    a.resize(50);
    cout<<a.size()<<" "<<a.capacity()<<endl;
    //50 200
}
```

### 写string类的构造，析构，拷贝函数

```C++
//String类的原型如下
class String
{
  public:
         String(const char *str=NULL); //构造函数
         String(const String &other); //拷贝构造函数
         ~String(void); //析构函数
         String& operator=(const String &other); //等号操作符重载
         ShowString();

  private:
         char *m_data; //指针
};

String::~String()
{
   delete [] m_data; //析构函数，释放地址空间
}
String::String(const char *str)
{
   if (str==NULL)//当初始化串不存在的时候，为m_data申请一个空间存放'\0'；
    {
       m_data=new char[1];
       *m_data='\0';
    }
   else//当初始化串存在的时候，为m_data申请同样大小的空间存放该串；
    {
       int length=strlen(str);
       m_data=new char[length+1];
       strcpy(m_data,str);
    }
}

String::String(const String &other)//拷贝构造函数，功能与构造函数类似。
{
   int length=strlen(other.m_data);
   m_data=new [length+1];
   strcpy(m_data,other.m_data);
}
String& String::operator =(const String &other) 
{
   if (this==&other)//当地址相同时，直接返回；
       return *this; 

   delete [] m_data;//当地址不相同时，删除原来申请的空间，重新开始构造；
   int length=sizeof(other.m_data);
   m_data=new [length+1];
   strcpy(m_data,other.m_data);
   return *this; 
}

String::ShowString()//由于m_data是私有成员，对象只能通过public成员函数来访问；
{
     cout<<this->m_data<<endl;
}

int main()
{
    String AD;
    char * p="ABCDE";
    String B(p);
    AD.ShowString();
    AD=B;
    AD.ShowString();
}
```

| 容器                                                         | 底层数据结构      | 时间复杂度                                                | 有无序 | 可不可重复 | 其他                                                         |
| ------------------------------------------------------------ | ----------------- | --------------------------------------------------------- | ------ | ---------- | ------------------------------------------------------------ |
| [array](https://github.com/huihut/interview/tree/master/STL#array) | 数组              | 随机读改 O(1)                                             | 无序   | 可重复     | 支持快速随机访问                                             |
| [vector](https://github.com/huihut/interview/tree/master/STL#vector) | 数组              | 随机读改、尾部插入、尾部删除 O(1) 头部插入、头部删除 O(n) | 无序   | 可重复     | 支持快速随机访问                                             |
| [list](https://github.com/huihut/interview/tree/master/STL#list) | 双向链表          | 插入、删除 O(1) 随机读改 O(n)                             | 无序   | 可重复     | 支持快速增删                                                 |
| [deque](https://github.com/huihut/interview/tree/master/STL#deque) | 双端队列          | 头尾插入、头尾删除 O(1)                                   | 无序   | 可重复     | 一个中央控制器 + 多个缓冲区，支持首尾快速增删，支持随机访问  |
| [stack](https://github.com/huihut/interview/tree/master/STL#stack) | deque / list      | 顶部插入、顶部删除 O(1)                                   | 无序   | 可重复     | deque 或 list 封闭头端开口，不用 vector 的原因应该是容量大小有限制，扩容耗时 |
| [queue](https://github.com/huihut/interview/tree/master/STL#queue) | deque / list      | 尾部插入、头部删除 O(1)                                   | 无序   | 可重复     | deque 或 list 封闭头端开口，不用 vector 的原因应该是容量大小有限制，扩容耗时 |
| [priority_queue](https://github.com/huihut/interview/tree/master/STL#priority_queue) | vector + max-heap | 插入、删除 O(log2n)                                       | 有序   | 可重复     | vector容器+heap处理规则                                      |
| [set](https://github.com/huihut/interview/tree/master/STL#set) | 红黑树            | 插入、删除、查找 O(log2n)                                 | 有序   | 不可重复   |                                                              |
| [multiset](https://github.com/huihut/interview/tree/master/STL#multiset) | 红黑树            | 插入、删除、查找 O(log2n)                                 | 有序   | 可重复     |                                                              |
| [map](https://github.com/huihut/interview/tree/master/STL#map) | 红黑树            | 插入、删除、查找 O(log2n)                                 | 有序   | 不可重复   |                                                              |
| [multimap](https://github.com/huihut/interview/tree/master/STL#multimap) | 红黑树            | 插入、删除、查找 O(log2n)                                 | 有序   | 可重复     |                                                              |
| hash_set                                                     | 哈希表            | 插入、删除、查找 O(1) 最差 O(n)                           | 无序   | 不可重复   |                                                              |
| hash_multiset                                                | 哈希表            | 插入、删除、查找 O(1) 最差 O(n)                           | 无序   | 可重复     |                                                              |
| hash_map                                                     | 哈希表            | 插入、删除、查找 O(1) 最差 O(n)                           | 无序   | 不可重复   |                                                              |
| hash_multimap                                                | 哈希表            | 插入、删除、查找 O(1) 最差 O(n)                           | 无序   | 可重复     |                                                              |

容器库：https://zh.cppreference.com/w/cpp/container
