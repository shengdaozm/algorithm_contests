## [重排字符形成目标字符串](https://leetcode.cn/problems/rearrange-characters-to-make-target-string/)

~~~C++
class Solution {
public:
    int rearrangeCharacters(string s, string t) {
        int n=s.size();
        int len=t.size();
        unordered_map<char,int>hs;
        for(auto x:s) {
            hs[x]++;
        }
        int ans=0;
        while(1) {
            bool ok=true;
            for(auto x:t) {
                if(hs.count(x)) {
                    --hs[x];
                    if(hs[x]==0) hs.erase(x);
                } else {
                    ok=false;
                }
            }
            if(ok) ++ans;
            else return ans;
        }
        return 0;
    }
};
~~~

## [价格减免](https://leetcode.cn/problems/apply-discount-to-prices/)

~~~C++
using ll=long int;
class Solution {
public:
    string change(string s,int d) {
        if(s[0]=='$') {
            bool ok=false;
            for(int i=1;i<s.size();++i) {
                if((s[i]<='9'&&s[i]>='0')||s[i]=='.') {
                    ok=true;
                } else {
                    ok=false;
                    break;
                }
            }
            if(ok) {
                ll num=strtol(s.substr(1,s.size()-1).c_str(),nullptr,0);
                string ans="$";
                double temp=(double)num*((100-d)/double(100));
                string a=to_string(temp);
                int len=a.size();
                int pos;
                for(int i=0;i<len;++i){
                    if(a[i]=='.'){
                        pos=i;break;
                    }
                }
                if(a[pos+3]>='5') {
                    if(a[pos+2]!='9') a[pos+2]++;
                    else {
                        a[pos+1]++;
                        a[pos+2]='0';
                    }
                }
                return ans+a.substr(0,pos+3);
            }else {
                return s;
            }
        }
        return s;
    }
    string discountPrices(string s, int d) {
        s+=" ";
        int n=s.size();
        string ans;
        for(int i=0,j=0;i<n;++i) {
            if(s[i]==' ') {
                ans+=change(s.substr(j,i-j),d)+" ";
                j=i+1;
            }
        }
        n=ans.size();
        return ans.substr(0,n-1);
    }
};
~~~

## [使数组按非递减顺序排列](https://leetcode.cn/problems/steps-to-make-array-non-decreasing/)

**thinking**

再比赛期间，考虑过单调栈的做法，但是无奈被t2的字符串卡了太久了，我们考虑用单调栈的做法进行求解

对于一个x来说，它终究会被它左边的比它大的y所删除。可以很明显的看出y$\dots$x之间可能是完全的递减序列，也可能不是，这样y把x删除的时间，我们就无从得知。因此，我们考虑用单调栈来维护一个严格递减的序列，在一个严格递减的序列当中，我们可以很简单地得到我们需要删除的时间。

* 在每一个递减的序列中，其序列元素删除的时间应该都是相同的。
* 当下一个元素破坏掉当前得单调递减的序列之后，我们依照原则删除栈顶的元素，在删除的过程中，记录保存的最大的时间，如果栈内仍然非空，说明这一轮删除后，位于下一轮的删除位置上，下一轮删除，将当前元素删除即可，因此将时间加一，放入栈中。

**solution**

~~~C++
class Solution {
public:
    int totalSteps(vector<int>& nums) {
        int n=nums.size();int ans=0;
        stack<pair<int,int>> s;
        for(auto &x:nums) {
            int maxT=0;
            while(!s.empty()&&s.top().first<=x) {
                maxT=max(maxT,s.top().second);
                s.pop();
            }
            if(!s.empty()) ++maxT;
            ans=max(ans,maxT);
            s.emplace(x,maxT);
        }
        return ans;
    }
};
~~~

## [到达角落需要移除障碍物的最小数目](https://leetcode.cn/problems/minimum-obstacle-removal-to-reach-corner/)

~~~C++
class Solution {
public:
    int dx[4]={-1,1,0,0},dy[4]={0,0,1,-1};
    int minimumObstacles(vector<vector<int>>& grid) {
        int n=grid.size(),m=grid[0].size();
        const int inf=m*n;
        vector<vector<int>> a(n,vector<int>(m,inf));
        vector<vector<int>> vis(n,vector<int>(m,0));
        queue<pair<int,int>> q;
        if(grid[0][0]==1) a[0][0]=1;
        else a[0][0]=0;
        q.push(make_pair(0,0));
        vis[0][0]=2;
        while(!q.empty()) {
            int len=q.size();
            for(int i=0;i<len;++i) {
                   auto p=q.front();
            q.pop();
            int x=p.first;
            int y=p.second;
            for(int i=0;i<4;++i) {
                int xx=x+dx[i];
                int yy=y+dy[i];
                if(xx<n&&xx>=0&&yy<m&&yy>=0&&a[xx][yy]>a[x][y]+int(grid[xx][yy]==1)) {
                    int cnt=int(grid[xx][yy]==1);
                    a[xx][yy]=a[x][y]+cnt;
                    vis[xx][yy]++;
                    q.push(make_pair(xx,yy));
                }
            }
            }
        }
        return a[n-1][m-1];
    }
};
~~~

