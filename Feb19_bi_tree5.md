# 235 bst lowest common ancessor
一开始我就在想是否在区间中就是最小公共的，但是似乎没有想到证实的办法。
其实试一试，比空想是否能够成功更为重要。
```
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        int bigNodeVal = p->val > q->val? p->val:q->val;
        int smallNodeVal = q->val < p->val? q->val:p->val;
        while(root != nullptr){
            if(root->val <= bigNodeVal && root->val >= smallNodeVal) return root;
            if(root->val > bigNodeVal) root = root -> left;
            if(root->val < smallNodeVal) root = root -> right;
        }
        return root;
    }
```

# 450 Delete Node in BST
前序遍历一次，中序遍历一次，删除节点之后重新构建树。
1. 自己为空
2. 左右为空
3. 左右有一不为空
4. 左右都不为空：将左子树接到右子树的最左边 