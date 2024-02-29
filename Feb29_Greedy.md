# 122 Best Time to buy and sell Stock 2
my solution：
本质上是沿着摇摆子序列的思路去写的，没想到还能有更简单的。
贪心的思路是找最低的买，找最高的卖。
最后一天直接抛售
```
    int maxProfit(vector<int>& prices) {
        //类似于昨天的摇摆子序列
        //找到波谷购入，遇到波峰卖出
        int prediff = 0;
        int curdiff = 0;
        int fore = 0;
        int share = -1;
        int ans = 0;
        for(int back = 0; back < prices.size(); back++){
            if(back == prices.size() -1 && share >= 0 && prices[back] >= share){
                ans += (prices[back] - share);//最后一天紧急抛售
                break;
            }
            if(back + 1 >= prices.size())
            {
                break;//没有后一天了，不看了
            }
            curdiff = prices[back + 1] - prices[back];
            if(prediff<=0 && curdiff>0){
                share = prices[back];//波谷买入
                prediff = curdiff;
            }
            if(prediff>=0 && curdiff<0 && share >= 0){
                //sale
                ans += (prices[back] - share);//波峰卖出
                prediff = curdiff;
                share = -1;
            }
        }
        return ans;
    }
```
solution：
只要有利润就直接进行一次出售，算总和的利润
```
    int maxProfit(vector<int>& prices) {
        int ans = 0;
        for(int i = 1; i < prices.size(); i++){
            ans += max(prices[i] - prices[i - 1], 0);
        }
        return ans;
    }
```

# 55 Jump Game
my solution:
暴力解，找到一个能跳到最后的
```
    bool backTracking(vector<int>& nums, int nowPos){
        if(nowPos >= nums.size() - 1){
            return true;
        }
        for(int i = nums[nowPos]; i > 0; i--){
            if(backTracking(nums, nowPos + i))
                return true;
        }
        return false;
    }
    bool canJump(vector<int>& nums) {
        //回溯的写法。。。
        return backTracking(nums,0);
    }
```
solution:
```  
  bool canJump(vector<int>& nums) {
        int cover = nums[0];
        if(nums.size() == 1) return true;
        for(int i = 0; i <= cover; i++){
            cover = max(i + nums[i], cover);
            if(cover >= nums.size() - 1)
                return true;
        }
        return false;
    }
```

# 45 Jump Game 2
solution:
```
    int jump(vector<int>& nums) {
        int curcover = 0;//第0步，如果设置了初始值，那就在第一步了
        int nextcover = 0;
        int ans = 0;//所以是0
        if(nums.size() == 1) return 0;
        for(int i = 0; i <nums.size(); i++){
            nextcover = max(i + nums[i], nextcover);
            if(i == curcover){
                curcover = nextcover;
                ans++;
                if(nextcover >= nums.size() - 1)
                break;
            }
            
        }
        return ans;
    }
```