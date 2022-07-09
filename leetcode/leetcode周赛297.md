## [计算应缴税款总额](https://leetcode.cn/problems/calculate-amount-paid-in-taxes/)

~~~C++
class Solution {
public:
    double calculateTax(vector<vector<int>>& b, int in) {
        if(in<=b[0][0]) return in*(b[0][1]/100.0);
        double ans = b[0][0] * (b[0][1] / 100.0);
        for (int i = 1; i < b.size(); i++) {
        	if(in>b[i-1][0]) 
            	ans+=(min(b[i][0],in)-b[i-1][0])*(b[i][1]/100.0);
        }
        return ans;
    }
};
~~~

## [网格中的最小路径代价](https://leetcode.cn/problems/minimum-path-cost-in-a-grid/)

~~~C++
~~~

