# 216 combination sum 3
```
public:
    vector<int> path;
    vector<vector<int>> result;
    void backTracking(int n,int k,int startIndex)
    {
        if(path.size() > k){
            return;
        }
        if(n == 0 && path.size() == k){
            result.push_back(path);
            return;
        }
        for(int i = startIndex; i <= 9; i++){//i <= 9 - (k - path.size()) + 1//根据数组大小剪枝
            path.push_back(i);
            //根据大小剪枝
            if(n - i < 0){
                path.pop_back();//一开始写忘记要回溯了，导致出现了错误的结果
                return;
            }
            backTracking(n - i, k, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        path.clear();
        result.clear();
        backTracking(n, k, 1);
        return result;
    }
```

# 17 letter Combinations of a Phone Number
```
   unordered_map<char, string> dict=
    {
        {'2',"abc"},
        {'3',"def"},
        {'4',"ghi"},
        {'5',"jkl"},
        {'6',"mno"},
        {'7',"pqrs"},
        {'8',"tuv"},
        {'9',"wxyz"}
    };
    vector<char> centen;
    vector<string> ans;
    void backTracking(string digits, int index){
        if(digits.size() == index){
            string a = "";
            for(auto c : centen){
                a += c;
            }
            ans.push_back(a);
            return;
        }
        char key = digits[index];
        string words = dict[key];
        for(int i = 0; i < words.size(); i++){
            centen.push_back(words[i]);
            backTracking(digits, index+1);
            centen.pop_back();
        }
        return;
    }
    vector<string> letterCombinations(string digits) {
        centen.clear();
        ans.clear();
        if(digits == ""){
            return ans;
        }
        backTracking(digits, 0);
        return ans;
    }
```


代码中最好考虑这些异常情况，但题目的测试数据中应该没有异常情况的数据，所以我就没有加了。

但是要知道会有这些异常，如果是现场面试中，一定要考虑到！

s.push_back(letters[i]); 似乎string的底层是vector，注意回顾补充！！