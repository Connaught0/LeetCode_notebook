# 回溯
回溯本质上就是穷举，那么遍历本质上也是一种穷举问题
组合问题：N个数里面按一定规则找出k个数的集合
切割问题：一个字符串按一定规则有几种切割方式
子集问题：一个N个数的集合里有多少符合条件的子集
排列问题：N个数按一定规则全排列，有几种排列方式
棋盘问题：N皇后，解数独等等
```
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

# 77 Combination
1. 两个for循环遍历所有O(n^2) !!!! 如果k不为2 那么就需要多次for，无法适用于所有的
2. 回溯
```
 vector<vector<int>> ans;
    vector<int> path;
    void backTracking(int n, int k, int startIndex){
        if(path.size() == k){
            ans.push_back(path);
            return;
        }
        //for(int i = startIndex; i <= n - (k -path.size()) + 1; i++)
        //剪枝就是把无效的for循环的步骤给去掉
        //就是不用让i再去到不存在结果的地方
        for(int i = startIndex; i <= n; i++){
            path.push_back(i);
            backTracking( n, k, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        path.clear();
        ans.clear();
        backTracking(n, k, 1);
        return ans;
    }
```