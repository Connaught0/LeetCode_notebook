# C++
C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树，所以map、set的增删操作时间时间复杂度是logn，注意我这里没有说unordered_map、unordered_set，unordered_map、unordered_set底层实现是哈希表。

# 94 preorder travel
solution 1： 朴素迭代法
```
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> ans;
        TreeNode* point = root;
        while(point != NULL || !st.empty())
        {
            if(point != NULL)
            {
                st.push(point);
                point = point -> left;
            }
            else
            { 
                point = st.top();
                ans.push_back(point -> val);
                st.pop();
                point = point -> right;
            }
        }
        return ans;
    }
```
solution 2: 统一迭代法
```
vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> ans;
        if(root!=NULL)st.push(root);
        while(!st.empty())
        {
            TreeNode* point = st.top();
            if(point != NULL)
            {
                st.pop();
                if(point->right)st.push(point->right);
                st.push(point);
                st.push(NULL);
                //给一个我应该访问当前位置数据的标记，
                //也就是在中间的时候我是应该访问了，
                //因此就是NULL在那里就是该访问数据的时候
                if(point->left)st.push(point->left);
            }
            else
            {
                st.pop();
                point = st.top();
                st.pop();
                ans.push_back(point->val);
            }
        }
        return ans;
    }

