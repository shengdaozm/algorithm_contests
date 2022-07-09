## [兼具大小写的最好英文字母](https://leetcode.cn/problems/greatest-english-letter-in-upper-and-lower-case/)

~~~C++
class Solution {
public:
    string greatestLetter(string s) {
        vector<int> xiao(26,-1);
        vector<int> big(26,-1);
        int n=s.size();
        for(int i=0;i<n;++i) {
            if(s[i]<='z'&&s[i]>='a') xiao[s[i]-'a']=i;
            else big[s[i]-'A']=i;
        }
        int cnt=-1;
        for(int i=0;i<26;++i) {
            if(xiao[i]!=-1&&big[i]!=-1) {
                cnt=i;
            }
        }
        if(cnt==-1) return "";
        return s.substr(big[cnt],1);
    }
};
~~~

## [个位数字为 K 的整数之和](https://leetcode.cn/problems/sum-of-numbers-with-units-digit-k/)

~~~C++
class Solution {
public:
    int minimumNumbers(int num, int k) {
        if(num==0) return 0;
        unordered_map<int,int> hs;
        for(int i=1;i<=10;++i) {
            string s=to_string(k*i);
            if(!hs.count(s.back()-'0'))
            hs[s.back()-'0']=i;
        }
        string s=to_string(num);
        int temp=s.back()-'0';
        if(!hs.count(temp)) return -1;
        int cnt=hs[temp];
        if(k*cnt>num) return -1;
        return cnt;
    }
};
~~~

## [小于等于 K 的最长二进制子序列](https://leetcode.cn/problems/longest-binary-subsequence-less-than-or-equal-to-k/)

~~~C++
class Solution {
public:
    int longestSubsequence(string s, int k) {
        int n = s.length(), m = 32 - __builtin_clz(k);
        if (n < m) return n;
        int ans = stoi(s.substr(n - m), nullptr, 2) <= k ? m : m - 1;
        return ans + count(s.begin(), s.end() - m, '0');
    }
};
~~~

## [卖木头块](https://leetcode.cn/problems/selling-pieces-of-wood/)

**thinking**

线性dp，我们定义$f[i][j]$表示一块长宽为$(i,j)$的木块可以卖到的最大值。根据题目的要求，我们可以得到有以下几种情况：
$$
f[i][j]=prices[i][j]\\
f[i][j]=max(f[i][j],f[i][k]+f[i][j-k])\\
f[i][j]=max(f[i][j],f[k][j]+f[i-k][j])
$$
因此，我们可以直接得到如下的代码

**solution**

~~~C++
class Solution {
public:
    long long sellingWood(int m, int n, vector<vector<int>>& pp) {
        int pr[m+1][n+1];
        memset(pr,0,sizeof(pr));
        for(auto &p:pp) pr[p[0]][p[1]]=p[2];
        long long f[m+1][n+1];
        for(int i=1;i<=m;++i)
            for(int j=1;j<=n;++j) {
                f[i][j]=pr[i][j];
                for(int k=1;k<j;++k)
                    f[i][j]=max(f[i][j],f[i][k]+f[i][j-k]);
                for(int k=1;k<i;++k)
                    f[i][j]=max(f[i][j],f[k][j]+f[i-k][j]);
            }
        return f[m][n];
    }
};
~~~

