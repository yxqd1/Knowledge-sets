# R进制转化（原题：Base6）

## 题目链接：

[Problem - I - Codeforces](https://codeforces.com/gym/411925/problem/I)

## 主要思想：

首先：将每一位转化成10进制作为过渡

其次：整除取余，从高位到低位，r=r*num+a[i],初始 r = 0



## 代码：

初始化字符数组转化：

```
map<char,int>mp1;
map<int,char>mp2;
void init(){
    string s="0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    for(int i=0;i<s.size();++i){
        mp1[s[i]]=i;
        mp2[i]=s[i];
    }
}
```

每位转化十进制：

```
//每位转化成十进制
vec cc(string s){
    vec aa;
    for(int i=s.size()-1;i>=0;--i){
        aa.emplace_back(mp1[s[i]]);
    }
    //vec是从低位到高位的
    return aa;
}
```

转化R进制并取余数：

```
vec div(vec a,int x,int y,int& mod) {
    vec aa;
    mod=0;
    for(int i=a.size()-1;i>=0;--i){//从高位到低位
        mod=mod*x+a[i];//转化为10进制
        aa.emplace_back(mod/y);
        mod%=y;//每次要余数，并进行值传递
    }
    reverse(aa.begin(),aa.end());
    while(aa.size()>1&&aa.back()==0)aa.pop_back();//去掉前导零
    return aa;
}
vec turn (vec a,int x,int y){
    //将x进制转化成y进制
    vec ans;
    if(a.back()==0)ans.emplace_back(0);
    while(a.back()!=0){
        int mod=0;
        a=div(a,x,y,mod);
        ans.emplace_back(mod);
    }
    return ans;
}
```

整体代码：

```
map<char,int>mp1;//注意用long long
map<int,char>mp2;
void init(){
    string s="0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    for(int i=0;i<s.size();++i){
        mp1[s[i]]=i;
        mp2[i]=s[i];
    }
}
//每位转化成十进制
vec cc(string s){
    vec aa;
    for(int i=s.size()-1;i>=0;--i){
        aa.emplace_back(mp1[s[i]]);
    }
    //vec是从低位到高位的
    return aa;
}
vec div(vec a,int x,int y,int& mod) {
    vec aa;
    mod=0;
    for(int i=a.size()-1;i>=0;--i){//从高位到低位
        mod=mod*x+a[i];//转化为10进制
        aa.emplace_back(mod/y);
        mod%=y;//每次要余数，并进行值传递
    }
    reverse(aa.begin(),aa.end());
    while(aa.size()>1&&aa.back()==0)aa.pop_back();//去掉前导零
    return aa;
}
vec turn (vec a,int x,int y){
    //将x进制转化成y进制
    vec ans;
    if(a.back()==0)ans.emplace_back(0);
    while(a.back()!=0){
        int mod=0;
        a=div(a,x,y,mod);
        ans.emplace_back(mod);
    }
    return ans;
}
void solve(){//R进制转化
   int x,y;cin>>x>>y;
   string s;cin>>s;
   init();
//    if(s=="0"){
//     cout<<0<<'\n';
//     return ;
//    }
   vec a=cc(s);
   vec ans=turn(a,x,y);
   string ss="";
   for(int i=ans.size()-1;i>=0;--i){
    //输出对应字符串
    cout<<mp2[ans[i]];
    // ss+=mp2[ans[i]];
   }
//    cout<<ss;
   cout<<'\n';
}   
```

