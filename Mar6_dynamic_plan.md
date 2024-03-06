# 509 Fibonacci Number
solution:
```
int fib(int n) {
        if(n <= 1) return n;
        vector<int> dp = vector<int>(n+1); //dp means the ith number
        //intial
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <=n; i++){
            dp[i] = dp[i - 1] + dp[i - 2];//递推公式
        }
        return dp[n];
    }

```

```
int fib(int n) {
        if(n <= 1) return n;
        vector<int> dp = vector<int>(2); //dp means the ith number
        //intial
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <=n; i++){
            int sum = dp[1] + dp[0];//递推公式
            dp[0] = dp[1];
            dp[1] = sum;
        }
        return dp[1];
    }
```

# 746 Min cost Climbing Stairs
```
int minCostClimbingStairs(vector<int>& cost) {
        int i = cost[0] >= cost[1] ? 1 : 0;
        int sum = 0;
        while(i < cost.size() - 2){
            i = cost[i + 1] >= cost[i + 2] ? i+2 : i+1;
            sum+=cost[i];
        }
        return sum;
    }
```
此时贪心贪不到最后
```
int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp = vector<int>(cost.size()+1);
        dp[0] = 0;
        dp[1] = 0;
        for(int i = 2; i <= cost.size(); i++){
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[cost.size()];
    }
```