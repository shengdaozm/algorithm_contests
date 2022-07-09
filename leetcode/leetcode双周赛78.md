> 模拟，前缀和，二分，

## [找到一个数字的 K 美丽值](https://leetcode.cn/problems/find-the-k-beauty-of-a-number/)

~~~C++
class Solution {
public:
    int getnum(string s,int k) {
        int ans=0;
        for(auto x:s) {
            ans+=(x-'0')*pow(10,--k);
        }
        return ans;
    }
    int divisorSubstrings(int num, int k) {
        int ans=0;
        string s=to_string(num);
        int n=s.size();
        for(int i=0;i<=n-k;++i) {
            if(getnum(s.substr(i,k),k)==0) continue;
            if(num%getnum(s.substr(i,k),k)==0) ++ans;
        }
        return ans;
    }
};
~~~

## [分割数组的方案数](https://leetcode.cn/problems/number-of-ways-to-split-array/)

~~~C++
using ll=long long;
class Solution {
public:
    int waysToSplitArray(vector<int>& nums) {
        int n=nums.size();
        vector<ll>sum(n+1);
        for(int i=1;i<=n;++i) sum[i]=sum[i-1]+nums[i-1];
        int ans=0;
        for(int i=0;i<n-1;++i)
            if(sum[i+1]>=sum[n]-sum[i+1]) 
                ++ans;
        return ans;
    }
};
~~~

## [毯子覆盖的最多白色砖块数](https://leetcode.cn/problems/maximum-white-tiles-covered-by-a-carpet/)

**thinking**



**solution**

~~~C++
class Solution {
public:
    int maximumWhiteTiles(vector<vector<int>>& t, int c) {
        sort(t.begin(),t.end(),[](auto& t1,auto& t2){
            return t1[0]<t2[0];
        });
        int ans=0,left=0,len=0;
        for(auto &tt:t) {
            int l=tt[0],r=tt[1];
            len+=r-l+1;
            while(t[left][1]<r-c+1) {
                len-=t[left][1]-t[left][0]+1;
                left++;
            }
            ans=max(ans,len-max(r-c+1-t[left][0],0));
        }
        return ans;
    }
};
~~~

## [最大波动的子字符串](https://leetcode.cn/problems/substring-with-largest-variance/)

**thinking**



**solution**

~~~C++
class Solution {
public:
    int largestVariance(string &s) {
        int ans=0;
        for(char a='a';a<='z';++a)
            for(char b='a';b<='z';++b) {
                if(a!=b) {
                    int cnt=0,count=-s.size();
                    for(char ch:s) {
                        if(ch==a) ++cnt,++count;
                        else if(ch==b) count=--cnt,cnt=max(cnt,0);
                        ans=max(count,ans);
                    }
                }
            }
        return ans;
    }
};
~~~

