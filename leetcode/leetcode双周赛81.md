## [统计星号](https://leetcode.cn/problems/count-asterisks/)

~~~C++
class Solution {
public:
    int countAsterisks(string s) {
        int n=s.size();
        vector<int> a;
        int count=0;
        for(int i=0;i<n;++i)
            if(s[i]=='|')
                a.push_back(i);
            else if(s[i]=='*')
                ++count;
        int ans=0;
        if(a.size()==0) return count;
        for(int i=0;i<a.size()-1;i+=2) {
            int left=a[i];
            int right=a[i+1];
            for(int j=left+1;j<right;++j) {
                if(s[j]=='*') ++ans;
            }
        }
        return count-ans;
    }
};
~~~

## [统计无向图中无法互相到达点对数](https://leetcode.cn/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/)

~~~C++
using ll=long long;
class UnionFind {
    public:
        vector<int> parent;//父辈节点
        vector<int> size;//当前的层数
        int n;
        int setCount;//当前的连通分量
    public:
        UnionFind(int _n):n(_n),setCount(_n),parent(_n),size(_n,1) {
            for(int i=0;i<n;++i) parent[i]=i;
        }

        int findset(int x) {return parent[x]==x?x:parent[x]=findset(parent[x]);}

        bool unite(int x,int y) {
            x=findset(x);y=findset(y);
            if(x==y) return false;
            if(size[x]<size[y]) swap(x,y);
            parent[y]=x;
            size[x]+=size[y];
            --setCount;
            return true;
        }

        bool connected(int x,int y) {return findset(y)==findset(x);}
};
class Solution {
public:
    long long countPairs(int n, vector<vector<int>>& edges) {
        UnionFind uf(n);
        if(edges.size()==0) return ll(n)*ll(n-1)/2;
        for(auto &e:edges)
            uf.unite(e[0],e[1]);
        vector<ll> b;
        map<int,int> hs;
        for(auto &x:uf.parent) x=uf.findset(x);
        for(auto &x:uf.parent) {
            hs[x]++;
        }
        for(auto &[val,cnt]:hs) {
            b.push_back(cnt);
        }
        ll ans=0;
        int len=b.size();
        for(int i=0;i<len;++i)
            for(int j=i+1;j<len;++j)
                ans+=b[i]*b[j];
        return ans;
    }
};
~~~

## [操作后的最大异或和](https://leetcode.cn/problems/maximum-xor-after-operations/)

~~~C++
class Solution {
public:
    int maximumXOR(vector<int>& nums) {
        int n=nums.size();
        vector<vector<int>> d(n,vector<int>(27,0));
        for(int i=0;i<n;++i) {
            int cnt=0;
            while(nums[i]) {
                d[i][cnt++]=(nums[i]&1);
                nums[i]>>=1;
            }    
        }
        vector<int> c(27);
        vector<bool> b(27,false);
        for(int j=0;j<27;++j) {
            int ji=0,ou=0;
            for(int i=0;i<n;++i) {
                if(d[i][j]==0)
                    ++ou;
                else
                    ++ji;
            }
            if(ji>0) b[j]=true;
            ji=ji%2==0?0:1;
            ou=0;
            c[j]=ji^ou;
        }
        for(int i=0;i<27;++i) {
            if(c[i]==0&&b[i])
                c[i]=1;
        }
        int ans=0;
        for(int i=0;i<27;++i) {
            if(c[i]==1)
                ans+=(1<<i);
        }
        return ans;
    }
};
~~~

## [不同骰子序列的数目](https://leetcode.cn/problems/number-of-distinct-roll-sequences/)

**thinking**

​       线性dp，我们考虑如何定义dp的状态，当比赛的时候，我定义的二维状态$f[i][j]$表示序列长度为i的长度，最后一个数字为j的时候的数目。但是因为隔开两个数字就可以重复，导致中间的状态转移方程出现问题。下面考虑三维的状态

​		$f[i][last][last2]$表示序列长度为i的时候，最后一个数字为last2,倒数第二个元素为last的序列数目。下面我们来考虑状态转移。

​		显然，我们要进行转移的方程是$f[k+1][i][j]$的数目，显然对于每一个$[i,j]$，我们只需要i前面的一个数$x$进行限制就行了，显然x不能和$i,j$相等，$x$不能和$i$冲突即可，累计相关的$f[k][x][i]$即可。

**solution**

~~~C++
const int N=1e4;
using ll=long long;
class Solution {
public:
    const int mod=1e9+7;
    ll f[N+1][7][7];
    //f[N][i][j] i表示倒数第二个数是i，j表示最后一个数是j
    int distinctSequences(int n) {
        if(n==1) return 6;
        memset(f,sizeof(f),0);
        unordered_map<int,vector<int>> hs;
        for(int i=1;i<=6;++i)
            for(int j=1;j<=6;++j)
                if(i!=j&&__gcd(i,j)==1) 
                    hs[i].push_back(j);
        //初始化预处理前两位的数字
        for(int i=1;i<=6;++i) {
            auto x=hs[i];
            for(auto xx:x)
                f[2][i][xx]=1;
        }
        //dp过程
        for(int k=3;k<=n;++k) {
            for(int i=1;i<=6;++i) {
                for(auto x:hs[i]) {
                    for(auto xx:hs[x])
                        if(xx!=i)
                            f[k][x][i]+=f[k-1][xx][x];
                    f[k][x][i]%=mod;
                }
            }
        }
        int ans=0;
        for(int i=1;i<=6;++i)
            for(int j=1;j<=6;++j)
                ans+=f[n][i][j],ans%=mod;
        return ans;
    }
};
~~~

