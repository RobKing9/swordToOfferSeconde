# Day5 解决面试题的思路

## [剑指 Offer 27. 二叉树的镜像](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/description/?favorite=xb9nqhhg)

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        // 思路：翻转 左右节点，然后左右继续翻转他的左右节点
        if(root == nullptr) return nullptr;
        auto tmp = root->left;
        root->left = root->right;
        root->right = tmp;
        // swap(root->le)
        mirrorTree(root->left);
        mirrorTree(root->right);
        return root;
    }
};
```

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func mirrorTree(root *TreeNode) *TreeNode {
    if root==nil {
        return nil
    }
    mirrorTree(root.Left)
    mirrorTree(root.Right)
    root.Left, root.Right = root.Right, root.Left
    return root
}
```

## [剑指 Offer 28. 对称的二叉树](https://leetcode.cn/problems/dui-cheng-de-er-cha-shu-lcof/description/)

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        // 思路：空返回true。
        // 左子树的左边和右子树的右边 并且 左子树的右边和右子树的左边 必须相等
        if(root == nullptr) return true;
        // if(root->left != root->right) return false;
        return isSym(root->left, root->right);
    }
    bool isSym(TreeNode* left, TreeNode * right) {
        if(left == nullptr && right == nullptr) return true;
        if(left == nullptr || right == nullptr) return false;
        // 只要判断左右是否相等 递归之后就是  左子树的左边和右子树的右边 并且 左子树的右边和右子树的左边 
        if(left->val != right->val) return false;
        return isSym(left->left, right->right) && isSym(right->left, left->right);
    }
};
```

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSymmetric(root *TreeNode) bool {
    //判断 左右两边是否 对称
    if root==nil {
        return true
    }
    return LR(root.Left, root.Right)
}

func LR(left, right *TreeNode) bool {
    //递归终止条件
    if left==nil && right==nil {
        return true
    }
    if left==nil || right==nil {
        return false
    }
    //本质上 是判断 值是否相等
    return left.Val==right.Val && LR(left.Left, right.Right) && LR(left.Right, right.Left)
}
```

## [剑指 Offer 29. 顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/description/?favorite=xb9nqhhg)

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```golang
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if(matrix.size() == 0) return {};
        // 思路：方向数组确定位置，用一个数组记录是否访问过这个位置
        int m = matrix.size(), n = matrix[0].size();
        vector<int> res;
        vector<vector<int>> used(m, vector<int>(n, 0));
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        int x = 0, y = 0, d = 0;
        // 注意循环的方式
        for(int i = 0; i < m * n; i ++) {
            // 先加入到结果
            res.push_back(matrix[x][y]);
            used[x][y] = 1;
            // 下一个位置！
            int a = x + dx[d], b = y + dy[d];
            // 判断下一个位置的合法性，注意used要写到最后！！
            if(a < 0 || a >= m || b < 0 || b >= n || used[a][b]) {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            x = a, y = b;
        }
        return res;
    }
};
```

