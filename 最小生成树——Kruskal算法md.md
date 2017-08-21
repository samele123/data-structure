<font color=green>**算法背景**</font>

克鲁斯卡尔（Kruskal）算法是一种按权值的递增次序选择合适的边来构造最小生成树的方法。Kruskal算法是由Joseph Kruskal在1956年发表。用来解决同样问题的还有Prim算法和Boruvka算法等。三种算法都是贪婪算法的应用。和Boruvka算法不同的地方是，Kruskal算法在图中存在相同权值的边时也有效。

<font color=green>**算法过程**</font>

克鲁斯卡尔（Kruskal）算法是基于贪心的思想得到的。首先我们将各边的权值按从小到大排列，然后按顺序依次选取，如果构不成回路就合并，直到选够（n-1）条边为止。

下边图示Kruskal算法的实现过程

<dig align=center>
![这里写图片描述](http://img.blog.csdn.net/20170720101405946?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc0MTIyMjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<font color=green>**代码部分**</font>

```
typedef struct          
{        
    char vertex[VertexNum];                                //顶点表         
    int edges[VertexNum][VertexNum];                       //邻接矩阵,可看做边表         
    int n,e;                                               //图中当前的顶点数和边数         
}MGraph; 
 
typedef struct node  
{  
    int u;                                                 //边的起始顶点   
    int v;                                                 //边的终止顶点   
    int w;                                                 //边的权值   
}Edge; 

void kruskal(MGraph G)  
{  
    int i,j,u1,v1,sn1,sn2,k;  
    int vset[VertexNum];                                    //辅助数组，判定两个顶点是否连通   
    int E[EdgeNum];                                         //存放所有的边   
    k=0;                                                    //E数组的下标从0开始   
    for (i=0;i<G.n;i++)  
    {  
        for (j=0;j<G.n;j++)  
        {  
            if (G.edges[i][j]!=0 && G.edges[i][j]!=INF)  
            {  
                E[k].u=i;  
                E[k].v=j;  
                E[k].w=G.edges[i][j];  
                k++;  
            }  
        }  
    }     
    heapsort(E,k,sizeof(E[0]));                            //堆排序，按权值从小到大排列       
    for (i=0;i<G.n;i++)                                    //初始化辅助数组   
    {  
        vset[i]=i;  
    }  
    k=1;                                                   //生成的边数，最后要刚好为总边数   
    j=0;                                                   //E中的下标   
    while (k<G.n)  
    {   
        sn1=vset[E[j].u];  
        sn2=vset[E[j].v];                                  //得到两顶点属于的集合编号   
        if (sn1!=sn2)                                      //不在同一集合编号内的话，把边加入最小生成树   
        {
            printf("%d ---> %d, %d",E[j].u,E[j].v,E[j].w);       
            k++;  
            for (i=0;i<G.n;i++)  
            {  
                if (vset[i]==sn2)  
                {  
                    vset[i]=sn1;  
                }  
            }             
        }  
        j++;  
    }  
}  
```

该算法的时间复杂度为O（e^2）

Kruskal算法要将所有的顶点合并到同一个集合中，可以用<font color=green>并查集</font>进行改进，这样的效率会提高很高。用这种方法构造最小生成树的时间复杂度为O（elog2e）其中e是边数。

下边是代码

```
//n为边的数量，m为点的数量
int Kruskal(int n, int m)
{
    int nEdge = 0, res = 0;  //nEdge  表示如果加入集合的点
    sort(a, a+n, cmp);//将边按照权值从小到大排序
    for(int i = 0; i < n && nEdge != m - 1; i++)
    {
        //判断当前这条边的两个端点是否属于同一棵树
        if(find(a[i].a) != find(a[i].b))
        {
            //如果不属于同一棵树，就要合并
            unite(a[i].a, a[i].b);
            res += a[i].price;  //最小生成树的值加上a[i].price
            nEdge++;
        }
    }
    //如果加入边的数量小于m - 1,则表明该无向图不连通,等价于不存在最小生成树
    if(nEdge < m-1) res = -1;
    return res;
}
 
```
