# 669 Trim bst
```
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root == nullptr) return nullptr;
        if(root -> val < low || root -> val > high){
            if(root -> left == nullptr && root -> right == nullptr){
                //delete root;
                return nullptr;
            }
            else if(root -> left == nullptr && root -> right != nullptr){
                TreeNode* node = root;
                root = root -> right;
                //delete node;
                return root;
            }
            else if(root -> left != nullptr && root -> right == nullptr){
                TreeNode* node = root;
                root = root  -> left;
                //delete node;
                return root;
            }
            else if(root -> left != nullptr && root -> right != nullptr)
            {
                TreeNode* node = root -> right;
                while(node -> left != nullptr){
                    node = node -> left;
                    if(node -> left == nullptr)
                        break;
                    }
                    node -> left = root -> left;
                    TreeNode* temp = root;
                    root = root -> left;
                    //delete temp;
                    return node;//到这里就全部返回了，没有进入到后面
                }
         }
        root -> left = trimBST(root -> left, low, high);
        root -> right = trimBST(root -> right, low, high);
        return root;
```
总是报出说删除了某些节点指针溢出的问题。
由于是二叉搜索树，所以可以直接切掉另外一边的枝条
solution：
```
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root == nullptr) return nullptr;
        if(root -> val > high){//because of BST, right tree will be bigger than the range!!!!!!!
            return trimBST(root -> left, low, high);
        }
        if(root -> val < low){
            return trimBST(root -> right, low, high);
        }
        root -> left = trimBST(root -> left, low, high);
        root -> right = trimBST(root -> right, low, high);
        return root;
    }
```