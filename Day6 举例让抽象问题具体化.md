# Day6  举例让抽象问题具体化

## [剑指 Offer 30. 包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/?favorite=xb9nqhhg)

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

```cpp
class Solution {
public:
    stack<int> st, minSt;
    void push(int value) {
        st.push(value);
        if(minSt.empty()) minSt.push(value);
        else {
            // 注意这里是和最小栈的栈顶进行比较，维持栈顶是最小值
            minSt.push(std::min(value, minSt.top()));
        }
    }
    void pop() {
        if(!st.empty()) {
            st.pop();
            minSt.pop();
        } 
    }
    int top() {
        if(!st.empty()) return st.top();
        return -1;
    }
    int min() {
        if(!minSt.empty()) return minSt.top();
        return -1;
    }
};
```

```golang
type MinStack struct {
    min []int
    array []int
    size int
}


/** initialize your data structure here. */
func Constructor() MinStack {
    return MinStack{}
}


func (this *MinStack) Push(x int)  {
    if (this.min == nil || len(this.min) == 0 || x<=this.min[len(this.min)-1]) {
        this.min = append(this.min, x)
    }

    this.array = append(this.array, x)
    this.size = this.size+1
}


func (this *MinStack) Pop()  {
    if this.size == 0 {
        panic("empty")
    }

    // _ := this.array[this.size-1]
    if this.array[this.size-1] == this.min[len(this.min)-1] {
        this.min = this.min[:len(this.min)-1]
    }
    this.array = this.array[:this.size-1]
    this.size = this.size -1
}


func (this *MinStack) Top() int {
    if this.size == 0 {
        panic("empty")
    }
    top := this.array[this.size-1]
    return top
}


func (this *MinStack) Min() int {
    if this.size == 0 {
        return math.MaxInt64
    }
    return this.min[len(this.min)-1]
}


/**
 * Your MinStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * obj.Pop();
 * param_3 := obj.Top();
 * param_4 := obj.Min();
 */
```



## [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/description/)

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        // 思路：用一个栈来模拟，入栈序列依次入栈，栈顶和出栈序列匹配
        // 特殊情况
        if(pushed.size() != popped.size()) return false;
        stack<int> st;
        int j = 0;
        for(int i = 0; i < pushed.size(); i ++) {
            // 入栈
            st.push(pushed[i]);
            // 这里是循环匹配，最后i为size-1，就会一直循环匹配，匹配完了栈为空
            while(!st.empty() && st.top() == popped[j]) {
                st.pop();
                j ++;
            }
        }
        return st.empty();
    }
};
```

```golang
func validateStackSequences(pushed []int, popped []int) bool {
    stack := make([]int, 0)
    j := 0
    for _, v := range pushed {
        stack = append(stack, v)
        for len(stack)!=0 && popped[j]==stack[len(stack)-1] {
            stack = stack[:len(stack)-1]
            j++
        }
    }
    if len(stack)!=0 {
        return false
    }
    return true
    
}
```

## [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/description/?favorite=xb9nqhhg)

## [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/description/?favorite=xb9nqhhg)

## [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/description/?favorite=xb9nqhhg)

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
    vector<int> levelOrder(TreeNode* root) {
        if(root == nullptr) return {};
        vector<int> res;
        queue<TreeNode *> qu;
        qu.push(root);
        while(!qu.empty()) {
            int size = qu.size();
            while(size --) {
                auto top = qu.front();
                qu.pop();
                res.push_back(top->val);
                if(top->left) qu.push(top->left);
                if(top->right) qu.push(top->right);
            }
        }
        return res;
    }
};
```

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root == nullptr) return {};
        queue<TreeNode *> qu;
        vector<vector<int>> res;
        qu.push(root);
        while(!qu.empty()) {
            int size = qu.size();
            vector<int> level;
            while(size --) {
                auto top = qu.front();
                qu.pop();
                level.push_back(top->val);
                if(top->left) qu.push(top->left);
                if(top->right) qu.push(top->right);
            }
            res.push_back(level);
        }
        return res;
    }
};
```

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root == nullptr) return {};
        vector<vector<int>> res;
        queue<TreeNode *> qu;
        qu.push(root);
        bool flag = true;
        while(!qu.empty()) {
            int size = qu.size();
            vector<int> level;
            flag = !flag;
            while(size --) {
                auto top = qu.front();
                qu.pop();
                level.push_back(top->val);
                if(top->left) qu.push(top->left);
                if(top->right) qu.push(top->right);
            }
                
            if(flag) reverse(level.begin(), level.end());
            res.push_back(level);
        }
        return res;
    }
};
```

## [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/description/)

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        // 特殊情况
        if(postorder.size() == 0) return true;
        int size = postorder.size();
        cout << "size " << size << endl;
        // 最后一个元素为根节点
        int tmp = postorder[size - 1];
        int i = 0;
        vector<int> left, right;
        // 左边都是小于根节点的，找到第一个不小于的，那么之后都是大于的
        // 不能包括最后一个根节点！！！
        for(; i < size - 1; i ++ ) {
            if(postorder[i] > tmp) break;
            else left.push_back(postorder[i]);

        }
        // 后面如果有不大于的 就肯定不是二叉搜索树
        // 不能包括最后一个根节点
        for(; i < size - 1; i ++ ) {
            if(postorder[i] < tmp) return false;
            else right.push_back(postorder[i]);
            cout << "right = " << postorder[i] << endl;
        }
        cout << "i " << i << endl;
        // 遍历完毕
        // 这个条件必须放到最后，和另外两个同时成立
        // if(i = size - 1) return true;
        // 递归左右两段
        return i == size - 1 && verifyPostorder(left) && verifyPostorder(right);
    }
};
```

```golang
func verifyPostorder(postorder []int) bool {
    if len(postorder)==0 {
        return true
    }
    //递归 子问题
    i := 0
    for ; i<len(postorder)-1; i++ {
        if postorder[i]>postorder[len(postorder)-1] {
            break
        }
    }
    fmt.Println(i)
    
    for j:=i; j<len(postorder)-1; j++ {
        if postorder[j]<postorder[len(postorder)-1] {
            return false
        }
    }
    return verifyPostorder(postorder[:i]) && verifyPostorder(postorder[i:len(postorder)-1])
}
```

## [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode.cn/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/description/)

给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        // 思路：前序遍历 到根节点的时候 判断是否为target，然后回溯
        if(root == nullptr) return {};
        vector<vector<int>> res;
        vector<int> tmp;
        dfs(root, target, tmp, res);
        return res;
    }
    void dfs(TreeNode * root, int target, vector<int> &tmp, vector<vector<int>> &res) {
        if(root == nullptr) return;
        // 选择
        tmp.push_back(root->val);
        target -= root->val;
        if(target == 0 && root->left == nullptr && root->right == nullptr) res.push_back(tmp);
        // 回溯
        dfs(root->left, target, tmp, res);
        dfs(root->right, target, tmp, res);
        // 撤销选择
        tmp.pop_back();

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
func pathSum(root *TreeNode, target int) [][]int {
    res := make([][]int, 0)
    list := make([]int, 0)
    traval(root, target, list, &res)
    return res
}

func traval(root *TreeNode, target int, list []int, res *[][]int) {
    if root==nil {
        return 
    }
    //选择
    list = append(list, root.Val)
    target -= root.Val
    //满足 条件
    if target==0 && root.Left==nil && root.Right==nil {
        ans := make([]int, len(list))
        copy(ans, list)
        *res = append(*res, ans)
    }
    //回溯
    traval(root.Left, target, list, res)
    traval(root.Right, target, list, res)
    //撤销选择
    list = list[:len(list)-1]
}
```

