## [移除字母异位词后的结果数组](https://leetcode.cn/problems/find-resultant-array-after-removing-anagrams/)

~~~C++
class Solution {
public:
    bool check(string s1,string s2) {
        unordered_map<char,int> hs;
        for(auto x:s1) hs[x]++;
        for(auto x:s2) {
            if(hs.count(x)&&hs[x]!=0) {
                --hs[x];
            } else return false;
        }
        for(auto &[val,cnt]:hs) {
            if(cnt!=0) return false;
        }
        return true;
    }
    vector<string> removeAnagrams(vector<string>& words) {
        vector<string> ans;
        int n=words.size();
        ans.push_back(words[0]);
        for(int i=1;i<n;++i) {
            string s1=ans.back();
            string s2=words[i];
            if(!check(s1,s2)) ans.push_back(s2);
        }
        return ans;
    }
};
~~~

## [不含特殊楼层的最大连续楼层数](https://leetcode.cn/problems/maximum-consecutive-floors-without-special-floors/)

~~~C++
class Solution {
public:
    int maxConsecutive(int b, int t, vector<int>& s) {
        int n=s.size();
        sort(s.begin(),s.end());
        vector<int>a(n+1);
        a[0]=s[0]-b;
        for(int i=1;i<n;++i) {
            a[i]=s[i]-s[i-1]-1;
        }
        a[n]=t-s[n-1];
        return *max_element(a.begin(),a.end());
    }
};
~~~

## [按位与结果大于零的最长组合](https://leetcode.cn/problems/largest-combination-with-bitwise-and-greater-than-zero/)

~~~C++
class Solution {
public:
    int largestCombination(vector<int>& c) {
        int n=c.size();
        vector<vector<int>> a(n,vector<int>(25,0));
        for(int i=0;i<n;++i) {
            int count=1;
            while(c[i]) {
                a[i][count]=(c[i]&1);
                c[i]>>=1;
                ++count;
            }
        }
        int ans=0;
        for(int i=1;i<25;++i) {
            int count=0;
            for(int j=0;j<n;++j)
                if(a[j][i]==1) ++count;
            ans=max(ans,count);
        }
        return ans;
    }
};
~~~

## [统计区间中的整数数目](https://leetcode.cn/problems/count-integers-in-intervals/)

**thinking**

区间操作问题，我们可以采用珂朵莉树，和线段树的动态开点进行解决

[链接](https://leetcode.cn/problems/count-integers-in-intervals/solution/by-endlesscheng-clk2/)

**solution**

~~~C++
//1.珂朵莉树
class CountIntervals {
public:
    map<int,int> m;
    int cnt=0;
    CountIntervals() {}
    
    void add(int left, int right) {
        for(auto it=m.lower_bound(left);it!=m.end()&&it->second<=right;m.erase(it++)) {
            int l=it->second,r=it->first;
            left=min(l,left);
            right=max(right,r);
            cnt-=r-l+1;
        }
        cnt+=right-left+1;
        m[right]=left;
    }
    
    int count() {return cnt;}
};
//2.动态开点
class CountIntervals {
    CountIntervals *left = nullptr, *right = nullptr;
    int l, r, cnt = 0;

public:
    CountIntervals() : l(1), r(1e9) {}

    CountIntervals(int l, int r) : l(l), r(r) {}

    void add(int L, int R) { // 为方便区分变量名，将递归中始终不变的入参改为大写（视作常量）
        if (cnt == r - l + 1) return; // 当前节点已被完整覆盖，无需执行任何操作
        if (L <= l && r <= R) { // 当前节点已被区间 [L,R] 完整覆盖，不再继续递归
            cnt = r - l + 1;
            return;
        }
        int mid = (l + r) / 2;
        if (left == nullptr) left = new CountIntervals(l, mid); // 动态开点
        if (right == nullptr) right = new CountIntervals(mid + 1, r); // 动态开点
        if (L <= mid) left->add(L, R);
        if (mid < R) right->add(L, R);
        cnt = left->cnt + right->cnt;
    }

    int count() { return cnt; }
};
~~~

