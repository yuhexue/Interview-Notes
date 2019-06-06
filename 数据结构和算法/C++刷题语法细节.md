### cin,cout的细节规范：

输入输出缓冲区：

键盘输入字符串时需要敲一下回车键才能够将这个字符串送入到缓冲区中，并且敲入的这个回车键(\r)会被转换为一个换行符\n，这个换行符\n也会被存储在cin的缓冲区中并且被当成一个字符来计算！（非输入性地添加换行符）

 

cin读取数据也是从缓冲区中获取数据，缓冲区为空时，cin的成员函数会阻塞等待数据的到来，一旦缓冲区中有数据，就触发cin的成员函数去读取数据。（实现间断输入）

#### cin的常用读取方法

使用cin从标准输入读取数据时，通常用到的方法有cin>>，cin.get，cin.getline。

cin>>可以连续从键盘读取想要的数据，以空格、tab或换行作为分隔符。实例程序如下

```C++
#include <iostream> 
using namespace std; 
int main() 
{ 
    char a; 
    int b; 
    float c; 
    cin>>a>>b>>c; 
    cout<<a<<" "<<b<<" "<<c<<" "<<endl; 
    system("pause"); 
    return 0; 
}
```

如果空格tab换行符是要输入的数据怎么办？

cin>>从缓冲区中读取数据时，若缓冲区中第一个字符是空格、tab或换行这些分隔符时，cin>>会将其忽略并清除，继续读取下一个字符，若缓冲区为空，则继续等待。但是如果读取成功，字符后面的分隔符是残留在缓冲区的，cin>>不做处理。 
 不想略过空白字符，那就使用 noskipws 流控制。比如cin>>noskipws>>input; 

 

总结，键盘输入时用分隔符分开数据，enter既可作为分隔符，还作为唯一数据送入符，并且会被转化为数据\n。

cin>>从缓冲区读取数据时，空则等待，分割符会被忽略且不做清除处理。不想略过空白字符，还要特别使用 noskipws 流控制。比如cin>>noskipws>>input;

注意：cin>>noskipws>>input;会将分割符都作为输入字符，不存在数据之间地分割，如果分割符被填充到string数据类型,会被忽略掉，无输出。会产生很多问题，比如int，string数据如何去处理。

 

getline(cin,data);   //接受一个字符串，可以*接收分割符并输出*

 ```C++
#include <string> 
#include <iostream> 
using namespace std; 

int main() 
{ 
    char a; 
    int b;
    float c; 
    string str; 
    cin>>a>>b>>c>>str; 
    cout<<a<<" "<<b<<" "<<c<<" "<<str<<endl; 

    //////////////程序分段
    string test; 
    getline(cin,test);//不阻塞 
    cout<<"test:"<<test<<endl; 
    system("pause"); 
    return 0; 
}
 ```

cin>>对缓冲区中的第一个换行符视而不见，采取的措施是忽略清除，继续阻塞等待缓冲区有效数据的到来。
程序分段但是，getline()读取数据时，并非像cin>>那样忽略第一个换行符，getline()发现cin的缓冲区中有一个残留的换行符，不阻塞请求键盘输入，直接读取，送入目标字符串后，再将换行符替换为空字符’\0’，因此程序中的test为空串。对空格和tab如实输出。 

 

cin.get()   //有重载情况的函数

读取一个字符

### 

```
#include <iostream>
using namespace std;
int main()
{
    char a;
    char b;
    a=cin.get();
    cin.get(b);
    cout<<a<<b<<endl;
    system("pause");
    return 0;
}
```

cin.get()从输入缓冲区读取单个字符时不忽略分隔符，直接将其读取。cin.get()的返回值是int类型，读取字符的ASCII码值，遇到文件结束符时，返回EOF，即-1。

 

cin.get读取一行

```C++
#include <iostream>
using namespace std; 

int main() 
{ 
    char a; 
    char array[20]={NULL}; 
    cin.get(array,20); 
    cin.get(a); cout<<array<<" "<<(int)a<<endl; 
    system("pause"); 
    return 0; 
} 
```

cin.get(array,20);读取一行时，遇到换行符时结束读取，但是不对换行符进行处理，换行符仍然残留在输入缓冲区。第二次由cin.get()将换行符读入变量b，打印输入换行符的ASCII码值为10。这也是cin.get()读取一行与使用getline读取一行的区别所在。getline读取一行字符时，默认遇到’\n’时终止，并且将’\n’直接从输入缓冲区中删除掉，不会影响下面的输入处理。

（2）cin.get(str,size);读取一行时，只能将字符串读入C风格的字符串中，即char*，但是C++的getline函数可以将字符串读入C++风格的字符串中，即string类型。鉴于getline较cin.get()的这两种优点，建议使用getline进行行的读取。

endl输出一个换行符，并立即刷新缓冲区。









迭代器：advance,next,prev

list<int> mylist;

for (int i=0; i<10; i++) 

mylist.push_back (i*10);

list<int>::iterator it = mylist.begin();

advance (it,5);   //将迭代器it，移动n位。

 

next (it,n);   //后n个，缺省为1

prev(it,n);   //前n个，缺省为1

 

一个细节：end()指向数组最后一个元素的后面（也就是第一个空）

 

int abs(int   i) 返回整型参数i的绝对值     
 double cabs(struct   complex   znum) 返回复数znum的绝对值     
 double fabs(double   x) 返回双精度参数x的绝对值     
 long labs(long   n) 返回长整型参数n的绝对值  





ListNode* p = new ListNode(val);

P->next = new ListNode(val);

TreeNode* q = new ListNode(val);

注意新节点的实例化（内存分配）

 

对于空指针不允许访问其属性，所以访问前应判断是否非空。

 

int arr = {1,2}; 

int* p = arr;

int* q = p;

int** ptr = &p;

区别二级指针和一级指针的初始化。*p和p和&p分别是value，指向内存的地址，指针本身的地址。那么p，q的赋值都是指向内存的地址。**ptr对应*p和arr,*ptr对应p，ptr对应&p。二级指针变量名对应于其所指向的一级指针本身的内存地址。

 

链表节点的删除：

//删除当前节点

ListNode* temp = head;   

head = head->next;

delete temp;

//删除next节点

ListNode* temp = head->next; 

head->next = temp->next; 

delete temp;

 

从ListNode指针看指针的功能，ListNode* q = p, 将指针p赋给了q，两个指针同时指向一个ListNode节点（节点属性改变将影响所有指向该节点的指针），该节点也只包含该节点处的next等属性，对后续的不知，一个指针在一个状态里只能访问到一个节点。

 

链表指针（节点）在进行访问时要注意是否为空，对空指针的许多操作是非法的。

 

 

C风格字符串和string字符串：

C风格：char str[] = {‘a’,’\0’,’\0’}; char* str = “hello”;都可以。以’\0’作为字符串结束标志，数组边界范围应计入’\0’，strlen不计入’\0’,是实际长度。Sizeof（str）是分配长度，’\0’会计入。

凡是讨论字符串长度，都不应考虑\0。C风格就当作字符数组来操作，当然它也有一些标准库函数。String使用通用stl方法。

 

new是堆分配的内存，故函数调用结束后，不会自行撤销，故需要在函数加入delete []ptr ;语句。

 

首先说明\0的问题：

由于string的定义里有长度信息，所以不再强制需要一个\0作为结束标志
 位于string中间位置，而不是最后一个位置的'\0'，不作为结束符处理。
 但是因为要和C串兼容，所以大部分实现，还是以'\0'结束的，
 也许C++标准里，string中，'\0'没有了结束符的特殊地位。
 然而，任何一个比较好的实现，应该都还保留'\0'作为结束符的习惯。
 只是用cout 输出的时候，中间的'\0'不再是结束符，而是看做空格处理。

string s1 = "ab\0c";

string s2 = "abAc"+”haha”;

s2[2] = '\0';

cout << "s1 is "<< s1 << endl;  //ab

cout << s2 << endl; // ab c

区别这段程序，”XXXXX”被认为是C风格字符串。注意在初始化中，””C风格字符串被赋给s1,也就是s1实际上只得到了\0之前的元素。但是实际上对s2[2]赋值是有效的。

 

其次说明空间问题：

string 在内部维护了3个变量

(1)  size :  表示字符串的长度（不计入末尾的\0），length()也有这个作用。
 (2) 指向所分配内存的指针
 (3) capacity : 该string 类型在不需要重新分配内存的情况下最多能存储多少字符

String更像一个vector，它是动态的，像静态数组和vector，C风格字符串和string，解决了容易越界的问题。

 注意：string::[]不做边界检查，遍历访问不会越界，程序仍可执行。初始化，元素的访问和修改与vector一致。

 

