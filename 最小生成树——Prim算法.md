### **最小生成树**
##### &emsp;&emsp;给定一个无向带权图，顶点数是n，要使图连通只需（n-1）条边，若这（n-1）条边的权值和最小，则称这n个顶点和（n-1）条边构成了图的最小生成树。

### **Prime算法**

#### **算法背景**
##### &emsp;&emsp;**普里姆算法**（Prim算法）是一种构造性算法。该算法于1930年有捷克数学家沃伊捷赫.亚尔尼克发现，并在1957年由美国计算机科学家罗伯特.普里姆独立发现，1959年，艾兹格·迪科斯彻再次发现了该算法。因此，在某些场合下，普里姆算法又被称为DJP算法、亚尔尼克算法或普里姆-亚尔尼克算法。

#### **算法过程**

##### &emsp;&emsp;Prim算法的大致思想：假设图G顶点集合为U，首先任选一点a作为起始点，加入集合V,再从集合U-V中找到另一点b使得b到V中任意一点的权值最小，把b加入集合V。以此类推，现在集合V={a，b}，再从集合U-V找到一点c使得c到V中任意一点的权值最小，将c加入集合V，此时就构建出一棵最小生成树。

##### 为了便于在集合U和U-V之间选择权值最小的边，建立两个数组closest和lowcost。

##### lowcost[i]：表示以i为终点的边的最小权值，当lowcost[i]=0说明i点加入了最小生成树。

##### closest[i]:表示对应lowcost[i]的起点，即说明边（closest[i],i）是最小生成树的一条边，当closest[i]=0时说明起点i加入了最小生成树。

![这里写图片描述](http://img.blog.csdn.net/20170719164723710?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc0MTIyMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


##### 图中是用Prim算法构建最小生成树的过程。

##### 下图是构建最小生成树过程中lowcost数组的变化

![这里写图片描述](http://img.blog.csdn.net/20170719165320404?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc0MTIyMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
##### 我们以0为初始点，进行初始化（！代表无穷大，即无通路）

##### lowset[1]=6，<font color=red>lowcost[2]=1</font>，lowcost[3]=5，lowcost[4]=!，lowcost[5]=！
##### closest[1]=0，closest[2]=0，closest[3]=0，closest[4]=0，closest[5]=0（所有点默认起点是0）

##### 权值最小的点是2，将点2加入集合V，对起始点0进行标记
##### lowcost[1]=5，<font color=blue>lowcost[2]=0</font>，lowcost[3]=5，lowcost[4]=6，<font color=red>lowcost[5]=4</font>

##### closest[1]=2，<font color=blue>closest[2]=0</font>，closest[3]=0，closest[4]=2，closest[5]=2
##### 权值最小的点是5，将点5加入集合V，对点2进行标记

##### lowcost[1]=5，<font color=blue>lowcost[2]=0</font>，<font color=red>lowcost[3]=2</font>，lowsest[4]=6，<font color=blue>lowcost[5]=0</font>

##### closest[1]=2，<font color=blue>closest[2]=0</font>，closest[3]=5，closest[4]=2，<font color=blue>closest[5]=0</font>
##### 权值最小的是点3，将点3加入集合V，对点5进行标记

##### <font color=red>lowcost[1]=5</font>，<font color=blue>lowcost[2]=0</font>，<font color=blue>lowcost[3]=0</font>，lowsest[4]=6，<font color=blue>lowcost[5]=0</font>

##### closest[1]=2，<font color=blue>closest[2]=0</font>，<font color=blue>closest[3]=0</font>，closest[4]=2，<font color=blue>closest[5]=0</font>
##### 权值最小的点是点1，将点1加入集合V，对点3进行标记

##### <font color=blue>lowcost[1]=0</font>，<font color=blue>lowcost[2]=0</font>，<font color=blue>lowcost[3]=0</font>，<font color=red>lowcost[4]=3</font>，<font color=blue>lowcost[5]=0</font>
##### <font color=blue>closest[1]=0</font>，<font color=blue>closest[2]=0</font>，<font color=blue>closest[3]=0</font>，closest[4]=1，<font color=blue>closest[5]=0</font>
##### 权值最小的点是点4，将点4加入集合V，将点1进行标记

##### 这时一颗最小生成树就构建完成了。

#### **算法代码**

```C++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <string.h>
#define MAX 100
#define MAXCOST 0x7fffffff
using namespace std;
int graph[MAX][MAX];

int Prim(int graph[][MAX],int n)
{
    int lowcost[MAX];
    int closest[MAX];
    int i,j,min,minid,sum=0;
    for(i=2; i<=n; i++)
    {
        lowcost[i]=graph[1][i];
        closest[i]=1;
    }
    closest[1]=0;
    for(i=2; i<=n; i++)
    {
        min=MAXCOST;
        minid=0;
        for(j=2; j<=n; j++)
        {
            if(lowcost[j]<min&&lowcost[j]!=0)
            {
                min=lowcost[j];
                minid=j;
            }
        }
        sum+=min;
        lowcost[minid]=0;
        for(j=2; j<=n; j++)
        {
            if(graph[minid][j]<lowcost[j])
            {
                lowcost[j]=graph[minid][j];
                closest[j]=minid;
            }
        }
    }
    return sum;
}
int main()
{
    int i,j,k,m,n;
    int x,y,cost;
    cin>>m>>n;///顶点个数和边的个数
    ///初始化图G
    for(i=1; i<=m; i++)
    {
        for(j=1; j<=m; j++)
            graph[i][j]=MAXCOST;
    }
    ///构建图G
    for(k=1; k<=n; k++)
    {
        cin>>i>>j>>cost;
        graph[i][j]=cost;
        graph[j][i]=cost;
    }
    ///求解最小生成树
    cost=Prim(graph,m);
    cout<<"最小权值和="<<cost<<endl;
    return 0;
}

```
>##### 输入

>6 10  
1 2 6  
1 3 1  
1 4 5  
2 3 5  
2 5 3  
3 4 5  
3 5 6  
3 6 4  
4 6 2  
5 6 6

&emsp;
>##### 输出

>最小权值和=15  