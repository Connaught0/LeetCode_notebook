# 39 Combination Sum
```
    vector<int> path;
    vector<vector<int>> ans;
    void backTracking(vector<int> nums, int target, int startIndex){
        if(target == 0){
            ans.push_back(path);
            return;
        }
        else if(target < 0){
            return;
        }
        for(int i = startIndex; i < nums.size(); i++){
            // if(target - nums[i] < 0)
            //     return;
            path.push_back(nums[i]);
            backTracking(nums, target - nums[i], i);
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        path.clear();
        ans.clear();
        //sort(candidates.begin(),candidates.end());//如果不剪枝就可以不用排序。因为剪枝操作导致很多组合没有尝试，因此需要排序，确保从小的开始相加
        backTracking(candidates, target, 0);
        return ans;
    }
```
# 40 Combination Sum 2
```
    vector<int> path;
    vector<vector<int>> ans;
    void backTracking(vector<int> nums, int target, int startIndex, vector<bool> used){
        if(target == 0){
            ans.push_back(path);
            return;
        }
        else if(target < 0){
            return;
        }
        for(int i = startIndex; i < nums.size(); i++){
            if(target - nums[i] < 0)
                return;
            if(i >= 1 && nums[i] == nums[i-1] && used[i-1] == false){//实际上真正判断是否重复的是第一句，但是如果只判断这个第一句的话就会导致同一个树杈上的判断错误，因此加上used来控制
                continue;
            }
            used[i] = true;
            path.push_back(nums[i]);
            backTracking(nums, target - nums[i], i+1, used);
            used[i] = false;
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        path.clear();
        ans.clear();
        sort(candidates.begin(),candidates.end());
        vector<bool> used = vector<bool>(candidates.size(), false);
        backTracking(candidates, target, 0, used);
        return ans;
    }
```
# 131 
```
    vector<vector<string>> ans;
    vector<string> path;
    bool isPalindrome(string s, int fore, int back){
        while(fore < back){
            if(s[fore] != s[back])
            {
                return false;
            }
            fore++;
            back--;
        }
        return true;
    }
    void backTracking(string s, int startIndex){
        if(startIndex >= s.size()){
            if(path.size()!=0){
                ans.push_back(path);
            }
            return;
        }
        for(int i = startIndex; i < s.size(); i++){
            if(isPalindrome(s, startIndex, i)){
                path.push_back(s.substr(startIndex, i - startIndex + 1));
            }
            else{
                continue;//主要是这里一开始没想明白，这里的startIndex实际上指的是当前字符串的最开头，i是分割的地方，因此当0-2分割完了之后就不用再看2-3了
            }
            backTracking(s, i+1);
            path.pop_back();
        }
        return;
    }
    vector<vector<string>> partition(string s) {
        path.clear();
        ans.clear();
        backTracking(s, 0);
        return ans;
    }
```