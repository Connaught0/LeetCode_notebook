# 455 Assign Cookies
thinking:
```
        //a. 两个for循环，把所有的配对都找出来
        //b. 用一个hash存所有的饼干数量，如果有孩子配对，就++，当饼干数为0的时候不分配
        int ans = 0;
        unordered_map<int, int> cookieNum;
        for(int i = 0; i < s.size(); i++){
            if(cookieNum.find(s[i]) == cookieNum.end()){
                cookieNum[s[i]] = 1;
            }
            else{
                cookieNum[s[i]]++;
            }
        }
        for(int i = 0; i < g.size(); i++){
            if(cookieNum.find(g[i]) == cookieNum.end() || cookieNum[g[i]] <= 0){
                continue;
            }
            else{
                cookieNum[g[i]]--;
                ans++;
            }
        }
        return ans;
```
这个思路只能是把能够分配的给分配了，而不是完全做到只要覆盖即可分配
```
    int findContentChildren(vector<int>& g, vector<int>& s) {
        //a. 排序，倒序减少
        
        int j = 0;
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        while( s[s.size() - j - 1]>=g[g.size() - j - 1] && j < s.size() && j < g.size()){
            j++;
        }
        return j;
    }
```

# 376 wiggle Subsequence
....看错题了
```
        int max_fore = 0;
        int max_back = 1;
        int max = 2;
        int lastSub = nums[max_back]-nums[max_fore];

        int i = 0;
        for(int j = 2; j < nums.size(); j++){
            if((nums[j]-nums[j-1] > 0 && lastSub < 0) || (nums[j]-nums[j-1] < 0 && lastSub > 0)){
                if((j - i + 1) > max)
                {
                    max_fore = i;
                    max_back = j;
                    max = max_back - max_fore + 1;
                }
            }
            else{
                i = j;
            }
            lastSub = nums[j]-nums[j-1];
        }
        return max;
```
solution:
```
        if(nums.size()<=1) return nums.size();
        int prediff = 0;
        int curdiff = 0;
        int ans = 1;
        for(int i = 1; i < nums.size(); i++){
            curdiff = nums[i] - nums[i - 1];
            if( (prediff >= 0 && curdiff < 0) || (prediff <= 0 && curdiff > 0)){
                ans++;
                prediff = curdiff;
            }
        }
        return ans;
```

# maximum-subarray
```
    int maxSubArray(vector<int>& nums) {
        int max = INT_MIN;
        int sum = 0;
        for(int i = 0; i < nums.size(); i++){
            sum = 0;
            for(int j = i; j < nums.size(); j++){
                sum += nums[j];
                if(sum > max){
                    max = sum;
                }
            }
        }
        return max;
    }
```
solution
```
    int maxSubArray(vector<int>& nums) {
        int max = INT32_MIN;
        int sum = 0;
        for(int i = 0; i < nums.size(); i++){
            sum += nums[i];
            max = sum > max ? sum : max;//得先将sum存入，再将sum归0，顺序不能调整
            sum = sum < 0 ? 0 : sum;
        }
        return max;
    }