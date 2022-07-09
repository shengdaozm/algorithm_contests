## [较大分组的位置](https://leetcode.cn/problems/positions-of-large-groups/)

~~~C++
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string s) {
        vector<vector<int>> ans;
        int n=s.size();
        s+='1';
        for(int i=0,j=0;i<n;++i) {
             if(s[i]!=s[i+1]) {
                 if(i-j+1>=3) ans.push_back({j,i});
                 j=i+1;
             }
        }
        return ans;
    }
};
~~~

## [隐藏个人信息](https://leetcode.cn/problems/masking-personal-information/)

~~~C++
class Solution {
public:
    bool check(string &s) {
        for(auto &x:s) {
            if(x=='@') return true;
        }
        return false;
    }

    string upa1(string &s) {
        int pos=0;
        while(s[pos]!='@') ++pos;
        if(s[0]<='Z'&&s[0]>='A') s[0]+=32;
        if(s[pos-1]<='Z'&&s[pos-1]>='A') s[pos-1]+=32;
        for(int i=pos+1;i<s.size();++i) {
            if(s[i]<='Z'&&s[i]>='A'&&s[i]!='.') s[i]+=32;
        }
        return s.substr(0,1)+"*****"+s.substr(pos-1,1)+s.substr(pos,s.size()-pos);
    }

    string upa2(string &s) {
        int cnt=0;
        for(int i=0;i<s.size();++i) {
            if(s[i]<='9'&&s[i]>='0') ++cnt;
        }
        string ans;
        if(cnt-10==0) {
            ans="***-***-XXXX";
        } else if(cnt-10==1) {
            ans="+*-***-***-XXXX";
        } else if(cnt-10==2) {
            ans="+**-***-***-XXXX";
        } else {
            ans="+***-***-***-XXXX";
        }
        cnt=4;
        for(int i=ans.size()-1,j=s.size()-1;cnt>0;) {
            if(s[j]<='9'&&s[j]>='0') {
                ans[i]=s[j];
                --i;--j;
                --cnt;
            } else {
                --j;
            }
        }
        return ans;
    }

    string maskPII(string s) {
        if(check(s)) return upa1(s);
        return upa2(s);
    }
};
~~~

## [连续整数求和](https://leetcode.cn/problems/consecutive-numbers-sum/)

**thinking**

因为要求是连续的数字序列求和，我们有如下的求解方式：
$$
我们假设有X个数字，分别为a,a+1,a+2,...a+X-1相加等于n，求和可以得到ax+\frac{x*(x-1)}{2}=n\\
所以，我们枚举x，只要a存在时，计数即可。
$$
需要注意的是，我们不可能一个一个枚举x，我们考虑对于n来说，最大的可能x是多少，我们可以初略的估计$\frac{(1+n)*n}{2}\geq n$即可得到x，我们采用二分找到这个x即可

**solution**

~~~C++
using ll=long long;
class Solution {
public:
    int getnum(int n) {
        ll left=1,right=n;
        while(left<right) {
            ll mid=(left+right)/2;
            ll cnt=(mid+1)*mid/2;
            if(cnt==n) return mid;
            else if(cnt<n) {
                left=mid+1;
            } else {
                right=mid-1;
            }
        }
        return (int)left;
    }
    int consecutiveNumbersSum(int n) {
        int ans=0;
        int maxnum=getnum(n);
        for(int x=1;x<=maxnum;++x) {
            int num=n-(x*x-x)/2;
            if(num>0&&num%x==0) ++ans;
        }
        return ans;
    }
};
~~~

## [统计子串中的唯一字符](https://leetcode.cn/problems/count-unique-characters-of-all-substrings-of-a-given-string/)

**thinking**

我们反过来思考，我们要查找每个字串只出现一次的字母的个数，反过来，对于每一个字母，我们只需要找到它只出现一次的子串就可以了。因此，我们有如下方案：

我们预处理字符串中每一个字母的位置，遍历每一个字母，

#######a####a，比如这个我们只需要统计每一个a（比如第一个a），向左向右延长多少时，它仍然保证子串中只有一个字母，设左边有$left$个字母，右边有$right$个字母，那么 当前位置的字母对应的字串的总数就是$(left+1)*(right+1)$，此部分请读者自行推导。

就此，我们解决了这个题目。

**solution**

~~~C++
using ll=long long;
class Solution {
public:
    const int mod=1e9+7;
    int uniqueLetterString(string s) {
        unordered_map<char,vector<int>> vis;
        int n=s.size();
        for(int i=0;i<n;++i) vis[s[i]].push_back(i);
        //查找单个字母出现的字串有多少
        ll ans=0;
        for(auto &[val,pos]:vis) {
            for(auto x:pos) {
                ll left=0,right=0;
                int i=x-1,j=x+1;
                //left
                while(i>=0&&s[i]!=val) --i,++left;
                while(j<n&&s[j]!=val) ++j,++right;
                ans+=(left+1)*(right+1);
                ans%=mod;
            }
        }
        return ans%mod;
    }
};
~~~

