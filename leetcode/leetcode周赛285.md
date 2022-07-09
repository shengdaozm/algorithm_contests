---
title: Leetcode周赛285
date: 2022-04-29 00:31:44
tags: Leetcode
categories: algorithm
mathjax: true
---

> 模拟，深度优先搜索、线段树

<!--more-->

## [统计数组中峰和谷的数量](https://leetcode-cn.com/problems/count-hills-and-valleys-in-an-array/)

**thinking**

题目很形象，但是我们操作起来略微麻烦——我们需要判断山峰山谷的个数（存在两个数都属于二者的情况），因此，对于每一个元素来说（第一个和最后一个当然不用考虑），我们有一个完美的解决方案：

我们对于遍历的每一个元素，它下标为i的$left=i-1$与$right=i+1$，再不越界的情况下，寻找左右第一个不想等的元素即可，如果查找到两端了，return，否则计数加一，将i移动到right的位置即可。

考虑到数据量的问题，我们可以直接忽略掉重复遍历的问题

**solution**

~~~C++
class Solution {
public:
    int countHillValley(vector<int>& nums) {
        int n=nums.size();
        int cnt=0;
        for(int i=1;i<n-1;++i) {
            int left=i-1,right=i+1;
            while(left>=0&&nums[left]==nums[i]) --left;//找到左边第一个不相等的
            while(right<n&&nums[right]==nums[i]) ++right;//找到右边第一个不相等的
            if(left==-1||right==n) continue;
            if(nums[left]<nums[i]&&nums[right]<nums[i]) ++cnt;
            if(nums[left]>nums[i]&&nums[right]>nums[i]) ++cnt;
            i=right-1;//循环结束后，i++还会执行
        }
        return cnt;
    }
};
~~~

## [统计道路上的碰撞次数](https://leetcode-cn.com/problems/count-collisions-on-a-road/)

**thinking**

朴素的想法，建立两两元素之间的哈希表关系，一次遍历每两个元素即可。

但是需要注意的是，如果两辆车发生了碰撞，则状态更新成静止，否则保持状态。但是，由于右边的突发情况对我们的前置判断有影响，比如：RRRRRL，这中情况相邻车是都会发生碰撞的，但是我们的操作方法并不能解决这一问题。因此，我们选择重复左右各遍历一次解决问题。

**solution**

~~~~C++
class Solution {
public:
    int countCollisions(string d) {
        unordered_map<string,int>hash;
        hash["LL"]=0;hash["LS"]=0;hash["LR"]=0;
        hash["RR"]=0;hash["RL"]=2;hash["RS"]=1;
        hash["SS"]=0;hash["SL"]=1;hash["SR"]=0;
        int n=d.size();
        int ans=0;
        for(int i=0;i<n-1;++i) {
            string a="aa";
            a[0]=d[i],a[1]=d[i+1];
            ans+=hash[a];
            if(hash[a]!=0) {
                d[i]='S',d[i+1]='S';
            }
        }
        for(int i=0;i<n-1;++i) {
            string a="aa";
            a[0]=d[i],a[1]=d[i+1];
            ans+=hash[a];
            if(hash[a]!=0) {
                d[i]='S',d[i+1]='S';
            }
        }
        for(int i=n-1;i>0;--i) {
            string a="aa";
            a[1]=d[i],a[0]=d[i-1];
            ans+=hash[a];
            if(hash[a]!=0) {
                d[i]='S',d[i-1]='S';
            }
        }
        return ans;
    }
};
~~~~

## [射箭比赛中的最大得分](https://leetcode-cn.com/problems/maximum-points-in-an-archery-competition/)

**thinking**

背包背包背包，仔细思考，太像01背包了，但是，01背包怎么求具体方案分匹配，不会写。

看一眼数据量，12，dfs一下，$2^{12}$，可以接受。代码如下，注意回溯一下。

**solution**

~~~C++
class Solution {
public:
    vector<int> ans;
    int maxsc=0;
    void dfs(int suma,int sumb,int n,int pos,vector<int>&a,vector<int>&b) {
        //递归结束条件
        if(pos==0) {
            if(sumb>maxsc) {
                maxsc=sumb;
                b[pos]=n;
                ans=b;
            }
            return;
        }
        if(n>a[pos]) {//选
            b[pos]=a[pos]+1;
            dfs(suma,sumb+pos,n-b[pos],pos-1,a,b);
        }
        //回溯操作
        b[pos]=0;
        //不选
        dfs(suma,sumb,n,pos-1,a,b);
    }
    vector<int> maximumBobPoints(int n, vector<int>& a) {
        int suma=0;
        for(int i=1;i<12;++i) {
            suma+=a[i]==0?0:i;
        }
        vector<int>b(12,0);
        dfs(suma,0,n,11,a,b);
        return ans;
    }
};
~~~

## [由单个字符重复的最长子字符串](https://leetcode-cn.com/problems/longest-substring-of-one-repeating-character/)

**thinking**

考察线段树，已经超出能力范围，待补

**solution**

~~~C++
~~~
