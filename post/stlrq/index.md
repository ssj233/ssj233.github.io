# STL容器

# 另外的补充(！必读！)

## 针对大多数容器

### 列表初始化
```cpp
列表初始化(cpp11)
int a=1;//一般的初始方式

// 但是如果int a=一个大于int的数;
// 仍能运行，但是有编译错误
// 如果改成
int a{一个大于int的数};
// 系统就会编译错误

// 列表初始化还有一个用法，在系统已经确定此处的类型时，调用相应构造函数
stack<pair<int,int>>a;

// 如果不用列表初始化,就应a.push(make_pair(123,321));
// 但如果用就可以a.push({123,321});
```
### auto类型
```cpp
// auto :让系统来判定此处的类型
auto x=123//此时系统会把此处的auto看作int
auto x='A'//此时系统会把此处的auto看作char
// auto 的变量必须有初始值
```
### 引用
```cpp
//什么是引用
在交换两数时，我们会这样写
void swap(int &a,int &b){int t=a;a=b;b=t;}
swap(x,y);

// 这里的&，不是取址，表引用
// 在C中因为没有引用，会用指针代替，但指针不安全
// 所以c++中引入了引用这个概念，可以把它看作没有*的指针

int a=&b;//a为b的引用
int *a=&b;//指针a为b的地址

//使用指针达成同样的效果
void swap(int *a,int *b){int t=*a;*a=*b;*b=t;}
swap(&x,&y);
```
### 通用
>所有容器均能有构造函数：
>
>        容器<阿巴阿巴>a(另一个相同类型的容器)
>
>和其他的函数
>
>        .clear()清空a
>        .size()返回当前容器元素个数
>        .max_size()返回容器a最大空间,array没有这个函数
>        .empty()若a为空，返回1，否则返回0

### 迭代器使用

```cpp
以vector为例
vector<int>a;
vector<int>::iterator it;//定义用于vector<int>的迭代器
it=a.begin()//令it等于a的首迭代器

//对于大多数容器，都有这几个返回迭代器的函数
a.begin()//开始
a.end()//结束
a.rbegin()//反转的开始
a.rend()//反转的结束

注：  end()//指向的是末尾元素+1的迭代器

for(auto i=a.begin();i!=a.end();i++) cout<<*i<<endl;
//遍历，i表迭代器  *i指向的值，与指针很类似

for(auto i=a.rbegin();i!=a.rend();i++) cout<<*i<<endl;
//使用反向迭代器

// 类python的遍历(cpp11)

for(int i:a) cout<<i<<endl;//这会输出a中的值

//普通数组也能这样遍历整个数组
```


# array(数组)

```cpp
#include<array>
array<int,114514>a;
//array必须的长度是constexpr的或准确的数字,

int a[114514];
//有区别吗[doge]

// array可以用迭代器
array<int,123>a;
// 批量初始化
a.fill(123);//让a内所有元素都为123
```

# vector(动态数组 ,向量)
## 定义
```cpp
#include<vector>
vector<int>a;
vector<int>a(2,3);//初始化为3个2
vector<int>a(迭代器，迭代器)//定义，从别的容器初始化
```
## 成员函数
>        .at(x)相当于返回第x个元素的引用
>        .push_back(x)在尾部插入
>        .pop_bakc()删除末尾
>        .front()首的元素
>        .back()尾的元素
>        .insert(迭代器,数)插入
>        .insert(迭代器,个数,数)插入多个
>        .insert(迭代器，另一个容器的迭代器，另一个容器的迭代器)从别的容器插入
>        .erase(迭代器)删除
>        .erase(迭代器，迭代器)删除区间
>        .resize(数)设置大小，多的地方用0填充,少的地方直接删除
>重载了运算符
>
>        []  用于像数组一样访问元素
>
>
>a.swap(b);交换
>
>swap(a,b);同上
>

> 对于vector的sort使用
>
>       vector<int>a;
>       sort(a.begin(),a.end());全部排序
>       sort(a.begin(),a.begin()+123);
>       这里sort会将a中0~122的排序
>       sort(a.begin(),a.begin()+123,cmp);

## 内存管理与时间效率(选读)

.reserve(n)

使用reserve()函数提前设定容量大小，避免多次容量扩充操作导致效率低下。

当进行insert或push_back等增加元素的操作时，如果此时动态数组的内存不够用，

就要动态的重新分配当前大小的1.5~2倍的新内存区，再把原数组的内容复制过去。就会浪费时间


对于vector容器来说，如果有大量的数据需要进行push_back，

应当使用reserve()函数提前设定其容量大小，否则会出现许多次容量扩充操作，导致效率低下。

(1) size()告诉你容器中有多少元素。它没有告诉你容器为它容纳的元素分配了多少内存。

(2) capacity()告诉你容器在它已经分配的内存中可以容纳多少元素。

    那是容器在那块内存中总共可以容纳多少元素，而不是还可以容纳多少元素。

(3) resize(n)强制把容器改为容纳n个元素。调用resize之后，size将会返回n。



# list(链表)

```cpp
list<int>a;
list<int>lint1(a);
```
## 成员函数
>        .push_back(x)在尾部插入
>        .push_front(x);在头部插入
>        .insert()具有与vector相同的insert方法
>        .erase()具有与vector相同的erase方法
>        .remove(x)删除值为x的元素
>        .remove_if(条件) 删除条件满足的元素,参数为自定义的函数
>        .sort(); 专用的sort,默认升序
>        .sort(cmp函数)自定义条件排序
>        .reverse()反转list
>
>.sort和std::sort的cmp用法一样
>
>   bool sortfunc(int a, int b) { return a > b ? 1 : 0; }
>
>   a.sort(sortfunc);
>


# deque(双端队列)
## **双端队列，两端均可进队和出队**
## 定义
```cpp
#include<deque>
deque<int>a;
```
## deque没有reserve()

除此之外，deque拥有vector的所有构造和其他函数和重载运算符

此外deque还有:
>            .push_front(x)在头部插入
>            .pop_front在头部删除


# stack(栈)
## 定义
```cpp
deque<int>a;
stack<int> s(a);//可以用deque或stack初始化
```
## 成员函数
>        .top()返回头的引用，可以看作返回一个值
>        .pop()出栈
>        .push(x)入栈


# queue(队列)
## 定义
```cpp
deque<int>a;
queue<int> q1(a);//可以用deque或queue初始化
```
## 成员函数
>        .back()返回尾的引用，可以看作返回尾的值
>        .front()返回头的
>        .push(x)入队
>        .pop()出队

```cpp
//补充:BFS (使用STL的queue)
while(!q1.empty())
{
    int h=q1.front()
    q1.pop()...
    for(...)
    {
        if(...)
        {
            q1.push(...)
            if(q1.back()....)
            {

            }
        }
    }
}

```

# priority_queue(用优先队列实现的堆)
## 定义
```cpp
#include <queue>
priority_queue<int>a//大根堆
priority_queue<int,vector<int>,less<int> > s1;//大根堆
priority_queue<int,vector<int>,greater<int> > s;//小根堆
struct node {
    //对于自定义的结构，不要忘记重载运算符
    int x,y;
    //对于less,重载<,,堆顶为最大的
    friend bool operator<(node a, node b){return a.x < b.x;}
    //对于greater,重载>,,堆顶为最小的
    friend bool operator>(node a, node b){return a.x > b.x;}
};
```
## 成员函数
>         .top()返回堆顶
>         .push(x)添加x到堆顶
>         .pop()弹出堆顶

# set&multiset

`multi`表能够重复元素

# 定义
```cpp
set<int>a;//顺序遍历时是升序
set<int,less<int>>a;//同上，但结构体必须用这种方法或下面的方法
set<int,greater<int>>aa;//顺序遍历时是降序
```

## 成员函数
>        .count(x)           返回某个值元素是否存在
>        .find(x)            返回迭代器，没有返回a.end
>        .erase(x)           删除
>        .erase(从迭代器,到迭代器)   删除
>        .insert(x)          在集合中插入元素
>        .lower_bound()   返回指向大于等于某值的第一个元素的迭代器
>        .upper_bound()   返回大于某个值元素的迭代器
>
>set可以像数组一样遍历,顺序如上
>
>    for(auto i:a){}
>
>    for(auto i=a.begin();i!=a.end();i++){}



# pair(双元组)

```cpp
#include<bits/stl_pair.h>
pair<int, int> p[1000];

//下面三种赋值语句效果相同
p[0]={1,1};
p[1]=make_pair(1,1);
p[2].first=1;p[0].second=1;
p[3]=pair<int,int>(1,1);

pair相当于
struct pair{int first,second;};

```
# map&multimap

> 在set中，元素既是实值又是键值
>
> 但map中，键值和实值分开
>
> 注:键值用于索引的值，实值顾名思义


> map可以相当于
>
> `bool operator<(pair<int,int>a,pair<int,int>b){return a.first<b.first;}`
>
> `set<pair<int,int>>a;`

## 定义
```cpp
#include<map>
map<int,int>a;//顺序遍历时是升序
map<int,int,greater<int>>b;//顺序遍历时是降序
map<键值类型，实值类型>a;
map内部用了pair
.first是键值
.second是实值
```
## 成员函数&使用
>         .erase(x)删除键值为x的
>         .erase(it)删除（迭代器）
>         .count(x)找键值为x的，有返回1，否则0
> 插入元素
>
>         a[1]=2令键值为1的等于2
>         .insert({1,2})同上
>         .insert(make_pair(1,2))同上
>
> map也可以向上面set一样遍历，但是按键值遍历
>
> for(auto i=a.begin();i!=a.end();i++){}
>
> for(pair<int,int> i:a){}


# bitset(二进制的)
> 貌似用的不多
>
> bitset不支持负数
>
> bitset<1>可以当作bool来使用，所占空间更小
>
> bool占一字节，bitset的每一位只占1bit，转int,string也更方便
>
> bitset可用于表示大的二进制数(只要不转成int溢出就没事)

## 定义
```cpp
#include<bitset>
bitset<3>a;定义3位的二进制数a
bitset<3>b(a);定义3位的二进制数b=a
bitset<3>b("110");定义b，使得b的值转化成二进制为110
bitset<3>c(114514);定义c,使得c的值转化成十进制为114514

bitset<3>a("100")
此时a[2]=1;
   a[1]=0;
   a[0]=0;
```

## 成员函数&运算符重载
>        .all()返回true如果所有位都为1
>        .any()返回true如果有>=1个位为1
>        .count()返回位为1的个数
>        .none()返回的值与all()相反
>        .flip()所有位取反
>        .flip(x)第x位取反
>        .set()全部变1
>        .set(x)x位变1
>        .reset()和.reset(x)与set同理，但是变0
>        ._Find_first()返回从第0位找第一个1的位
>        ._Find_next(x)同上,但是从x开始
>        .to_string()返回一个bitset转string,
>        .to_ullong()转unsigned long long
>        .to_ulong()转unsigned int
>
>        ^,~,>>等位运算符，但操作的的两个数必须都必须是位数相同的bitset



# 此外的此外[doge]:)

对于大多数stl容器，可以指定其内部实现方法（继承自上一个容器）

如`stack`,`queue`

我们可以把他们称之为适配器

比如`stack`内部默认用`deque`实现，但也可以指定它用`vector`来实现

`stack<int,vector<int>>a;`

但有的容器却不能这样，如`queue`不能构建于`vector`上，

因为`vector`不支持在最前面弹出元素

此外的此外的此外,

还是以`stack`为例

`stack`存在一个构造函数在默认`deque`的实现下

可以用`deque`来初始化`stack`

`deque<int>d;`

`stack<int>a(d);`

当然,改成用`vector`实现后也可以有

`vector<int>v;`

`stack<int,vector<int>>a(v);`

## 迭代器

```cpp
//让我们来复习指针先
int a[114514],b;
//指针指向变量在内存中的首地址
int *it=&b;//使用&取址符获取变量地址
*it=114514;相当于b=114514;
//因为it指向b,所以*it相当于b

it=a;//让it指向a的首地址(it=&a[0])
it++;//让it指向a[1]
it=a+3;//让it指向a[3]
//因为数组在内存中的地址是连续的
//所以it++能切换到a的下一个位置
//不要越界了！！！！！！

对于结构体,用->访问成员
struct node{int a,b};
node *it;
node b;
it=&b;
it->a=1;
it->b=2;


//迭代器同理
//可以看作是专用于STL的"高级特工指针"
//那么迭代器高级在何处
//例如，在STL中list的每一个元素存储在内存中的地址不一定是连续的
//这时用指针++访问下一个元素就会 :D
//但迭代器重载了++和--，使得++和--总是指向下一个或上一个位置


定义：

容器名<阿巴阿巴>::iterator 迭代器名;
vector<int>a;//定义vector a
vector<int>::iterator it;//定义用于vector<int>的迭代器

it=a.begin();//让it指向a的首地址
it++;//it迭代到下一个位置
*(it+3)=114514//令it迭代到的第3个位置为114514
vector<node>a;
it=a.begin()//让it指向a的首地址
(*it).a=114514  //访问元素
it->a=114514  //同上

//在STL中任意容器a.end()总是指向的是容器尾+1

//所以遍历时
for(auto i=a.begin();i!=a.end();i++)
{
    //阿巴阿巴阿巴阿巴阿巴阿巴
}
有的容器有反向迭代器
for(auto i=a.rbegin();i!=a.rend();i++)
{
    //阿巴阿巴阿巴阿巴阿巴
}

```


## 关于struct的更多important补充

```cpp
struct complex
{
    double real,image;
    void add(complex t)//
    {
        real=real+t.real;//real指当前节点的real（ this->real ）, t.real指参数complex t中的real
        image=image+t.image;
   }
    //一个简单的结构体成员函数
};

a.real=1,a.image=3;
b.real=3,b.image=4;
a.add(b);
cout<<a.real<<' '<<a.image;

//构造函数是在定义结构体变量时被调用的函数，它的作用是对结构体成员进行初始化
//构造函数的函数名必须与结构体名相同，并且无返回类型。
//如果用户没有增加构造函数，也没有修改默认构造函数，则默认构造函数可以省略,如果用户自己定义了构造函数，则默认构造函数必须写上。

struct complex
{
    double real,image;
    complex() //默认构造函数
    {}
    complex(double a,double b) //用户自己定义的构造函数
    {real=a,image=b;}
    //另一种写法
    complex(double a,double b):real(a),image(b){}
    //推荐的写法——更快且能初始化常量
}a;


//使用构造函数赋值
complex a(1.1,2.0);
for(int i=0;i<100;i++) arr[i]=complex(i,i);

struct complex
{
   double real,image;
   friend bool operator==(complex a,complex b){
        return a.real==b.real  &&  a.image==b.image;
    }//推荐的写法
    bool operator==(complex bb)
    {return (a == bb.a) && (b == bb.b);}
    //两种等号重载方法，其他也同理
    //priority_queue只能用第一种,而非这种
    complex operator +(complex temp)
    {return complex(real+temp.real,img+temp.img);}
    //重载加号
}

//任何类型的数组都可以用sort进行进行排序，只要定义了这种类型的“<”运算。
//我们自定义的类型，如果定义了小于关系，则可以被sort当做基本数据类型来进行排序了
//简单的说，就是把cmp写到了结构体里面

在结构体外也能定义
bool operator==(complex a,complex b){return a.real==b.real  &&  a.image==b.image;}
```
