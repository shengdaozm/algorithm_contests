---
title: Leetcode周赛292
date: 2022-05-08 11:51:40
tags: Leetcode
categories: algorithm
mathjax: true
---

> 模拟，dp,剪枝

<!--more-->

## [字符串中最大的 3 位相同数字](https://leetcode-cn.com/problems/largest-3-same-digit-number-in-string/)

**thinking**

直接遍历即可

**solution**

~~~C++
class Solution {
public:
    bool check(string s) {
        if(s[0]==s[1]&&s[1]==s[2]) return true;
        return false;
    }
    string largestGoodInteger(string num) {
        string ans;
        int n=num.size();
        if(n<3) return ans;
        for(int i=0;i<=n-3;++i) {
            if(check(num.substr(i,3))) {
                if(ans!="") {
                    if(atoi(ans.c_str())<atoi(num.substr(i,3).c_str())) {
                        ans=num.substr(i,3);
                    }
                } else {
                    ans=num.substr(i,3);
                }
            }
        }
        return ans;
    }
};
~~~

## [统计值等于子树平均值的节点数](https://leetcode-cn.com/problems/count-nodes-equal-to-average-of-subtree/)

**thinking**

dfs依次遍历所有节点，进行计算即可。

**solution**

~~~C++
class Solution {
public:
    int ans=0;
    int count=0;
    int findnum(TreeNode* root) {
        if(root==nullptr) return 0;
        ++count;
        return findnum(root->left)+findnum(root->right)+root->val;
    }
    void dfs(TreeNode* root) {
        if(root==nullptr) return;
        count=0;
        if((findnum(root->left)+findnum(root->right)+root->val)/(count+1)==root->val) ++ans;
        dfs(root->left);
        dfs(root->right);
    }
    int averageOfSubtree(TreeNode* root) {
        ans=0;
        dfs(root);
        return ans;
    }
};
~~~

## [统计打字方案数](https://leetcode-cn.com/problems/count-number-of-texts/)

**thinking**

线性dp，我们定义$f[i][j]$记录每一次的状态转移，$f[i][j]$表示，取到第i个位置，且与前面的j个字符一块进行翻译的方案数。因此，我们右如下的状态转移方程
$$
if\quad s[i]!=s[i-1] \quad f[i][0]=\sum_{k=0}^{4}f[i-1][k]\\
if\quad s[i]=s[i-1] \quad f[i][0]=\sum_{k=0}^{4}f[i-1][k]\\
f[i][k]=f[i-1][k-1]\quad (k为这个字符对应能够打印的最多的字母数)
$$


**solution**

~~~C++
using ll=long long;
class Solution {
public:
    const int mod=1e9+7;
    int countTexts(string s) {
        unordered_map<char,int> a;
        a['2']=3;a['3']=3;a['4']=3;a['5']=3;a['6']=3;a['7']=4;a['8']=3;a['9']=4;
        int n=s.size();
        vector<vector<ll>> f(n,vector<ll>(4,0));
        f[0][0]=1;
        for(int i=1;i<n;++i) {
            if(s[i]==s[i-1]) {
                int maxnum=a[s[i]];
                for(int j=0;j<4;++j) {
                    f[i][0]+=f[i-1][j];
                    f[i][0]%=mod;
                }
                for(int j=1;j<maxnum;++j) {
                    f[i][j]+=f[i-1][j-1];
                    f[i][j]%=mod;
                }
            } else {
                //用不了前面的---true
                for(int j=0;j<4;++j) {
                    f[i][0]+=f[i-1][j];
                    f[i][0]%=mod;
                }
            }
        }
        int ans=0;
        for(int i=0;i<4;++i) {
            ans+=f[n-1][i];
            ans%=mod;
        }
        return ans;
    }
};

~~~

## [检查是否有合法括号字符串路径](https://leetcode-cn.com/problems/check-if-there-is-a-valid-parentheses-string-path/)

**thinking**

这道题的剪枝有点难以想象，单纯的dfs会超时。具体的剪枝思路如下：

我们定义状态$(x,y,c)$来表示dfs的过程，我们设(为1，)为-1，用c来表示，取到的括号的类型，那么题设的条件就转变成了遍历该条路径的时候，需要所有状态的c$\geq$0，且最终也为0.

我们再来看，这题有如下的特点：

* 一定是从$(0,0)\rightarrow(m-1,n-1)$，所以，我们走过的路径一定有$m+n-1$的长度，**注意**，由于题目要求路径的括号是匹配的，也就是说$m+n-1$必须是偶数
* 对于我们遍历的某一种状态来说，我们可以预判，进行剪枝：$(x,y,c)$处于当前状态，如果$c>m-x+n-y-1$的话，就可以剪掉了，因为后面的路径全是），也不能匹配了。
* 对于某一重复状态的访问，可以直接return false，因为如果当前状态一旦可以达到目标，dfs就结束了
* 对于状态的描述，因为路径有$m+n-1$的长度，而且一旦c小于0，就会剪掉，所以状态数可以压缩到$n+m-1$

**solution**

~~~C++
class Solution {
public:
    bool dfs(int x,int y,int c,vector<vector<char>>& g,vector<vector<vector<bool>>> &vis,int m,int n) {
        if(c>m-x+n-y-1) return false;
        if(x==m-1&&y==n-1) return c==1;
        if(vis[x][y][c]) return false;
        vis[x][y][c]=true;
        c+=g[x][y]=='('?1:-1;
        return c>=0&&((x+1<m&&dfs(x+1,y,c,g,vis,m,n))||(y+1<n&&dfs(x,y+1,c,g,vis,m,n)));
    }
    bool hasValidPath(vector<vector<char>>& g) {
        int m=g.size();
        int n=g[0].size();
        vector<vector<vector<bool>>> vis(m,vector<vector<bool>>(n,vector<bool>(n+m,false)));
        if((m+n)%2==0||g[0][0]==')'||g[m-1][n-1]=='(') return false;
        return dfs(0,0,0,g,vis,m,n);
    }
};
~~~

