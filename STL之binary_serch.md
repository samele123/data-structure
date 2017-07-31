## STL之binary_search

### **概括**

大家都知道，二分查找是在排序后的基础上来对其进行查找操作。所以在使用bianry_search的时候，需要将要查找的容器进行排序。

### **说明**

在这里说明两种，一种是对结构体的二分操作，一种是对数组进行二分操作，在操作之前，肯定是需要对结构体进行排序，如何排序，这里就用到了STL里面很实用的sort函数。

**特别强调：排序的必须是从小到大**

### vector

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
vector <int> V;
int main()
{
    V.push_back(2);
    V.push_back(3);
    V.push_back(1);
    sort(V.begin(), V.end());//对vector 的排序
    if(binary_search(V.begin(), V.end(), 2)) // 对vector进行二分查找
        cout << "Yes" << endl;
    else
        cout << "No" << endl;
    return 0;
}
 
 
```

### struct

对结构体进行二分操作之前，肯定需要先对其排序。对结构体排序需要用到运算符重载，对结构体中的<进行重载，排序可以直接用sort排序，或者是定义一个结构体类型的set，set会对其去重排序，然后就可以使用二分查找了。

```cpp
struct Node//结构体定义以及运算符重载
{
    int x,y;
    bool operator<(const Node &rhs)const
    {
        if(x == rhs.x)
            return y < rhs.y;
        else
            return x < rhs.x;
    }
};
//二分查找函数
if(binary_search(nodes,nodes+n,tmp))
    cout << "Yes" << endl;
else
    cout << "No" << endl;
 
```