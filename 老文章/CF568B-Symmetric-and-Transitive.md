title: CF568B Symmetric and Transitive
tags:
  - 数学
  - DP
  - 计数dp
categories: []
date: 2018-09-18 18:43:00
---
#### 题目

有一个集合，它的元素是二元关系（x，y），其中x，y的范围是[1,n]。这个集合满足如下性质：
1.如果这个集合里有（x，y），那么一定有（y，x）。
2.如果这个集合里同时有（x，y）和（y，z），那么一定有（x，z）。注意，在这条性质里，x，y，z可以相等
3.这个集合**不**同时包含（1，1），（2，2），……（n，n）。
问这个集合有多少种组成方式。
（当n=3时，有10种方式，为∅, {(1,1)} , {(2,2)} , {(3,3)} , {(1,1),(2,2)} , {(1,1),(3,3)} , {(2,2),(3,3)} , {(1,2),(2,1),(1,1),(2,2)} , {(1,3),(3,1),(1,1),(3,3)} , {(2,3),(3,2),(2,2),(3,3)}）

<!--more-->

#### 思路

分析终态：

将整个集合看作一个无向图，把（a，b）看作是ab之间连了一条边，那么整个图就会被划分成若干个联通快，且不能包含一些点（至少一个），那么集合的数目就是把一些点剔除之后，集合划分的数目，这恰好就是**贝尔数**。

下面我们来推贝尔数的式子：

**贝尔数定义** ： 一个包含n个元素的集合，集合划分的数目，如n=1时，f(n)=1,为{1};

若已知了$f(n)$

那么$f(n+1)$=$\sum_{k=0}^n C_n^k f(n-k)$

**证明**：

考虑第n+1个元素，如果它和某一个元素被分为一类，那么为$C_n^n f(n)$

如果它和某两个元素被分为一类，那么为$C_n^{n-1} f(n-1)$。

以此类推。

有了此公式，我们就可以解此题了。

$ans=\sum_{i=1}^n C_n^i f(i)$

#### 代码
```c++
#include<bits/stdc++.h>
#define M 4005
#define MOD 1000000007
#define LL long long
using namespace std;
LL C[M][M];
LL bel[M];
void Init() {
    C[0][0]=1;
    for(int i=1; i<=M-5; i++) {
        C[i][0]=1;
        for(int j=1; j<=i; j++)
            C[i][j]=(C[i-1][j]+C[i-1][j-1])%MOD;
    }
    bel[0]=bel[1]=1;
    for(int i=2; i<=M-5; i++) {
        for(int j=0; j<i; j++)
            bel[i]+=C[i-1][j]*bel[j],bel[i]%=MOD;
    }
}
int n;
int main() {
    Init();
    cin>>n;
    LL ans=0;
    for(int i=0; i<n; i++) {
        ans+=C[n][i]*bel[i];
        ans%=MOD;
    }
    cout<<ans<<endl;
    return 0;
}
```