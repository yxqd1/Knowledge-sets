# Lca

## 知识：

基本概述：在树上求解最近公共祖先



### 方法一：

向上标记法：O(n)很少用

### 方法二：倍增

倍增：O(n*log(n))，在线

二进制拼凑：可以从高位到低位枚举二进制从而拼凑出任何一个数

#### 首先预处理：

bfs(防止爆栈)或者dfs(求siz)

fa[i,j]表示从i节点开始，向上走2^j步，所能走到的节点，0<=j<=logn

dep[i]表示深度

##### 代码：

bfs

```c++
void bfs(int root)
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[root] = 1;
    int hh = 0, tt = 0;
    q[0] = root;
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;
                for (int k = 1; k <= 15; k ++ )
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}
```

dfs

```c++
//lca是这类问题的核心，注意哨兵的作用 ,把1令成根 
void dfs(int u,int faa,int de){
	siz[u]=1;
	dep[u]=de;
	fa[u][0]=faa;
	for(int j=1;j<=20;++j){
		fa[u][j]=fa[fa[u][j-1]][j-1];
	}
	for(int i=0;i<g[u].size();++i){
		int v=g[u][i];
		if(v==faa)continue;
		dfs(v,u,de+1);
		siz[u]+=siz[v];
	}
}
```

#### 步骤：

1）首先，让u成为深的一层，让两个点成为同一层

2）让两个点同时往上跳，一直跳到它们最近公共祖先的下一层，返回fa[u] [0];

##### 代码：

```c++
int lca(int a, int b)
{
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 15; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
    if (a == b) return a;
    for (int k = 15; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}
```



### 方法三：

 Tarjan:离线算法，线性O(n+m)

运用并查集

1）存入询问

2）搜索暴力

3）结果在于 正在搜索的 对应于 已经搜索过的点  的合并的点

分为三大类：

1、已经遍历过且回溯过的点，2

2、正在搜索的，1

3、还未搜索的，0

## 例题：

### 祖孙询问（方法二



[1172. 祖孙询问 - AcWing题库](https://www.acwing.com/problem/content/1174/)

板子题：

#### 代码：

```c++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 40010, M = N * 2;

int n, m;
int h[N], e[M], ne[M], idx;
int depth[N], fa[N][16];
int q[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void bfs(int root)
{
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0, depth[root] = 1;
    int hh = 0, tt = 0;
    q[0] = root;
    while (hh <= tt)
    {
        int t = q[hh ++ ];
        for (int i = h[t]; ~i; i = ne[i])
        {
            int j = e[i];
            if (depth[j] > depth[t] + 1)
            {
                depth[j] = depth[t] + 1;
                q[ ++ tt] = j;
                fa[j][0] = t;
                for (int k = 1; k <= 15; k ++ )
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}

int lca(int a, int b)
{
    if (depth[a] < depth[b]) swap(a, b);
    for (int k = 15; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
    if (a == b) return a;
    for (int k = 15; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}

int main()
{
    scanf("%d", &n);
    int root = 0;
    memset(h, -1, sizeof h);

    for (int i = 0; i < n; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        if (b == -1) root = a;
        else add(a, b), add(b, a);
    }

    bfs(root);

    scanf("%d", &m);
    while (m -- )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        int p = lca(a, b);
        if (p == a) puts("1");
        else if (p == b) puts("2");
        else puts("0");
    }

    return 0;
}
```

###  距离（方法三

[1171. 距离 - AcWing题库](https://www.acwing.com/problem/content/1173/)

两点的距离：两点到根节点的距离-2*lca到根节点的距离

#### 代码：

```c++
 #include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef pair<int, int> PII;

const int N = 10010, M = N * 2;

int n, m;
int h[N], e[M], w[M], ne[M], idx;
int dist[N];
int p[N];
int res[M];
int st[N];
vector<PII> query[N];   // first存查询的另外一个点，second存查询编号

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u, int fa)
{
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        if (j == fa) continue;
        dist[j] = dist[u] + w[i];
        dfs(j, u);
    }
}

int find(int x)
{
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

void tarjan(int u)
{
    st[u] = 1;
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];
        //没有搜索和进行搜索回溯合并
        if (!st[j])
        {
            tarjan(j);
            p[j] = u;
        }
    }

    for (auto item : query[u])
    {
        int y = item.first, id = item.second;
        if (st[y] == 2)
        {
            int anc = find(y);
            res[id] = dist[u] + dist[y] - dist[anc] * 2;
        }
    }

    st[u] = 2;
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c), add(b, a, c);
    }
//预处理，存入询问
    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        scanf("%d%d", &a, &b);
        if (a != b)
        {
            query[a].push_back({b, i});
            query[b].push_back({a, i});
        }
    }
//并查集初始化
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    dfs(1, -1);
    tarjan(1);

    for (int i = 0; i < m; i ++ ) printf("%d\n", res[i]);

    return 0;
}
```

### Alexandria Library

向上跳dep

```c++
for(int i=0;i<=20;++i){
	if((dep>>i)&1)x=fa[x][i];
}
return x;
```



#### 代码： 

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define rl rt<<1
#define rr rt<<1|1
// #pragma GCC optimize(2)
// #pragma GCC optimize(3,"ofast","inline")
// #define ull unsigned long long
typedef long long ll;
const int maxj=3e5+100,P=131,mod=998244353,inf=0x3f3f3f3f;
#define rep(i,m,n) for(int i=m;i<=n;++i)
#define pii pair<int,int>
template <class t>
void read(t &res)
{
    char c;
    t flag = 1;
    while ((c = getchar()) < '0' || c > '9')
        if (c == '-')
            flag = -1;
    res = c - '0';
    while ((c = getchar()) <= '9' && c >= '0')
        res = res * 10 + c - '0';
    res *= flag;
}
int n,q;
vector<int>g[maxj];
int siz[maxj],dep[maxj];
int fa[maxj][22];
//lca是这类问题的核心，注意哨兵的作用 ,把1令成根 
void dfs(int u,int faa,int de){
	siz[u]=1;
	dep[u]=de;
	fa[u][0]=faa;
	for(int j=1;j<=20;++j){
		fa[u][j]=fa[fa[u][j-1]][j-1];
	}
	for(int i=0;i<g[u].size();++i){
		int v=g[u][i];
		if(v==faa)continue;
		dfs(v,u,de+1);
		siz[u]+=siz[v];
	}
}
//跳跃到的位置 ,表达跳多高 ,发生差值的跳跃 
int up(int x,int de){
	for(int i=0;i<=20;++i){
		if((de>>i)&1)x=fa[x][i];
	} 
	return x;
}
int query(int u,int v){
	//让y的深度尽可能小
	if(dep[u]<dep[v])swap(u,v);
	int xx=u,yy=v;
	//需要一个记载他跳跃的步数,具不具备路径相同的点 
	int sep=0;
	bool flag=0; 
	//跳到同一深度
	for(int i=20;i>=0;--i){
		
		if(dep[fa[u][i]]>=dep[v]){
			flag=1;
			sep-=dep[fa[u][i]]-dep[u];
			u=fa[u][i];
		}
	} 
	//单只树上的情况 
	if(u==v){
		if(sep%2)return 0;
		int f1=up(xx,sep/2);
		int f2=up(xx,sep/2-1);
		return siz[f1]-siz[f2];
	}
	//寻找lca算法 
	for(int i=20;i>=0;--i){
		if(fa[u][i]!=fa[v][i]){
			//两个分支都在跳 
			sep=sep+(-dep[fa[u][i]]+dep[u])-(dep[fa[v][i]]-dep[v]);
			u=fa[u][i];
			v=fa[v][i];
		}
	}
	//少算了lca点
	sep+=2;
	if(sep%2)return 0;
	//开始的时候都在同一层次 ,减去树的两个分支 
	if(flag==0){
	 	int f1=up(xx,sep/2-1);
		int f2=up(yy,sep/2-1);
		return n-siz[f1]-siz[f2];
	}
	int f1=up(xx,sep/2);
	int f2=up(xx,sep/2-1);
	return siz[f1]-siz[f2];
}
void solve(){
	cin>>n>>q;
	for(int i=1;i<=n;++i)g[i].clear();
	for(int i=1;i<=n-1;++i){
		int u,v;cin>>u>>v;
		g[u].push_back(v);
		g[v].push_back(u);
	} 
	dfs(1,0,1);
	while(q--){
		int u,v;
		cin>>u>>v;
		if(u==v){
			cout<<n<<'\n';
		}else cout<<query(u,v)<<'\n';
	}
}   
signed main(){
    freopen("library.in", "r", stdin);
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t;t=1;
    cin>>t;
    while(t--){
        solve();
    }
    return 0;
}
```

