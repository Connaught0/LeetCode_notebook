# 93 revocery the IP dress
solution:
```
    vector<string> ans;
    bool inRange(string num, int fore, int back){//左闭右闭
        if(fore > back)
            return false;
        if(num[fore] == '0' && fore != back)
            return false;
        int n = 0;
        for(int i = fore; i <= back; i++){
            if(num[i]>'9'||num[i]<'0')
                return false;
            n = n * 10 + (num[i]-'0');
            //n += n * 10 + (num[i]-'0');本来是写得每层*10之后相加，修改了之后没有改回来，导致错误
            if(n > 255){
               return false;
            }
        }
        
        return true;
    }
    void backTracking(string &s, int startIndex, int pointNum){
        if(pointNum == 3){
            if(inRange(s, startIndex, s.size()-1))
                ans.push_back(s);
            return;
        }
        for(int i = startIndex; i < s.size(); i++){
            if(inRange(s, startIndex, i)){
                s.insert(s.begin() + i + 1, '.');
                pointNum++;
                backTracking(s, i + 2, pointNum);
                s.erase(s.begin() + i + 1);
                pointNum--;
            }
            else
                break;//前面超范围就不用继续往后查了
        }
        return;
    }
    vector<string> restoreIpAddresses(string s) {
        ans.clear();
        if(s.size()<4||s.size()>12)return ans;
        backTracking(s, 0, 0);
        return ans;
    }
```

# 90 Subset With duplicate
solution:
```
vector<vector<int>> ans;
    vector<int> path;
    void backTracking(vector<int>& nums, int startIndex, vector<bool> useNum){
        ans.push_back(path);
        if(startIndex >= nums.size() ){
            return;
        }
        for(int i = startIndex; i < nums.size(); i++){
            if(i>0 && nums[i] == nums[i-1] && useNum[i-1] == false)
                continue;
            path.push_back(nums[i]);
            useNum[i] = true;//do not forget
            backTracking(nums, i + 1, useNum);
            path.pop_back();
            useNum[i] = false;
        }

    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums){
        path.clear();
        ans.clear();
        vector<bool> useNum = vector<bool>(nums.size(), false);
        sort(nums.begin(),nums.end());
        backTracking(nums, 0, useNum);
        return ans;
    }
```