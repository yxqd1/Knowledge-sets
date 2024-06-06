# stl的运用

## 一、模拟，暴力，贪心，set，

[Problem - J - Codeforces](https://codeforces.com/group/Aokqa6Haao/contest/425631/problem/J)

   set如果对结构体使用，set<node>se,node里边最好重载<号和赋值方式，要不然erase使用有点问题，而且set有se.lower_bound(node(a,b))->a;的使用



## 二、多路归并，优先队列

[1378. 谦虚数字 - AcWing题库](https://www.acwing.com/problem/content/description/1380/)

每个质因子都有一组，每次都从其中找到最小的，遍历n次找到，第n小的数

代码：

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
const int N=2e5+100;
// 多路归并，每次都将最小的数进行合并，使用堆进行优化
struct node{
	// 值 那一组 这组的第几个 
	int v,p,id;
	bool operator < (const node& a) const {
		return v>a.v;
	}
}; 
signed main(){
    int k,n;
    cin>>k>>n;
    priority_queue<node>que;
    for(int i=1,a;i<=k;++i){
        cin>>a;
        que.push({a,a,0});
    }
    vector<int>q(1,1);
//    q.push_back(1);
    while(q.size()<=n){
    	// 找到最小的数累乘
		node now = que.top();
		que.pop();
		// 当前的值就是当前小的数 
		q.push_back(now.v);
//		cout<<now.v<<'\n';
		// id指针向前移动，记录新值,新值采用的是原始值 
		que.push({now.p*q[now.id+1],now.p,now.id+1});
		// 有可能有重复的值所以要做简单的重复处理 
		while(que.top().v==now.v){
			node noww=que.top();
			que.pop();
			que.push({noww.p*q[noww.id+1],noww.p,noww.id+1});
		} 
	}
	cout<<q.back()<<'\n';
    return 0;
}
```

## 三、优先队列，模拟

[Problem - H - Codeforces](https://codeforces.com/group/n9YFSztryg/contest/523445/problem/H)

结构体排序

```c++
struct node
{
    int l,r;
    bool operator <(const node &a)const
    {
        return r>a.r;
    }
};
priority_queue<node> q;
/*
sort默认为从小到大排序，优先队列默认为从大到小。

*/
```

