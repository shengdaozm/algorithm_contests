## [极大极小游戏](https://leetcode.cn/problems/min-max-game/)

~~~C++
class Solution {
public:
    int minMaxGame(vector<int>& nums) {
        queue<int>q;
        for(auto x:nums) q.push(x);
        int count=0;
        while(q.size()!=1) {
            int len=q.size();
            for(int i=0;i<len/2;++i) {
                int a=q.front();q.pop();
                int b=q.front();q.pop();
                if(count%2==0)
                    q.push(min(a,b));
                else 
                    q.push(max(a,b));
                ++count;   
            }
        }
        return q.front();
    }
};
~~~

## [划分数组使最大差为 K](https://leetcode.cn/problems/partition-array-such-that-maximum-difference-is-k/)

~~~C++
class Solution {
public:
    int partitionArray(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        int n=nums.size();
        int ans=0;
        for(int i=n-1;i>=0;--i) {
            int num=nums[i];
            --i;
            while(i>=0&&num-k<=nums[i]) --i;
            if(i<0) {
                ++ans;
                return ans;
            }
            ++i;
            ++ans;
        }
        return ans;
    }
};
~~~

## [替换数组中的元素](https://leetcode.cn/problems/replace-elements-in-an-array/)

~~~C++
class Solution {
public:
    vector<int> arrayChange(vector<int>& nums, vector<vector<int>>& o) {
        unordered_map<int,int> hs,sh;
        for(auto x:o) {
            if(sh.count(x[0])) {
                int num=sh[x[0]];
                hs[num]=x[1];
                sh.erase(x[0]);
                sh[x[1]]=num;
            } else {
                hs[x[0]]=x[1];
                sh[x[1]]=x[0];
            }
        }
        for(auto &x:nums) {
            if(hs.count(x)) {
                x=hs[x];
            }
        }
        return nums;
    }
};
~~~

## [设计一个文本编辑器](https://leetcode.cn/problems/design-a-text-editor/)

**thinking**

直接用字符串模拟，TLE，采用前排大佬的操作，进行双栈模拟

**solution**

~~~C++
class TextEditor {
public:
    stack<char>left;
    stack<char>right;
    TextEditor() {}
    
    void addText(string text) {for(auto x:text) left.push(x);}
    
    int deleteText(int k) {
        int cnt=0;
        while(left.size()>0&&k>0) {
            left.pop();
            ++cnt;
            --k;
        }
        return cnt;
    }
    
    string cursorLeft(int k) {
        int num=min(k,int(left.size()));
        string ans;
        while(num) {
            --num;
            char x=left.top();left.pop();
            right.push(x);
        }
        num=min(int(left.size()),10);
        ans.resize(num);
        for(int i=num-1;i>=0;--i) {
            ans[i]=left.top();
            left.pop();
        }
        for(int i=0;i<num;++i) left.push(ans[i]);
        return ans;
    }
    
    string cursorRight(int k) {
        int num=min(k,int(right.size()));
        string ans;
        while(num) {
            --num;
            char x=right.top();right.pop();
            left.push(x);
        }
        num=min(int(left.size()),10);
        ans.resize(num);
        for(int i=num-1;i>=0;--i) {
            ans[i]=left.top();
            left.pop();
        }
        for(int i=0;i<num;++i) left.push(ans[i]);
        return ans;
    }
};
~~~

