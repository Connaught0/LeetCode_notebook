# 104 max depth of bi tree
solution 1: 
```
    int helper(TreeNode* node, int lastNum)//输入
    {
        if(node == nullptr)
            return lastNum;//终止条件
        else
            lastNum++;
            int left=0;
            int right=0;
            if(node -> left) left = helper(node -> left, lastNum);
            if(node -> right) right = helper(node -> right, lastNum);
            return max(max(left,right),lastNum);
    }
    int maxDepth(TreeNode* root) {
        
        if(root == nullptr) return 0;
        int maxDep = 1;
        int ans =0;
        ans = helper(root, 0);
        return ans;
    }
```
挺无语的，一开始left和right写错了
solution 2：
```
    int maxDepth(TreeNode* root) {
        
        if(root == nullptr) return 0;
        int ans = 0;
        queue<TreeNode*> nodeQue;
        nodeQue.push(root);
        while(!nodeQue.empty())
        {
            int size = nodeQue.size();
            for(int i = 0; i < size; i++)
            {   
                TreeNode* point = nodeQue.front();
                nodeQue.pop();
                if(point -> left)nodeQue.push(point -> left);
                if(point -> right)nodeQue.push(point -> right);
            }
            ans++;
        }
        return ans;
    }
```
# 111 minDepth of bi tree
solution 1:
```
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        int ans = 0;
        queue<TreeNode*> nodeQue;
        nodeQue.push(root);
        bool meetLeaf = false;//这还蛮好理解的
        while(!nodeQue.empty())
        {
            int size = nodeQue.size();
            for(int i = 0; i < size; i++)
            {   
                TreeNode* point = nodeQue.front();
                nodeQue.pop();
                if(point -> left == nullptr && point -> right == nullptr)
                    meetLeaf = true;
                if(point -> left)nodeQue.push(point -> left);
                if(point -> right)nodeQue.push(point -> right);
            }
            ans++;
            if(meetLeaf)
            {
                return ans;
            }
        }
        return ans;
    }
```
没啥难度，我就觉得容易犯写min的问题，后来没写就还好
solution 2: 前序
```
    int helper(TreeNode* node, int lastNum)
    {
        if(node == nullptr) return lastNum;
        if(node->left == nullptr&&node->right == nullptr)
            return lastNum+1;
        lastNum++;
        int left = INT_MAX;
        int right = INT_MAX;
        if(node->left) left = helper(node->left, lastNum);
        if(node->right) right = helper(node->right, lastNum);
        return min(left,right);
    }
    int minDepth(TreeNode* root) {
        return helper(root, 0);
    }
```