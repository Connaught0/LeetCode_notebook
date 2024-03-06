# 435 Non-overlapping Intervals
solution:
```
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        //不同于气球穿透，这个是开区间
        sort(intervals.begin(),intervals.end(),[](vector<int> a, vector<int> b){
            if(a[0] == b[0]) return a[1] < b[1];
            return a[0] < b[0];
        });
        int aim = intervals[0][1];
        int ans = 0;
        for(int i = 1; i < intervals.size(); i++){
            if(aim > intervals[i][0]){
                aim = min(aim, intervals[i][1]);
                ans++;
            }
            else{
                aim = intervals[i][1];
            }
        }
        return ans;
    }
```

# 56 Merge Intervals
solution:
```
vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int> a, vector<int> b){
            if(a[0] == b[0]) return a[1] < b[1];
            return a[0] < b[0];
        });
        vector<vector<int>> ans;
        int left = intervals[0][0];
        int right = intervals[0][1];//取第一个的头
        for(int i = 1; i < intervals.size(); i++){
            if(right >= intervals[i][0]){
                right = max(right, intervals[i][1]);//在内就找最大的尾巴
            }
            else{
                vector<int> a;
                a.push_back(left);
                a.push_back(right);//否则就加进去，然后再看新的
                ans.push_back(a);
                left = intervals[i][0];
                right = intervals[i][1];
            }
        }
        vector<int> a;
        a.push_back(left);
        a.push_back(right);//到最后了。直接加
        ans.push_back(a);
        return ans;
    }
```

# 763 Partition Labels
solution：
```
    vector<int> partitionLabels(string s) {
        int hash[27] = {0};
        for(int i = 0; i < s.size(); i++){
            hash[s[i] - 'a'] = i;
        }
        vector<int> ans;
        int left = 0;
        int right = 0;
        for(int i = 0; i < s.size(); i++){
            right = max(right, hash[s[i]-'a']);
            if(i == right){
                ans.push_back(right - left + 1);
                left = right + 1;
            }
        }
        return ans;
    }
```
这题主要是没看懂要我干啥，其实就是让每一个字符串尽可能的短，并且每一个字符只出现一次。