---
title: 2022leetcode春季战队赛
date: 2022-04-30 20:37:29
tags: Leetcode
categories: algorithm
mathjax: false 
---

> 模拟，01-bfs，

<!--more-->

## [采集果实](https://leetcode-cn.com/contest/season/2022-spring/problems/PTXy4P/)

**thinking**

直接模拟就行

**solution**

~~~C++
class Solution {
public:
    int getMinimumTime(vector& t, vector>& fruits, int m) {
        int ans=0;
        for(auto &f:fruits) {
            ans+=(f[1]+m-1)/m*t[f[0]];
        }
        return ans;
    }
};
~~~


## [信物传送](https://leetcode-cn.com/contest/season/2022-spring/problems/6UEx57/)

**thinking**

一开始拿到题目是非常懵的，我们考虑搜索或者dp进行操作。下面我们介绍非常经典的做法：01-bfs

01-bfs采用双端队列进行维护，与普通的bfs不同，我们维护的是走到某一个点的状态，维护队列的单调性，这样，当我们碰到了终点，那么我们使用的魔法次数一定是最少的。

* 我们遍历一个格子，如果与前一个格子方向相同，则入队首。不同，将魔法值加1，放在队尾。
* 取出队首元素，继续遍历。

对于队列维护的单调性，我们可以知道，我们取出的队首元素，对该元素执行+1或者+0的操作，明显，每次我们并不会破坏队列的单调性。

**solution**

~~~C++
class Solution {
public:
    char index[4]={'^','v','<','>'};
    int dx[4]={-1,1,0,0},dy[4]={0,0,-1,1};
    int conveyorBelt(vector<string>& matrix, vector<int>& s, vector<int>& t) {
        int m=matrix.size();
        int n=matrix[0].size();
        vector<vector<bool>> vis(m,vector<bool>(n,false));
        vector<vector<int>> ans(m,vector<int>(n,-1));
        deque<pair<int,int>>q;
        q.push_back(make_pair(s[0],s[1]));
        ans[s[0]][s[1]]=0;
        while(!q.empty()) {
            auto pre=q.front();
            q.pop_front();
            int x=pre.first;
            int y=pre.second;
            if(x==t[0]&&y==t[1]) return ans[x][y];
            if(vis[x][y]) continue;
            vis[x][y]=true;
            for(int i=0;i<4;++i) {
                int xx=x+dx[i];
                int yy=y+dy[i];
                if(xx<0||xx>=m||yy<0||yy>=n||vis[xx][yy]) continue;
                else {
                    if(matrix[x][y]==index[i]) {
                        q.push_front(make_pair(xx,yy));
                        ans[xx][yy]=ans[x][y];
                    } else {
                        q.push_back(make_pair(xx,yy));
                        ans[xx][yy]=ans[xx][yy]==-1?ans[x][y]+1:min(ans[xx][yy],ans[x][y]+1);
                    }
                }
            }
        }
        return -5;
    }
};
~~~

## [打地鼠](https://leetcode-cn.com/contest/season/2022-spring/problems/ZbAuEH/)

**thinking**



**solution**

~~~C++
~~~



