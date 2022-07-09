## [判断一个数的数字计数是否等于数位的值](https://leetcode.cn/problems/check-if-number-has-equal-digit-count-and-digit-value/)

~~~C++
class Solution {
public:
    bool digitCount(string num) {
        map<char,int> hs;
        for(int i=48;i<58;++i) hs[char(i)]=0;
        int n=num.size();
        for(auto x:num) hs[x]++;
        for(int i=48;i<n+48;++i) {
            if(hs[char(i)]!=num[i-48]-'0') return false;
        }
        return true;
    }
};
~~~

## [最多单词数的发件人](https://leetcode.cn/problems/sender-with-largest-word-count/)

~~~C++
class Solution {
public:
    string largestWordCount(vector<string>& m, vector<string>& s) {
        map<string,int>hs;
        int n=m.size();
        for(int i=0;i<n;++i) {
            m[i]+=" ";
            int cnt=0;
            int len=m[i].size();
            for(int j=0;j<len;++j) {
                if(m[i][j]==' ') ++cnt;
            }
            hs[s[i]]+=cnt;
        }
        int cnt=0;string ans;
        for(auto &[name,val]:hs) {
            if(val>=cnt) {
                cnt=val;
                ans=name;
            }
        }
        return ans;
    }
};
~~~

## [道路的最大总重要性](https://leetcode.cn/problems/maximum-total-importance-of-roads/)

~~~C++

using ll=long long;
class Solution {
public:
    long long maximumImportance(int n, vector<vector<int>>& r) {
        ll ans=0;
        map<int,int> hs;
        for(auto e:r) {
            hs[e[0]]++;
            hs[e[1]]++;
        }
        vector<int> a;
        for(auto &[val,cnt]:hs) a.push_back(cnt);
        sort(a.begin(),a.end());
        int len=a.size();
        ll count=n;
        for(int i=len-1;i>=0;--i) {
            ans+=count*a[i];
            --count;
        }
        return ans;
    }
};
~~~

## [以组为单位订音乐会的门票](https://leetcode.cn/problems/booking-concert-tickets-in-groups/)

**thinking**

线段树（待补）

**solution**

~~~C++
class BookMyShow {
public:
    int n,m;
    vector<int> min;
    vector<long> sum;

    void add(int o,int l,int r,int idx,int val) {
        if(l==r) {
            min[o]+=val;
            sum[o]+=val;
            return;
        }
        int m=(l+r)/2;
        if(idx<=m) add(o*2,l,m,idx,val);
        else add(o*2+1,m+1,r,idx,val);
        min[o]=std::min(min[o*2],min[o*2+1]);
        sum[o]=sum[o*2]+sum[o*2+1];
    }

    long query_sum(int o,int l,int r,int L,int R) {
        if(L<=l&&r<=R) return sum[o];
        long sum=0L;
        int m=(l+r)/2;
        if(L<=m) sum+=query_sum(o*2,l,m,L,R);
        if(R>m) sum+=query_sum(o*2+1,m+1,r,L,R);
        return sum;
    }

    int index(int o,int l,int r,int R,int val) {
        if(min[o]>val) return 0;
        if(l==r) return l;
        int m=(l+r)/2;
        if(min[o*2]<=val) return index(o*2,l,m,R,val);
        if(m<R) return index(o*2+1,m+1,r,R,val);
        return 0;
    }

    BookMyShow(int n, int m):n(n),m(m),min(n*4),sum(n*4) {}
    
    vector<int> gather(int k, int maxRow) {
        int i=index(1,1,n,maxRow+1,m-k);
        if(i==0) return {};
        int seats=query_sum(1,1,n,i,i);
        add(1,1,n,i,k);
        return {i-1,seats};
    }
    
    bool scatter(int k, int maxRow) {
        if((long)(maxRow+1)*m-query_sum(1,1,n,1,maxRow+1)<k) return false;
        for(int i=index(1,1,n,maxRow+1,m-1);;++i) {
            int left_seats=m-query_sum(1,1,n,i,i);
            if(k<=left_seats) {
                add(1,1,n,i,k);
                return true;
            }
            k-=left_seats;
            add(1,1,n,i,left_seats);
        }
        return true;
    }
};
~~~

