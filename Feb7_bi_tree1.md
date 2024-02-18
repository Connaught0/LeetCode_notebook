# 102 level order travl
本来想的是用NULL做结束符，导致出现了无限循环，其实没必要节约太多的空间，利用好已有的数据
```
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> nodeQue;
        nodeQue.push(root);
        nodeQue.push(NULL);//作为当前层的结束符
        vector<vector<int>> ans;
        vector<int> nowLevelAns;
        while(!nodeQue.empty()){
            TreeNode* point = nodeQue.front();
            nodeQue.pop();
            if(!point){
                ans.push_back(nowLevelAns);
                nowLevelAns = vector<int>();
                nodeQue.push(NULL);
                continue;
            }
            nowLevelAns.push_back(point -> val);
            if(point -> left)nodeQue.push(point -> left);
            if(point -> right)nodeQue.push(point -> right);
            }
        return ans;
        }
```
solution ：
```
   vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> nodeQue;
        if(root)nodeQue.push(root);
        vector<vector<int>> ans;
        vector<int> nowLevelAns;
        while(!nodeQue.empty()){
            int size = nodeQue.size();
            for(int i=0; i < size; i++)
            {
                TreeNode* point = nodeQue.front();
                nodeQue.pop();
                nowLevelAns.push_back(point -> val);
                if(point -> left)nodeQue.push(point -> left);
                if(point -> right)nodeQue.push(point -> right);
            }
            ans.push_back(nowLevelAns);
            nowLevelAns = vector<int>();
            }
        return ans;
        }
```

# 107 buttom to top level order
...没想到reverse咋写
```
    void perLevel(TreeNode* node, vector<vector<int>>& res, int depth)
    {
        if(node==nullptr)return;
        if(res.size() == depth) res.push_back(vector<int>());
        res[depth].push_back(node -> val);
        perLevel(node -> left, res, depth+1);
        perLevel(node -> right, res, depth+1);
    }
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        
        vector<vector<int>> ans;
        perLevel(root, ans, 0);
        ///////////////////////
        stack<vector<int>> res;
        for(int i=0;i < ans.size(); i++)
        {
            res.push(ans[i]);
        }
        ans = vector<vector<int>>();
        while(!res.empty())
        {
            ans.push_back(res.top());
            res.pop();
        }
        ///////////////////////////
        reverse(ans.begin(), ans.end());
        return ans;
    }
```

# 226 Invert Bi tree
为什么我写void的执行用时比歇treenode的更多呢？
4ms 11.17mb
```
    TreeNode* helper(TreeNode* cur)
    {
        if(cur == nullptr) return cur;
        swap(cur -> left, cur -> right);
        helper(cur -> left);
        helper(cur -> right);
        return cur;
    }
    TreeNode* invertTree(TreeNode* root) {
        TreeNode* ans = helper(root);
        return ans;
    }
```

8ms 11.13mb
```
void helper(TreeNode* cur)
    {
        if(cur == nullptr) return;
        swap(cur -> left, cur -> right);
        helper(cur -> left);
        helper(cur -> right);
    }
    TreeNode* invertTree(TreeNode* root) {
        helper(root);
        return root;
    }
```

# 101 symmetric bi tree

```
    bool compare(TreeNode* left, TreeNode* right){
        if(left == nullptr&& right == nullptr) return true;
        else if(left != nullptr&& right == nullptr) return false;
        else if(left == nullptr&& right != nullptr) return false;
        else if(left->val != right->val)return false;

        bool inside = compare(left -> right, right -> left);
        bool outside = compare(left -> left, right -> right);
        return inside&&outside;
        
    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;//always forget that!
        return compare(root->left,root->right);

    }
```