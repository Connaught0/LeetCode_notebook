# 491 non decreasing subsequnce
```
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    void backTracking(vector<int> nums, int startIndex){
        if(path.size() > 1){//子序列需要至少为2
            ans.push_back(path);
            //return; 类似于子集问题，需要找到所有子集
        }
        unordered_set<int> uset;//无法重排序通过判断是否和前一个相同来决定是否是用过的
        for(int i = startIndex; i < nums.size(); i++){
            if(uset.find(nums[i]) != uset.end()){
                continue;
            }
            if(path.size() == 0){
                path.push_back(nums[i]);
            }
            else if(path.size() > 0 && nums[i] >= path.back()){
                path.push_back(nums[i]);
            }
            else if(path.size() > 0 && nums[i] < path.back())
            {
                continue;
            }
            uset.insert(nums[i]);
            backTracking(nums, i+1);
            path.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        ans.clear();
        path.clear();
        backTracking(nums, 0);
        return ans;
    }
};

```

after polish：
```
    vector<vector<int>> ans;
    vector<int> path;
    void backTracking(vector<int> nums, int startIndex){
        if(path.size() > 1){//point1
            ans.push_back(path);
        }
        unordered_set<int> uset;//point 2
        for(int i = startIndex; i < nums.size(); i++){
            if((path.size() > 0 && nums[i] < path.back()) //有变小的
                || uset.find(nums[i]) != uset.end()){ //有重复的
                continue;
            }
            path.push_back(nums[i]);
            uset.insert(nums[i]);
            backTracking(nums, i+1);
            path.pop_back();
        }
    }
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        ans.clear();
        path.clear();
        backTracking(nums, 0);
        return ans;
    }
```

# 46 permutation
发现自己思考算法总是以一种希望最优雅的解决方式，  
一开始看到排列问题的时候，我想的时候从nums中erase掉当前层使用的数，  
然后传递修改后的nums进入到下一层，  
然后我就被每一次都是从第一个开始选的这个思维给困住了。  
实际上，以1， 2， 3举例  
1弹出之后，第一次for循环2弹出，第二次for循环3弹出也就完成了所有的结果。  
只是erase的效率很明显比多用一个used更低。  
```
    vector<int> path;
    vector<vector<int>> ans;
    vector<bool> used;
    void backTracking(vector<int> nums, vector<bool> used){
        if(path.size() == nums.size()){
            ans.push_back(path);
            return;
        }
        for(int i = 0; i < nums.size(); i++){
            if(used[i] == true){
                continue;
            }
            path.push_back(nums[i]);
            used[i] = true;
            backTracking(nums, used);
            path.pop_back();
            used[i] = false;

        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        path.clear();
        ans.clear();
        used = vector<bool>(nums.size(), false);
        backTracking(nums, used);
        return ans;
    }
```

# 47 permutation_with_dicuplate
```
    vector<int> path;
    vector<vector<int>> ans;
    vector<bool> used;
    void backTracking(vector<int> nums, vector<bool> used){
        if(path.size() == nums.size()){
            ans.push_back(path);
            return;
        }
        for(int i = 0; i < nums.size(); i++){
            if(used[i] == true){
                continue;
            }
            if(i > 0 && nums[i-1] == nums[i] && used[i-1] == false){
                continue;
                //break 这里一开始写的是break，发现112这个例子怎么跑都不进2
            }
            path.push_back(nums[i]);
            used[i] = true;
            backTracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
        return;
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        path.clear();
        ans.clear();
        used = vector<bool>(nums.size(), false);
        sort(nums.begin(), nums.end());
        backTracking(nums, used);
        return ans;
    }
```
`if(i > 0 && nums[i-1] == nums[i] && used[i-1] == false){`这句有两种写法  
一种是`used[i - 1] == false` 对树层去重
一种是`used[i - 1] == true` 对树枝去重