# 数组

## [剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        unordered_map<int, int> myMap;
        for(auto num : nums) {
            myMap[num] ++;
            if(myMap[num] > 1) return num;
        }
        return -1;
        
    }
};
```

```golang
func findRepeatNumber(nums []int) int {
    //哈希表
    hashtable := make(map[int]bool, 0)
    for _, v := range nums {
        if hashtable[v] {
            return v
        }
        //加入哈希表
        hashtable[v] = true
    }
    return -1
}
```

## [剑指 Offer 04. 二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

在一个 n * m 的二维数组中，每一行都按照从左到右 非递减 的顺序排序，每一列都按照从上到下 非递减 的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```cpp
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0) return false;
        // 思路
        int j = matrix[0].size() - 1, i = 0;
        while(j >= 0 && i < matrix.size()) {
            if(target == matrix[i][j]) return true;
            else if(target > matrix[i][j]) i ++;
            else j --;
        }
        return false;
    }
};
```

```golang
func findNumberIn2DArray(matrix [][]int, target int) bool {
    if len(matrix)==0 {
        return false
    }
    //类似于 二叉搜索树
    i, j := 0, len(matrix[0])-1
    //将最 右上角 元素 作为“根节点”
    for i<len(matrix) && j>=0 {
        if target==matrix[i][j] {
            return true
        }else if target>matrix[i][j] { 
            i++
        }else {
            j--
        }
    }
    return false
}
```

# 字符串

## [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        string res;
        for(int i = 0; i < s.size(); i ++ ) {
            if(s[i] == ' ') res += "%20";
            else res.push_back(s[i]);
        }
        return res;
    }
};
```

```golang
func replaceSpace(s string) string {
    var str string
    //遍历 如果是空格 替换
    for _, v := range s {
        if v==' ' {
            str += "%20"
        }else {
            str += string(v)
        }
    }
    return str
}
```

# 链表

#### [剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        if(head == nullptr) return {};
        vector<int> res;
        ListNode * cur = head;
        while(cur != nullptr) {
            res.push_back(cur->val);
            cur = cur->next;
        }
        for(int i = 0, j = res.size() - 1; i < j; i ++, j --) {
            swap(res[i], res[j]);
        }
        return res;
    }
};
```

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) []int {
    result := make([]int, 0)
    if head==nil {
        return nil
    }
    for head!=nil {
        result = append(result, head.Val)
        head = head.Next
    }
    for i,j := 0,len(result)-1; i<j; i,j = i+1,j-1 {
        result[i], result[j] = result[j], result[i]
    }
    return result
}
```

# 树

## [剑指 Offer 07. 重建二叉树](https://leetcode.cn/problems/zhong-jian-er-cha-shu-lcof/)

输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。

假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size() == 0) return nullptr;
        // 确定根节点
        TreeNode * root = new TreeNode(preorder[0]);
        root->left = root->right = nullptr;
        // 在中序中找到根节点的位置
        int gen = 0;
        for( ; gen < inorder.size(); gen ++) {
            if(inorder[gen] == root->val) break;
        }
        vector<int> inLeft, inRight;
        vector<int> preLeft, preRight;
        // 左边是左子树 [0, gen - 1] 右边是右子树 
        for(int i = 0; i < gen; i ++ ) {
            inLeft.push_back(inorder[i]);
            preLeft.push_back(preorder[i + 1]);	// 第一个为根节点，所以i + 1
        }

        for(int i = gen + 1; i < inorder.size(); i ++ ) {
            inRight.push_back(inorder[i]);
            preRight.push_back(preorder[i]);
        }

        // 递归
        root->left = buildTree(preLeft, inLeft);
        root->right = buildTree(preRight, inRight);
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
func buildTree(preorder []int, inorder []int) *TreeNode {
    //递归 
    //边界条件
    if len(preorder)==0 {
        return nil
    }
    //根节点 为前序遍历的 第一个元素
    root := &TreeNode{preorder[0], nil, nil}
    //遍历 中序遍历结果 得到索引值
    i := 0
    for ;i<len(inorder); i++ {
        //等于根节点值
        if inorder[i] == preorder[0] {
            break
        }
    }
    //递归得到左右子树

    //找到索引 索引左边的元素长度 即为左子树长度 映射到前序遍历结果
    //索引右边 右子树
    //左子树长度加一
    root.Left = buildTree(preorder[1:i+1], inorder[:i])
    root.Right = buildTree(preorder[i+1:], inorder[i+1:])
    return root
}
```



# 栈和队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```cpp
class CQueue {
public:
    stack<int> stackIn;
    stack<int> stackOut;
    CQueue() {

    }
    
    void appendTail(int value) {
        stackIn.push(value);
    }
    
    int deleteHead() {
        if(stackOut.empty()) {
            if(stackIn.empty()) return -1;
            else {
                while(!stackIn.empty()) {
                    stackOut.push(stackIn.top());
                    stackIn.pop();
                }
            }
        }
        int res = stackOut.top();
        stackOut.pop();
        return res;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

```golang
type CQueue struct {
    inStack, outStack []int 
}


func Constructor() CQueue {
    return CQueue{}
}


func (this *CQueue) AppendTail(value int)  {
    //inStack栈用来 入队元素
    this.inStack= append(this.inStack, value)
}


func (this *CQueue) DeleteHead() int {
    //将inStack的元素逆序 放到outStack中
    if len(this.outStack)==0 {
        //还没有元素
        if len(this.inStack)==0 {
            return -1
        }
        for len(this.inStack)>0 {
            this.outStack = append(this.outStack, this.inStack[len(this.inStack)-1])
            this.inStack = this.inStack[:len(this.inStack)-1]
        }
    }
    top := this.outStack[len(this.outStack)-1]
    this.outStack = this.outStack[:len(this.outStack)-1]
    return top
}


/**
 * Your CQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AppendTail(value);
 * param_2 := obj.DeleteHead();
 */
```

