## STL提供的Sort算法

### **所有Sort算法介绍**

所有的sort算法的参数都需要输入一个范围[begin，end)。如果你需要自己定义比较函数，你可以把你定义好的仿函数作为参数传入。每种算法都支持传入比较函数。

以下是所有STL_sort算法函数名称：

|函数名|功能描述|
|-------------|------------------------|
|sort|对给定区间的所有元素进行排序|
|stable_sort|对给定区间所有元素进行稳定排序|
|partial_sort|对给定区间所有元素部分排序|
|partial_sort_copy|对给定区间复制并排序|
|nth_element|找出给定区间某个位置对应的元素|
|is_sorted|判断一个区间是否已经排好序|
|partition|使得符合某个条件的元素放在前面|
|stable_partition|相对稳定的使得符合某个条件的元素放在前面|

### **sort中的比较函数**

当你需要按照某种特定方式进行排序时，你需要给sort指定比较函数，否则程序会自动提供给你一个比较函数。

```cpp
vector <int> vec;
sort(vec.begin(),vec.end());
///此时相当于调用
sort(vec.begin(),vec.end(),less <int> ());

```
上述例子中系统自己为sort提供了less仿函数。在STL中还提供了其他函数。

以下是仿函数表：

|名称|功能描述|
|-------------|----------------|
|equal_to|相等|
|not_equal_to|不相等|
|less|小于|
|greater|大于|
|less_equal|小于等于|
|greater_equal|大于等于|

注意：
这些函数不是都能适用于你的sort算法，如何选择，决定于你的应用。

当你的容器中元素是一些标准类型 int,float,char或string时，你可以直接使用这些函数模板。但如果你是自己定义的类型或者你需要按照其他方式排序，你可以有两种方式来达到效果：一种是自己写比较函数；另一种是重载类型的“<”操作符。

下边是一个例子：

```cpp
#include <iostream>
#include <algorithm>
#include <functional>
#include <vector>
using namespace std;
class myclass
{
public:
    myclass(int a,int b):first(a),second(b){}
    int first;
    int second;
    bool operator < (const myclass &m)const
    {
        return first<m.first;
    }
};
bool less_second(const myclass &m1,const myclass &m2)
{
    return m1.second<m2.second;
}
int main()
{
    vector <myclass> vec;
    for(int i=0; i<10; i++)
    {
        myclass my(10-i,i*3);
        vec.push_back(my);
    }
    for(int i=0; i<vec.size(); i++)
        cout<<"("<<vec[i].first<<","<<vec[i].second<<")"<<"   ";
    cout<<endl;
    cout<<endl;
    sort(vec.begin(),vec.end());
    cout<<"第一次排序后："<<endl;
    cout<<endl;
    for(int i=0; i<vec.size(); i++)
        cout<<"("<<vec[i].first<<","<<vec[i].second<<")"<<"   ";
    cout<<endl;
    cout<<endl;
    sort(vec.begin(),vec.end(),less_second);
    cout<<"第二排序后："<<endl;
    cout<<endl;
    for(int i=0; i<vec.size(); i++)
        cout<<"("<<vec[i].first<<","<<vec[i].second<<")"<<"   ";
    cout<<endl;
    cout<<endl;
    return 0;
}

```

运行效果如下：

![](http://i.imgur.com/kfpwo5x.png)


**sort的稳定性**

你发现有sort和stable_sort还有partition和stable_partition。其中的区别是：带有stable的函数可以保证相等元素的原本相对次序在排序后保持不变。这里的相等是指你提供的函数表示两个元素相等，并不是一模一样的元素。

例如：
```
bool less_len(const string &str1,const string &str2)
{
    return str1.length()<str2.length();
}

```
此时“apple”和“winter”是相等的，如果“apple”出现在“winter”前边，用带stable的函数排序后，他们的次序一定不变。如果用不带stable的函数排序，那么排序之后，“winter”有可能在“apple”的前面。

**全排序**

全排列即把所给定范围所有元素按照大小关系顺序排列。

如以下函数：

```
void sort( first,  last); 

void sort( first,  last, camp);

void stable_sort( first, last); 

void stable_sort( first,  last, camp);

```

在第1,3种形式中，sort和stable_sort都没有指定比较函数，系统会默认使用operator < 对区间[first,last)内的所有元素进行排序。

第2,4种形式，你可以随意指定比较函数。

下边看一个具体例子吧！

班上有10个同学，我想知道他们的成绩排名：

```cpp
#include <iostream>
#include <algorithm>
#include <functional>
#include <vector>
#include <string>
using namespace std;
class student
{
public:
    student(const string &a,int b):name(a),score(b) {}
    string name;
    int score;
    bool operator < (const student &m) const
    {
        return score<m.score;
    }
};
int main()
{
    vector <student> vect;
    student st1("Tom", 74);
    vect.push_back(st1);
    st1.name="Jimy";
    st1.score=56;
    vect.push_back(st1);
    st1.name="Mary";
    st1.score=92;
    vect.push_back(st1);
    st1.name="Jessy";
    st1.score=85;
    vect.push_back(st1);
    st1.name="Jone";
    st1.score=56;
    vect.push_back(st1);
    st1.name="Bush";
    st1.score=52;
    vect.push_back(st1);
    st1.name="Winter";
    st1.score=77;
    vect.push_back(st1);
    st1.name="Andyer";
    st1.score=63;
    vect.push_back(st1);
    st1.name="Lily";
    st1.score=76;
    vect.push_back(st1);
    st1.name="Maryia";
    st1.score=89;
    vect.push_back(st1);
    cout<<"没排序之前"<<endl;
    for(int i=0; i<vect.size(); i++)
        cout<<vect[i].name<<":\t"<<vect[i].score<<endl;
    stable_sort(vect.begin(),vect.end(),less<student>());
    cout<<"排序之后"<<endl;
    for(int i=0; i<vect.size(); i++)
        cout<<vect[i].name<<":\t"<<vect[i].score<<endl;
    return 0;
}

```

我们看一下它的输出的结果：

![](http://i.imgur.com/6aEtk0E.png)

**局部排序**

局部排序其实是为了减少不必要的操作而提供的排序方式。其函数原型为：

```
void partial_sort( first , middle, last);

void partial_sort( first , middlel, last,camp);

```
理解了sort和stable_sort后，再理解partial_sort就比较容易了。

它的用途如：班上有10个学生，想知道分数最低的5名是谁。

那么我们将上边的程序做以下修改：

```
stable_sort(vect.begin(),vect.end(),less<student>());

改为

partial_sort(vect.begin(), vect.begin()+5, vect.end(),less<student>()); 

```

看一下结果如何：

![](http://i.imgur.com/fhQXpa7.png)

partial_sort采用的堆排序，它在任何情况下的时间复杂度都是 `n*log(n). `

**partition和stable_partition**

好像这两个函数并不是用来排序的。“分类”算法会更加贴切一点。

partition就是把一个区间的元素按照某个条件分成两类。其函数原型为：

```
template <class ForwardIterator, class Predicate> 
ForwardIterator partition(ForwardIterator first,ForwardIterator last, Predicate pred) 
template <class ForwardIterator, class Predicate> 
ForwardIterator stable_partition(ForwardIterator first, ForwardIterator last, Predicate pred); 

```

我们来看一个应用吧！

班上有10个学生，计算所有没有及格的学生。
我们将上边的代码做如下修改就好：

```
stable_sort(vect.begin(),vect.end(),less<student>());

改为

student exam("pass", 60);
stable_partition(vect.begin(), vect.end(), bind2nd(less<student>(), exam));

```

我们来看一下结果：

![](http://i.imgur.com/M7096PW.png)

**nth_element 指定元素排序**

nth_element使用时需要加algorithm头文件，作用为求第n大的元素，并把它放在第n位置上，可以保证n位置之前的数都比第n位置上的数小。n位置之后的数都比n位置上的数大。但是n位置前后并不是有序的。

如以下代码所示：

```
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
    int a[]={1,3,4,5,2,6,8,7,9};
    int i;
    cout<<"数列如下："<<endl;
    for(i=0;i<9;i++)
       cout<<a[i]<<" ";
       cout<<endl;
    //void nth_element (RandomAccessIterator first, RandomAccessIterator nth,RandomAccessIterator last);
    nth_element(a,a+5,a+9);
 
    cout<<endl<<"输出第五大的数： "<<a[4]<<endl; //注意下标是从0开始计数的
    return 0;
}
 
```

nth_element(l,k,r)
其中区间为左闭右开：[l,r)
k是实的
O(n)把第k小放到第k位，第k位之前都比它小，第k位之后都比它大
没有排序效果



