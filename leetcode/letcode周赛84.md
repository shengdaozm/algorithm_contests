## [翻转图像](https://leetcode.cn/problems/flipping-an-image/)

~~~C++
class Solution {
public:
    vector<vector<int>> flipAndInvertImage(vector<vector<int>>& g) {
        int m=g.size();
        int n=g[0].size();
        for(int i=0;i<m;++i) {
            for(int j=0;j<n/2;++j) {
                swap(g[i][j],g[i][n-j-1]);
            }
        }
        for(int i=0;i<m;++i) {
            for(int j=0;j<n;++j) {
                g[i][j]=g[i][j]==0?1:0;
            }
        }
        return g;
    }
};
~~~

## [字符串中的查找与替换](https://leetcode.cn/problems/find-and-replace-in-string/)

~~~C++
~~~

