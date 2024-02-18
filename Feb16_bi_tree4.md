# 654 maximu bi tree

一开始以为是最大堆和最小堆那种，不过最后哪里解决了昨天留下的疑问

可以发现上面的代码看上去简洁一些，主要是因为第二版其实是允许空节点进入递归，所以不用在递归的时候加判断节点是否为空

第一版递归过程：（加了if判断，为了不让空节点进入递归）

```
if (maxValueIndex > 0) { // 这里加了判断是为了不让空节点进入递归
    vector<int> newVec(nums.begin(), nums.begin() + maxValueIndex);
    node->left = constructMaximumBinaryTree(newVec);
}
if (maxValueIndex < (nums.size() - 1)) { // 这里加了判断是为了不让空节点进入递归
    vector<int> newVec(nums.begin() + maxValueIndex + 1, nums.end());
    node->right = constructMaximumBinaryTree(newVec);
}
```
第二版递归过程： （如下代码就没有加if判断）
```
root->left = traversal(nums, left, maxValueIndex);

root->right = traversal(nums, maxValueIndex + 1, right);
```
第二版代码是允许空节点进入递归，所以没有加if判断，当然终止条件也要有相应的改变。

第一版终止条件，是遇到叶子节点就终止，因为空节点不会进入递归。

第二版相应的终止条件，是遇到空节点，也就是数组区间为0，就终止了。


# 617 mergeTree
本来写了一个逻辑很差的，就是类似
t1 == null && t2 == null return null;
t1 != null && t2 == null  n -> val = t1 -> val;
t1 == null && t2 != null  n -> val = t2 -> val;
t1 != null && t2 != null  n -> val = t1 -> val + t2 -> val;
但是还不如直接在原树上替换。

# 98 valid bst
wrong solution:
```
bool isValidBST(TreeNode* root) {
        //直接看该节点的左右子树是否符合
        //还需要判断左右子树是否存在
        //1. 左小，2. 右大， 3. 满足父节点条件
        bool left = true,right =true;

        if(root->left != nullptr )
        {
            if(root -> left -> val < root -> val)
                left = isValidBST(root->left);
            else
                return false;
        }
        if(root->right != nullptr)
        {
            if(root -> right -> val > root -> val)
                left = isValidBST(root->right);
            else 
                return false;
        }
        
        return left && right;
}
```
没有注意到
[5,4,6,null,null,3,7]
及有子节点不满足祖父节点的情况