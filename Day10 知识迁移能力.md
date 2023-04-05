# Day10 知识迁移能力

## [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/description/)

统计一个数字在排序数组中出现的次数。

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.size() == 0) return 0;
        int cnt = 0;
        for(auto num : nums) {
            if(num == target) cnt ++;
        }
        return cnt;
    }
};
```

## [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/description/?favorite=xb9nqhhg)

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        for(int i = 0; i < nums.size(); i ++ ) {
            if(i != nums[i]) return i;
        }
        return nums.size();
    }
};
```

```golang
func missingNumber(nums []int) int {
    size := len(nums)
    for i:=0; i<size; i++ {
        if nums[i] != i {
            return i
        }
    }
    return size
}
```



## [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/description/)	

给定一棵二叉搜索树，请找出其中第 `k` 大的节点的值。

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
    int cnt = 1;
    int kthLargest(TreeNode* root, int k) {
        if(root == nullptr) return -1;
        int res = 0;
        // int a = k;
        helper(root, k, res);
        return res;
    }
    void helper(TreeNode* root, int& k, int &res) {
        if(root == nullptr) return ;
        helper(root->right, k, res);
        cout << k << endl;
        if(k == 1) {
            // cout << root->val << endl;
            res = root->val; 
            k --;
            return ;
        } 
        k --;

        helper(root->left, k, res);
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
func kthLargest(root *TreeNode, k int) int {
    //遍历二叉树 
    arr := make([]int, 0)
    find(root, &arr)
    fmt.Println(arr)
    return arr[len(arr)-k]
}
//中序遍历 将树的值 从小到大排序
func find(root *TreeNode, arr *[]int){
    if root==nil {
        return 
    }
    find(root.Left, arr)
    *arr = append(*arr, root.Val)
    find(root.Right, arr)
}
```

## [剑指 Offer 55 - I. 二叉树的深度](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/description/?favorite=xb9nqhhg)

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
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        return 1 + max(left, right);
    }
};
```

## [剑指 Offer 55 - II. 平衡二叉树](https://leetcode.cn/problems/ping-heng-er-cha-shu-lcof/description/?favorite=xb9nqhhg)

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

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
    bool isBalanced(TreeNode* root) {
        if(root == nullptr) return true;
        return abs(maxDepth(root->left) - maxDepth(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right); 
    }
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

## [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/description/)

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

```cpp
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        // 思路：先将所有的数 异或 结果就是 只出现一次的那两个数 异或的结果
        // 异或不为0 说明两个数的二进制位肯定有一位 是0和1，这样得到的异或值二进制这位是 1
        // 根据这个位 可以将所有的数分为两半
        // 一半是这位为0的，一半是这位为1的，另外相等的数一定会出现在同一边
        // 这样就化为了两个集合，结果那两个数分别在两个集合中，集合异或即可 得到那个数
        if(nums.size() <= 0) return {};
        int sum = 0;  // 所有数的异或
        for(auto num : nums) sum ^= num;
        
        //通过移位 找到为 1 的那一位
        int isOne = 0;
        // !的优先级比 & 高，注意加括号
        while( ! ((sum >> isOne) & 1) )  isOne ++; 
        cout << sum << ' ' << isOne;
        // 通过这一位分半
        int left = 0;
        for(auto num : nums) {
            if( (num >> isOne ) & 1) left ^= num;
        }
        // 得到的left就是其中一个值，和sum异或得到的就是另一个值
        return vector<int>{left, left ^ sum};
    }
};
```

## [剑指 Offer 57. 和为s的两个数字](https://leetcode.cn/problems/he-wei-sde-liang-ge-shu-zi-lcof/description/)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        unordered_map<int, bool> umap;
        for(auto num : nums) {
            if(umap.find(target - num) != umap.end()) return {num, target-num};
            umap[num] = true;
        }
        return {};
    }
};
```

## [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/description/)

```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        // 滑动窗口
        vector<vector<int>> res;
        int left = 1, right = 2, sum = 0;
        while(left < right) {
            int sum = (right + left) * (right - left + 1) / 2;
            if(sum == target) {
                vector<int> tmp;
                for(int i = left; i <= right; i ++ ) {
                    tmp.push_back(i);
                }
                res.push_back(tmp);
                left ++;
            }else if(sum < target) {
                right ++;
            }else {
                left ++;
            }
        }
        return res;
    }
};
```

## [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/description/?favorite=xb9nqhhg)

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string res;
        for(int i = n; i < s.size(); i ++ ) res.push_back(s[i]);
        for(int i = 0; i < n; i ++ ) res.push_back(s[i]);
        return res;
    }
};
```

## [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/description/?favorite=xb9nqhhg)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> qu;
        vector<int> res;
        for(int i = 0; i < nums.size(); i ++ ) {
            // 最大值已经不在窗口了
            if(!qu.empty() && qu.front() + k <= i ) qu.pop_front();
            // 加入队列中
            // 维持一个单调 递减 队列
            while(!qu.empty() && nums[qu.back()] <= nums[i]) qu.pop_back();
            qu.push_back(i);
            // 将队头也就是最大值加入到结果中
            if(i >= k - 1) res.push_back(nums[qu.front()]);
        }
        return res;
    }
};
```

```golang
func maxSlidingWindow(nums []int, k int) []int {
    res := make([]int, 0)
    tmp := make([]int, 0)
    for i := 0; i < len(nums); i ++ {
        if len(tmp) != 0 && tmp[0] + k <= i {
            tmp = tmp[1:]
        }
        for (len(tmp) != 0 && nums[tmp[len(tmp) - 1]] < nums[i]) {
            tmp = tmp[:len(tmp) - 1]
        }
        tmp = append(tmp, i)
        if i >= k - 1 {
            res = append(res, nums[tmp[0]])
        }
    }
    return res;
}
```

