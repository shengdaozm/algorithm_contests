## [强密码检验器 II](https://leetcode.cn/problems/strong-password-checker-ii/)

~~~C++
class Solution {
public:
    bool strongPasswordCheckerII(string &s) {
        int n=s.size();
        if(n<8) return false;
        bool a=false;bool b=false;bool c=false;bool d=false;
        unordered_set<char>hs;
        hs.insert('!');hs.insert('@');hs.insert('+');
        hs.insert('#');hs.insert('$');hs.insert('%');
        hs.insert('^');hs.insert('&');hs.insert('*');
        hs.insert('(');hs.insert(')');hs.insert('-');
        for(auto &x:s) {
            if(x<='z'&&x>='a') a=true;
            if(x<='Z'&&x>='A') b=true;
            if(x<='9'&&x>='0') c=true;
            if(hs.count(x)) d=true;
        }
        if(!(a&&b&&c&&d)) return false;
        for(int i=0;i<n-1;++i) 
            if(s[i]==s[i+1])
                return false;
        return true;
    }
};
~~~

## [咒语和药水的成功对数](https://leetcode.cn/problems/successful-pairs-of-spells-and-potions/)

~~~C++
using ll=long long; 
class Solution {
public:
    vector<int> successfulPairs(vector<int>& a, vector<int>& b, long long c) {
        int n=a.size();
        int m=b.size();
        sort(b.begin(),b.end());
        vector<int> ans(n);
        for(int i=0;i<n;++i) {
            ll num=(c/a[i])+(c%a[i]==0?0:1);
            int pos=lower_bound(b.begin(),b.end(),num)-b.begin();
            ans[i]=(pos==n?0:m-pos);
        }
        return ans;
    }
};
~~~

##   [替换字符后匹配](https://leetcode.cn/problems/match-substring-after-replacement/)

**thinking**



**solution**



~~~C++

~~~

## [统计得分小于 K 的子数组数目](https://leetcode.cn/problems/count-subarrays-with-score-less-than-k/)

~~~C++

~~~

