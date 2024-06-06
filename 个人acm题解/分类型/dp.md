# dp

dp的一个特点就是书写递归方程。

dp其实强调对于集合的划分，策略的安排

## 一、整数分块+dp暴力

[Problem - B - Codeforces](https://codeforces.com/gym/104095/problem/B)

先开始要找到可以可以转移的人数

然后开两个数组，pre   和  now    ，表示一定体重下的最大价值，

最后暴力dp

![image-20230212134053435](C:\Users\30279\AppData\Roaming\Typora\typora-user-images\image-20230212134053435.png)

## 二、dp，trie树

[Problem - I - Codeforces](https://codeforces.com/gym/104081/problem/I)

### 思路：

trie树预处理子串，可以从1或者0开始，建议从1，end可以标记是否是完整串，cnt可以标记子串出现次数，也可以返回层数，或者长度，

dp[flag] [i]意思是：a或者b在i的位置所需要的最小值：

```
if(tt[j].end[p]){
     dp[j][k]=min(dp[!j][i-1]+1,dp[j][k]);
}
```

dfs会超时，dp不会，而且更清楚

### 注意：

trie树和dp的结合

## 三、dp三维背包，滚动数组

[Problem - E - Codeforces](https://codeforces.com/group/Aokqa6Haao/contest/427952/problem/E)

### 思路：

这个题是dp

开始只是暴力所有的情况，很好地利用容量这个问题，最后处理答案，

有的值的综合减去当前量的总和的一半才是补充的量，最后暴力选择的时候，别超当前容量

### 注意：

输入的方式和数组大小

## 四、dp，滚动数组，用multiset动态维护可行的位置更新

[Problem - F - Codeforces](https://codeforces.com/group/Aokqa6Haao/contest/427952/problem/F)

### 思路：

dp[i] [j]表示第i道主菜，第j个菜被用，花费最小

但是你需要动态的维护需要更新的节点，先删除不可以走的，然后更新dp，选最小的前边的那个，然后添加回之前的

需要multiset动态维护、记录更新

### 注意：

跟题目的下标含义要一致

## 五、dp，滚动数组，

[Problem - A - Codeforces](https://codeforces.com/group/Aokqa6Haao/contest/428602/problem/A)

（纯粹暴力很难，但是具备一定规律，这题开始误认为是组合数学，其实不是，因为规律在于过程和递推方程上）

### 思路：

开始，dp[i] [j] [p] [q]这么设状态，空间上不允许

之后，dp[2] [j] [p]这样用滚动数组优化一下，空间上是可以的，0表示之前的状态，1表示现在的状态，j表示列，p表示拥有0的个数，（总数为n+m-1，所以q也就知道了），整体表示情况数

### 注意：

初始化dp[1] [1] [k]的时候要注意a[1] [1]的情况

还有一个就是i==1&&j==1特判

a[i] [j]==0是，dp[1] [j] [0]=0;此时没有0的情况不成立

## 六、dp,汉诺塔问题，待理解

[Problem - F - Codeforces](https://codeforces.com/group/Aokqa6Haao/contest/428602/problem/F)

### 思路：

本身套用三汉诺塔问题的递推方程，ans表示答案，预处理，并且在处理过程中有一个类似最短路径的处理

## 七、dp背包，

[Dashboard - CUSTACM 2023 Training 1 - Codeforces](https://codeforces.com/group/Aokqa6Haao/contest/432112)

### 思路：

   但是有个区间的压缩，r[i]记录这个i的位置的最右区间，然后常规dp

  一维遍历前i的位置，二维遍历选几个区间，内部选和不选，

选的话 从i-1的位置进入，j-1的位置再选，

不选的话，从i-1的位置到现在的位置，每次选最大，最后dp【n】【k】即是最后的答案

## 八、dp，欧拉筛，线性组合，01背包变形

[Problem - D - Codeforces](https://codeforces.com/contest/1794/problem/D)

### 思路：

首先预处理fac（mod意义下的前缀积），facinv(前缀积的逆元),预处理欧拉筛

之后，dp[i] [j]表示前i种质数，选择j种的所有可以的情况的facinv

最后fac(n)* （所有非质数的全排列的逆元） %mod *dp[cnt] [n]%mod;表示在所有数中选n个不同的质数的全部情况

## 九、dp暴力或者贪心都行

[Problem - D - Codeforces](https://codeforces.com/contest/1809/problem/D)

### 思路

dp[i] [3]表示到i位置不同情况的集合

集合划分：

当前是1：

前边是1、0或者直接转移，或者删除当前



当前是0：

前边是1、0或者删除当前



最后取最小

## 十、多种方法，dp

[Problem - E - Codeforces](https://codeforces.com/gym/104252/problem/E)

### 思路：

开二维数组

状态表示：反向开设，dp[ i ] [ j ]指的是左边i和右边j块是否覆盖，

集合划分：暴力所有块数，不到O（n^3），如果len长度可以就给左边或者右边,维数一个变一个不变，相当于只用一次

```c++
for(int len=1;len<=n+1;++len){
	if(len==k)continue;
		for(int j=l;j>=0;--j){
			for(int z=r;z>=0;--z){
				if(len<=j)dp[j][z]|=dp[j-len][z];//维数一个变一个不变，相当于只用一次
				if(len<=z)dp[j][z]|=dp[j][z-len];
			}
		}
	}
```

## 十一、记忆化搜索，树上

[Problem - E - Codeforces](https://codeforces.com/contest/1806/problem/E)

### 思路：

因为空间开不了2个1e5，所以有个编号cnt，但是cnt《=320的时候才记录或者返回，（不太清楚

记忆化搜索：截止的地方--返回数组--深搜--回溯更新dp数组，这个320还没法处理掉，也就是说一定得推导出320这个边界。

## 十二、换根dp（树上的

[Problem - D - Codeforces](https://codeforces.com/contest/1805/problem/D)

### 思路：

 [动态规划.md](..\知识集\动态规划.md) 

## 十三、dp，自动取模

[Problem - G2 - Codeforces](https://codeforces.com/contest/1811/problem/G2)

### 思路：

题意：每一块同颜色，最大块数的前提下最多数量

策略：明显的是dp但是需要优化，sum记录每一块的最大数量，注意每块（同块内颜色一致）和每块边界的处理

（可以有不同的颜色，所以重新累加

状态:dp[i ] [j ]          i表示最后的位置（右边界），j表示颜色，        一维位置，一维颜色      问题的解决决定状态和划分

划分：分块，处理块间和块中。

注意：一定注意数组初始化，不初始化数组的话，会混乱掉的。

## 十四、线性dp

[P1280 尼克的任务 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1280)

### 思路：

题意：最长空闲时间的选择

策略：

dp[i]最大休闲时间，正着走会受到后续的影响，所以倒着走

用sum记录当前点开始的个数，

集合划分：

sum==0时，空闲时间+1

sum！=-时，num表示应该运行第几个任务了，然后将长空闲时间转移到当前位置。

## 十五、区间dp或者树形dp

[P1040 [NOIP2003 提高组\] 加分二叉树 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1040)

### 思路：

dp是用来处理可重叠问题的，并且具有处理过的状态不会再影响后续状态的特点

  **题意**：给出二叉树中序遍历的结果，个人感觉是在固定左右关系

  **策略**：区间dp，分堆类型，并且用root记录根节点，最后前序遍历，要学习这种路径的记载（状态更新的时候，记录更新后的节点，更新后的路径

```c++
void print(int l,int r){
    if(l>r)return ;
    cout<<root[l][r]<<' ';
    if(l==r)return ;
    print(l,root[l][r]-1);
    print(root[l][r]+1,r);
}
void solve(){
    //30个节点
    //题意：给出二叉树中序遍历的结果，个人感觉是在固定左右关系
    //策略：区间dp，分堆类型，并且用root记录根节点，最后前序遍历
    int n;cin>>n;
    for(int i=1;i<=n;++i){
        cin>>dp[i][i];
        dp[i][i-1]=1;//左子树应该默认为空，因为这个节点最大
        root[i][i]=i;
    }
    for(int len=1;len<=n;++len){
        for(int l=1;l+len<=n;++l){
            int r=l+len;
            for(int k=l;k<r;++k){
                if(dp[l][r]<dp[l][k-1]*dp[k+1][r]+dp[k][k]){
                    dp[l][r]=dp[l][k-1]*dp[k+1][r]+dp[k][k];
                    root[l][r]=k;
                }
            }
        }
    }
    cout<<dp[1][n]<<'\n';
    print(1,n);
}   
```

## 十六、线性dp

[P4933 大师 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P4933)

### 思路：

如何记载子序列不全为空且有次序可缺项的情况的总合，1 2 3  4  ans +1 +2 +3 +4           +1  +1  +1  +1

题意：子序列构成等差数列的情况的个数，

策略：线性dp，dp[i ] [ j ]表示以i为结尾，j为公差的情况数     ans记录总合，但是单个次序的记录的方法

```c++
int h[maxj];
int dp[1100][40010];//以i为结尾，以j为公差，总共的个数，重复子问题递推的求解。
void solve(){
    //n的个数支持立方的操作。
    int n;cin>>n;
    for(int i=1;i<=n;++i){
        cin>>h[i];
    }
    int ans=0;
    int p=20000;
    //转移，集合划分错了，排列组合，逐个求解可以表达所有可能,也就是说  +1 +2 +3 逐个加可以
    for(int i=1;i<=n;++i){
        ans++;//作为单个出现的时候
        for(int l=i-1;l>=1;--l){//01背包
            dp[i][h[i]-h[l]+p]+=dp[l][h[i]-h[l]+p]+1;//公差可能重复所以需要累加
            dp[i][h[i]-h[l]+p]%=mod;
            ans+=dp[l][h[i]-h[l]+p]+1;
            ans%=mod;
        }
    }
    cout<<ans<<'\n';
}  
```

## 十七、线性dp

[Problem - E - Codeforces](https://codeforces.com/group/QJA47ykHfD/contest/437692/problem/E)

**题意：** 每次只移动左手或者右手，使得成本最小

**策略：** 线性dp，

**集合表示：**dp[ flag ] [ i ] [ j ]  ，一维表示到哪里了，（因为只有i和i-1，所以可以压缩） ，i表示左手哪个位置，j表示右手那个位置。

**集合处理：**预处理距离，初始化dp数组，第一层暴力到那个音符，第二层初始化下个flag  and  左手走（左手和右手都从0到8暴力，并且左手产生新的贡献）  and  右手走（左手和右手都从0到8暴力，并且右手产生新的贡献）

**集合划分：** 左手和右手分别暴力转移。

因为手放的点数很少，所以可以作为一维，

## 十八、01背包

[G-Chevonne's Necklace_“统信杯” 第十七届黑龙江省大学生程序设计竞赛（正式赛）（重现赛）@yxh03 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/57225/G)

不太好想，对赌状态互不影响，对于状态方程而言，确实也是互不影响

然后用01背包去做，体积上的累加。

## 十九、换根dp（未完



## 二十、单调队列结合dp

[Problem - E - Codeforces](https://codeforces.com/contest/940/problem/E)

### 思路：

题意：给定意义下最小和

  策略：单调队列优化dp，维护单调递减队列

尽可能细分，尽可能多删，少加大的,多删一点

 开始c长度区间无法区别，之后c区间的每个可以进行选择

## 二十一、dp、拓扑序

https://codeforces.com/contest/1863/problem/E

## 二十二、dp、一个位置有一种情况

[Problem - D - Codeforces](https://codeforces.com/contest/1875/problem/D)

## 二十三、状压dp+记忆化搜索

[Problem - D - Codeforces](https://codeforces.com/gym/104023/problem/D)

## 二十四、dp

https://codeforces.com/gym/104022/problem/B

## 二十五、换根dp

https://codeforces.com/contest/1882/problem/D

## 二十六、换根dp

https://codeforces.com/gym/104008/problem/G

## 二十七、dp



[Problem - B - Codeforces](https://codeforces.com/gym/104128/problem/B)

## 二十八、树形dp

[Problem - D - Codeforces](https://codeforces.com/contest/1929/problem/D)

## 二十九、四维dp

[1212. 地宫取宝 - AcWing题库](https://www.acwing.com/problem/content/description/1214/)



```c++
int dp[N][N][N][N];// 四维dp，主要看影响因素和状态的转移
signed main(){
    int n,m,k;
    cin>>n>>m>>k;
    for(int i=1;i<=n;++i){
        for(int j=1;j<=m;++j){
            cin>>a[i][j];
            a[i][j]++;
        }
    }
    // 初始化，第一个元素拿或者不拿都有一种
    dp[1][1][0][0]=1;
    dp[1][1][1][a[1][1]]=1;
    // dp[i][j][cnt][k]在i、j位置，要了cnt个，然后重量是k，因为这个值受四个状态影响
    for(int i=1;i<=n;++i){
        for(int j=1;j<=m;++j){
            for(int cnt=0;cnt<=k;++cnt){
                for(int kk=0;kk<15;++kk){
                    // 这个位置不要，那么个数和值都可满足
                    dp[i][j][cnt][kk]=(dp[i][j][cnt][kk]+dp[i-1][j][cnt][kk])%MOD;
                    dp[i][j][cnt][kk]=(dp[i][j][cnt][kk]+dp[i][j-1][cnt][kk])%MOD;
                    if(cnt>0&&kk==a[i][j]){
                        // 多一个需要对第四维大小进行操作
                        for(int ss=0;ss<kk;++ss){ // 从0开始，要是没有东西咋办
                            dp[i][j][cnt][kk]=(dp[i][j][cnt][kk]+dp[i-1][j][cnt-1][ss])%MOD;
                            dp[i][j][cnt][kk]=(dp[i][j][cnt][kk]+dp[i][j-1][cnt-1][ss])%MOD;
                        }
                    }
                }
            }
        }
    }
    // for(int i=)
    int ans=0;
    // 累加所有可能最大重量
    for(int i=1;i<=15;++i){
        ans=(ans+dp[n][m][k][i])%MOD;
        // cout<<dp[n][m][k][i]<<'\n';
    }
    cout<<ans<<'\n';
    return 0;
}
```

## 三十、单调队列优化dp

维护区间最值问题

问题难点：区间长度为d必须有一次跳跃，没办法逐一维护区间的每一个点

在可行范围内维护最小值，队列记录下标，队列原数组有序从小到大

注意下标和变量设置

[Problem - F - Codeforces](https://codeforces.com/contest/1941/problem/F)

## 三十一、线性dp，在两行中选择，有数量控制

问题在于：审错题了，名次在m以内就行，然后可以随便使用操作

[Problem - D - Codeforces](https://codeforces.com/contest/1945/problem/D)

```c++
void solve()
{
	// 里边有个规则，可以任意使用操作包括次数
	// a的可以随便取，b的前边需要有个a
	int n,m;cin>>n>>m;
	for(int i=1;i<=n;++i){
		cin>>a[i];
	} 
	for(int i=1;i<=n;++i){
		cin>>b[i];
	}
	// 注意审题和问题的转换
	// 从后往前，只要前边有个a后边可以随便选
	int sum=0;
	int ans=inf;
	for(int i=n;i>=1;--i){
		if(i<=m){
			ans=min(ans,sum+a[i]);
		}
		sum+=min(a[i],b[i]);
	} 
	cout<<ans<<"\n";
}
```

## 三十二、单调队列优化dp，二分

[Problem - D - Codeforces](https://codeforces.com/contest/1918/problem/D)

## 三十三、状压dp

[3494. 国际象棋 - AcWing题库](https://www.acwing.com/problem/content/description/3497/)

如何计算二进制中1的个数

```c++
int get_count(int x){
    int ans=0;
    while(x){
        ans++;
        x-=x&-x;
    }
    return ans;
}
```



```c++
// 对可以状压dp的一维进行状压dp，那些状态影响就要考虑几维
int dp[N][M][M][K];
int get_count(int x){
    int ans=0;
    while(x){
        ans++;
        x-=x&-x;
    }
    return ans;
}
signed main(){
    int n,m,k;
    scanf("%lld %lld %lld",&n,&m,&k);
    // cout<<get_count(3)<<'\n';
    for(int i=1;i<=m;++i){
        // 状态是三列之间的转移，列行的概念不绝对
        // 从深到浅 b a c
        dp[0][0][0][0]=1;
        for(int a=0;a<(1<<n);++a){
            for(int b=0;b<(1<<n);++b){
                // 有就不行所以没有&1
                
                if(((a<<2)&b)||((b<<2)&a))continue;
                for(int c=0;c<(1<<n);++c){
                    if(((a<<2)&c)||((c<<2)&a))continue;
                    // 这三层相互之间都有影响
                    if(((b<<1)&c)||((c<<1&b)))continue;
                    // 从i-1转移到i
                    int t=get_count(b);
                    for(int kk=t;kk<=k;++kk){
                        // 更新kk数量，代表上一层转移到当前层
                        dp[i][a][b][kk]=(dp[i][a][b][kk]+dp[i-1][c][a][kk-t])%MOD;
                    }
                }
            }
        }
    }    
    int ans=0;
    for(int i=0;i<(1<<n);++i){
        for(int j=0;j<(1<<n);++j){
            // 我的第一维是列
            ans=(ans+dp[m][i][j][k])%MOD;
        }
    }
    cout<<ans<<'\n';
    return 0;
}
```

## 三十四、状压dp，构造题，mex（待处理

[Problem - D - Codeforces](https://codeforces.com/contest/1956/problem/D)

## 三十五、树形dp

[Problem - C - Codeforces](https://codeforces.com/group/n9YFSztryg/contest/520588/problem/C)

## 三十六、线性dp+组合数

[Problem - B - Codeforces](https://codeforces.com/group/n9YFSztryg/contest/523445/problem/B)

一方面要限制轮数，一方面要限制每一轮的操作数
