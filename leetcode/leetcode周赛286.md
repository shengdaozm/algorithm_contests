---
title: Leetcode周赛286
date: 2022-04-29 00:26:02
tags: Leetcode
comments: true
categories: algorithm
mathjax: true
---

> 哈希表，模拟，字符串，背包dp

<!--more-->

## [找出两数组的不同](https://leetcode-cn.com/problems/find-the-difference-of-two-arrays/)

~~~C++
class Solution {
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>>ans(2);
        unordered_set<int> a1,a2;
        for(auto &x:nums1) {
            a1.insert(x);
        }
        for(auto &x:nums2) {
            a2.insert(x);
        }
        for(auto &x:nums1) {
            if(!a2.count(x)) {
                a2.insert(x);
                ans[0].push_back(x);
            }
        }
        for(auto &x:nums2) {
            if(!a1.count(x)) {
                a1.insert(x);
                ans[1].push_back(x);
            }
        }
        return ans;
    }
};
~~~

## [美化数组的最少删除数](https://leetcode-cn.com/problems/minimum-deletions-to-make-array-beautiful/)

~~~C++
class Solution {
public:
    int minDeletion(vector<int>& nums) {
        int n=nums.size();
        int cnt=0;
        if(n==0) return cnt;
        if(n==1) return 1;
        //数组至少有2个
        stack<int> s;
        for(int i=0;i<n;++i) {
            if((s.size())%2==0) {
                 s.push(nums[i]);
            } else {
                if(!s.empty()&&s.top()!=nums[i]) s.push(nums[i]);
                else ++cnt;
            }
        }
        int len=s.size();
        if(len%2!=0) ++cnt;
        return cnt;
    }
};
~~~

## [找到指定长度的回文数](https://leetcode-cn.com/problems/find-palindrome-with-fixed-length/)

**thinking**

回文数的构造，我们仔细观察，发现：

* 长度为1的回文数，最小是1，最大是9
* 长度为n（大于2）的回文数：最小$10^{n-1}+1$，最大$10^n-1$,

拿3位数的回文数举例，101，111，121，131，141，…999我们不难发现，数字的前一半，是一次变大的，然后进行反折拼在一块就是第k个回文数了，因此，我们有如下的操作策略

ps：关于部分的代码细节，可以选用部分测试案例进行模拟查看

**solution**

~~~C++
class Solution {
public:
    vector<long long> kthPalindrome(vector<int>& q, int l) {
        vector<long long>ans;
        int k=(l+1)/2;
        int p=1;
        for(int i=0;i<k-1;++i) p*=10;
        for(auto x:q) {
            if(x>9*p) ans.push_back(-1);
            else {
                long long res=p+x-1;
                string s=to_string(res);
                s.resize(l-k);
                reverse(s.begin(),s.end());
                for(auto c:s) {
                    res=res*10+c-'0';
                }
                ans.push_back(res);
            }
        }
        return ans;   
    }
};
~~~

## [从栈中取出 K 个硬币的最大面值和](https://leetcode-cn.com/problems/maximum-value-of-k-coins-from-piles/)

**thinking**

背包的问题，我们将每一个栈中的前缀和计算出来，$for 第n个栈，sum[j]表示体积为j的，价值为sum[j]的物品$，我们将其当作我们需要处理的背包，下面就是01背包的经典模板

**solution**

~~~C++
class Solution {
public:
    int maxValueOfCoins(vector<vector<int>>& piles, int k) {
       vector<int> f(k+1,0);
       int sumn=0;
        for(auto &pile:piles) {
            int n=pile.size();
            for(int i=1;i<n;++i) pile[i]+=pile[i-1];
            sumn=min(sumn+n,k);
            for(int j=sumn;j>=0;--j) {
                for(int i=0;i<min(n,j);++i)
                    f[j]=max(f[j],f[j-i-1]+pile[i]);
            }
        }
       return f[k]; 
    }
};
~~~

~~~C++
//为什么这个不对啊
class Solution {
public:
    int maxValueOfCoins(vector<vector<int>>& piles, int k) {
       vector<int> f(k+1,0);
        for(auto &pile:piles) {
            int n=pile.size();
            for(int i=1;i<n;++i) pile[i]+=pile[i-1];
            for(int i=0;i<n;++i) {
                for(int j=k;j>i;--j)
                    f[j]=max(f[j],f[j-i-1]+pile[i]);
            }
        }
       return f[k]; 
    }
};
~~~
