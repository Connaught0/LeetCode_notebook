# 513 Find Botom Left Tree Value
solution 1:
```
    int result = 0;
    int maxDepth = INT_MIN;
    void helper(TreeNode* node, int depth){
        if(node -> left == nullptr && node -> right == nullptr)
        {
            if(maxDepth < depth)
            { 
                maxDepth = depth;
                result = node -> val;
            } 
        }
        if(node -> left){
            depth++;
            helper(node -> left, depth+1);
            depth--;
        }
        if(node -> right){
            depth++;
            helper(node -> right, depth);
            depth--;
        }
    }
    int findBottomLeftValue(TreeNode* root) {
        helper(root, 1);
        return result;
    }
```
写完之后其实有一个疑问，他并没有判断哪一个是最左边的。
不过想了想由于是先左后右，当第一次看见新的最大深度的时候必然就是最左边的哪一个，于是乎并不需要额外判断一次哪一个是最大深度。
第二个疑问是是否能够使用引用而非全局的数据放入到递归之中。发现如果是引用的话，在不同的时候得到的结果实际上是不一样的。比如进入了左树实际上并不会影响右树。

solution 2：
```
    int findBottomLeftValue(TreeNode* root) {
        int ans = 0;
        int maxDpeth = INT_MIN;
        deque<TreeNode*> nodeQue;
        nodeQue.push_back(root);

        int depth = 1;
        while(!nodeQue.empty()){
            if(maxDpeth < depth){
                maxDpeth = depth;
                ans = nodeQue.front()->val;
            }
            int size = nodeQue.size();
            for(int i = 0; i < size; i++)
            {
                TreeNode* nowNode = nodeQue.front();
                nodeQue.pop_front();
                //if (i == 0) result = node->val; // 记录最后一行第一个元素
                if(nowNode -> left)nodeQue.push_back(nowNode -> left);
                if(nowNode -> right)nodeQue.push_back(nowNode -> right);
            }
            depth++;
        }
        return ans;
    }
```
实际上当i=0时即可当作当前层的最左边那一个！

# 112 Path Sum
第一反应，遍历所有路径，然后存一个hash，这个算暴力解法？
其实感觉可以层序，然后判断是否有小于其的，如果有就继续将其子节点加进去，如果没有就直接返回false了。
如果前序也行，
wrong solution:
```
    bool helper(TreeNode* node, int fatherSum ,int targetSum){
        if(node == nullptr)
        {
            return fatherSum == targetSum; 
        }
        int sum  = fatherSum + node -> val; 
        if(sum > targetSum)
        {
            return false;
        }
        // else if(sum == targetSum)
        // {
        //     return true;
        // }提前开香槟是吧，能提前宣布结束，不能提前宣布成功
        bool left = false, right = false;
        if(node->left)left = helper(node->left, sum, targetSum);
        if(node->right)right = helper(node->right, sum, targetSum);
        return left || right;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        return helper(root, 0, targetSum);
    }
```
这个题解引发的问题是，什么时候用判断子节点，什么时候用判断为null
需要判断叶子节点的时候判断是否是叶子节点
如果只是遍历的话直接看做类似图的方式去遍历即可
solution 2:
```
    bool helper(TreeNode* node, int fatherSum ,int targetSum){
        int sum  = fatherSum + node -> val; //中
        if(node->left == nullptr && node->right == nullptr)
        {
            return sum == targetSum; 
        }
        // if(sum > targetSum)
        // {
        //     return false;
        // }有负数，不能这么做
        // else if(sum == targetSum)
        // {
        //     return true;
        // }提前开香槟是吧，能提前宣布结束，不能提前宣布成功
        bool left = false, right = false;
        if(node->left)left = helper(node->left, sum, targetSum);
        if(node->right)right = helper(node->right, sum, targetSum);
        return left || right;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        return helper(root, 0, targetSum);
    }
```
再来看返回值，递归函数什么时候需要返回值？什么时候不需要返回值？这里总结如下三点：

如果需要搜索整棵二叉树且不用处理递归返回值，递归函数就不要返回值。（这种情况就是本文下半部分介绍的113.路径总和ii）
如果需要搜索整棵二叉树且需要处理递归返回值，递归函数就需要返回值。 （这种情况我们在236. 二叉树的最近公共祖先 (opens new window)中介绍）
如果要搜索其中一条符合条件的路径，那么递归一定需要返回值，因为遇到符合条件的路径了就要及时返回。（本题的情况）