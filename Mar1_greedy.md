# 1005 Maximize Sun of Array after K negations
solution:
```
    int sum(vector<int> nums){
        int ans = 0;
        for(auto &a : nums){
            ans += a;
        }
        return ans;
    }
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        //将所有的顺序排列
        sort(nums.begin(),nums.end());
        int i = 0;
        //将所有大的负数变为正德
        while(k > 0  && i < nums.size() && nums[i] < 0){
            nums[i] = -nums[i];
            k--;
            i++;
        }
        //1. k 用完了，但是还有负的
        //此时 i 指向负数
        //1.5. k 刚好用完，i 指向0，
        //2. k 没用完，选择最小的或者0来消耗k的次数
        // 此时i指向原本最大的负数
        //3. k 没用完，数组不够了，同2
        // 此时i指向原本最大的负数或是0
        if(k > 0){
            sort(nums.begin(),nums.end(),[](int a,int b){return abs(a)<abs(b);});
            nums[0] = k % 2 == 0 ? nums[0] : -nums[0];
        }
        return sum(nums);
    }
```
polish solution: only search once is enough.
```
int largestSumAfterKNegations(vector<int>& nums, int k) {
        //将所有的按绝对值顺序排列
        sort(nums.begin(), nums.end(),[](int a, int b){return abs(a) < abs(b);});
        for(int i = nums.size() - 1; i >= 0 ; i--){
            if(nums[i] < 0){
                nums[i] = - nums[i];
                k--;
            }
            if(k == 0)
                break;
        }
        if( k > 0 ){
            nums[0] = k % 2 == 0? nums[0] : - nums[0];
        }
        int sum = 0;
        for( auto a : nums){
            sum += a;
        }
        return sum;
    }
```

# 134 Gas station
solution but out of time:
```    
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        //找到目前站和下一站差值最大的始发站开始
        int i = 0;
        int n = gas.size();
        if(n == 1 && gas[0] >= cost[0]) return 0;//只有一个站，能到就行
        vector<pair<int, int>> gas_sub_cast = vector<pair<int,int>>(n);//根据这一站加了油，能否到下一站，剩下的汽油排序，从小到大
        int sum = 0;
        for(int i = 0; i < n ; i++){
            gas_sub_cast[i] = pair<int,int>(i, gas[i] - cost[i]);
            sum += gas[i] - cost[i];
        }
        if(sum < 0)
            return -1;
        sort(gas_sub_cast.begin(), gas_sub_cast.end(),[](pair<int, int> a,pair<int, int> b){
            return a.second < b.second;
        });//increase and find the first non-zero
        for(int i = 0; i < n; i++){
            if(gas_sub_cast[i].second > 0){
                int start = gas_sub_cast[i].first;//贪心找到最少的。。。这里傻逼了，直接看最大的可不可以不久行了嘛，如果不可以就下一个//////。。。。。。
                int last = gas[start] - cost[start]; //如果可以就返回
                int point = (start + 1) % n;//
                while(last >= 0 ){
                    last += gas[point] - cost[point];
                    point = (point + 1) % n;
                    if(last >= 0 && point == start){
                        return start;
                    }
                    
                }
            }
        }
        return -1;
    }
```
////过了
solution:
```
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        //找到目前站和下一站差值最大的始发站开始
        int i = 0;
        int n = gas.size();
        if(n == 1 && gas[0] >= cost[0]) return 0;
        vector<pair<int, int>> gas_sub_cast = vector<pair<int,int>>(n);
        int sum = 0;
        for(int i = 0; i < n ; i++){
            gas_sub_cast[i] = pair<int,int>(i, gas[i] - cost[i]);
            sum += gas[i] - cost[i];
        }
        if(sum < 0)
            return -1;
        sort(gas_sub_cast.begin(), gas_sub_cast.end(),[](pair<int, int> a,pair<int, int> b){
            return a.second > b.second;
        });//increase and find the first non-zero
        for(int i = 0; i < n; i++){
            if(gas_sub_cast[i].second > 0){
                int start = gas_sub_cast[i].first;
                int last = gas[start] - cost[start]; 
                int point = (start + 1) % n;
                while(last >= 0 ){
                    last += gas[point] - cost[point];
                    point = (point + 1) % n;
                    if(last >= 0 && point == start){
                        return start;
                    }
                    
                }
            }
        }
        return -1;
    }
```
solution:
```
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        //找到目前站和下一站差值最大的始发站开始
        int totalSum = 0;
        int curSum = 0;
        int start = 0;
        for(int i = 0; i < gas.size(); i++){
            totalSum += gas[i] - cost[i];
            curSum += gas[i] - cost[i];
            if(curSum < 0){//当前小于0了，就看下一个是不是作为起点，后续会不会小于0
                curSum = 0;
                start = i + 1;
            }
        }
        if(totalSum < 0){
            return -1;
        }
        return start;
    }
```
# 135 Candy
```
    int candy(vector<int>& ratings) {
        //别想着一遍过
        vector<int> candies = vector<int>(ratings.size(), 1);
        for(int i = 0; i < ratings.size(); i++){
            if(ratings[i+1] > ratings[i]){//后面大于前面，后面+1
                candies[i+1] = candies[i] + 1;
            }
        }
        for(int i = ratings.size() - 1; i > 0; i--){
            if(ratings[i] < ratings[i-1]){//后面小于前面，前面的等于现在的，或者是后面的+1
                candies[i-1] = max(candies[i-1], candies[i]+1);
            }
        }
        int ans = 0;
        for(int a : candies){
            ans += a;
        }
        return ans;
    }
```