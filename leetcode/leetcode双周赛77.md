---
title: leetcode双周赛77
date: 2022-04-30 22:57:42
tags: Leetcode
categories: algorithm
---

> 模拟，前缀和，bfs

<!--more-->

## [统计是给定字符串前缀的字符串数目](https://leetcode-cn.com/contest/biweekly-contest-77/problems/count-prefixes-of-a-given-string/)

**thinking**

直接查就行

**solution**

~~~C++
class Solution {
public:
    int countPrefixes(vector<string>& w, string s) {
        int cnt=0;
        for(auto &f:w) {
            bool ok=true;
            if(f.size()>s.size()) continue;
            for(int i=0;i<f.size()&&i<s.size();++i) {
                if(f[i]!=s[i]) ok=false;
            }
            if(ok) ++cnt;
        }
        return cnt;
    }
};
~~~

## [最小平均差](https://leetcode-cn.com/contest/biweekly-contest-77/problems/minimum-average-difference/)

**thinking**

前缀和处理一下即可，注意最后一次的计算除数为0.

**solution**

~~~C++
class Solution {
public:
    int minimumAverageDifference(vector<int>& nums) {
        int n=nums.size();
        vector<long long> sum(n+1,0);
        for(int i=1;i<=n;++i) sum[i]=sum[i-1]+nums[i-1];
        vector<int> cnt(n);
        for(int i=0;i<n;++i) {
            if(i!=n-1)
            cnt[i]=abs(sum[i+1]/(i+1)-(sum[n]-sum[i+1])/(n-i-1));
            else cnt[i]=abs(sum[i+1]/(i+1)-0);
        }
        //for(auto x:cnt) cout<<x<<" ";
        int minnum=0x3f3f3f3f,pos=0;
        for(int i=0;i<n;++i) {
            if(cnt[i]<minnum) {
                minnum=cnt[i];
                pos=i;
            }
        }
        return pos;
    }
};
~~~

##  [统计网格图中没有被保卫的格子数](https://leetcode-cn.com/contest/biweekly-contest-77/problems/count-unguarded-cells-in-the-grid/)

**thinking**

数据量太小，直接暴力即可

**solution**

~~~C++
class Solution {
public:
    int dx[4]={-1,1,0,0},dy[4]={0,0,1,-1};
    int countUnguarded(int m, int n, vector<vector<int>>& g, vector<vector<int>>& w) {
        int ans=m*n;
        vector<vector<int>> a(m,vector<int>(n,0));
        for(auto &x:w) a[x[0]][x[1]]=2;
        for(auto &x:g) a[x[0]][x[1]]=2;
        for(auto &pos:g) {
            int x=pos[0],y=pos[1];
            for(int i=0;i<4;++i) {
                int xx=x+dx[i];
                int yy=y+dy[i];
                while(xx<m&&xx>=0&&yy<n&&yy>=0&&a[xx][yy]!=2) {
                    a[xx][yy]=1;
                    xx+=dx[i];
                    yy+=dy[i];
                }
            }
        }
        for(int i=0;i<m;++i)
            for(int j=0;j<n;++j)
                if(a[i][j]!=0) --ans;
        return ans;
    }
};
~~~

## [逃离火灾](https://leetcode-cn.com/contest/biweekly-contest-77/problems/escape-the-spreading-fire/)

**thinking**

[参考题解](https://leetcode-cn.com/problems/escape-the-spreading-fire/solution/c-by-muriyatensei-cjq4/)，我们采用多次bfs进行求解，计算出人与火源到达安全屋的最近距离即可。在进行具体的分类讨论。

**solution**

~~~C++
class Solution {
public:
    static constexpr int dir[4][2] = {1, 0, 0, 1, -1, 0, 0, -1};

    int maximumMinutes(vector<vector<int>> &grid) {
        int m = grid.size(), n = grid.front().size();
        //以终点为源的一次BFS, 计算出 （最近的）火距离(ex, ey)，比人远多少
        
        auto solve = [&](int ex, int ey) {
            if (grid[ex][ey] == 2) return -1;
            int people = 0x3f3f3f3f, fire = 0x3f3f3f3f;
            vector<vector<int>> vis(m, vector<int>(n));
            deque<pair<int, int>> q{{ex, ey}};
            vis[ex][ey] = 1;
            int sz, des = 0;
            while ((sz = q.size())) {
                ++des;
                while (sz--) {
                    auto [x, y] = q.front();
                    q.pop_front();
                    if(fire == 0x3f3f3f3f && grid[x][y] == 1) fire = des;
                    if(people == 0x3f3f3f3f && x == 0 && y == 0) people = des;
                    for(auto[xx, yy]: dir){
                        int nx = x + xx, ny = y + yy;
                        if(nx < 0 || nx >= m || ny < 0 || ny >= n || grid[nx][ny] == 2 || vis[nx][ny] == 1)
                            continue;
                        vis[nx][ny] = 1;
                        q.emplace_back(nx, ny);
                    }
                }
                if(fire != 0x3f3f3f3f && people != 0x3f3f3f3f)
                    break;
            }
            if(people == 0x3f3f3f3f) return -1;
            if(fire == 0x3f3f3f3f) return 1000000000;
            return fire - people;
        };

        int ans = solve(m - 1, n - 1);//粗算时间
        if(ans == -1 || ans == 1000000000) return ans;
        int tg = max(solve(m - 2, n - 1), solve(m - 1, n - 2));//精算相等是否可行，即 提前出发判断
        if (ans == tg) --ans;//需要少等1min
        if(ans < 0) return -1;
        return ans;
    }
};
~~~

