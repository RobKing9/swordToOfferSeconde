# Day7  分解让问题简单化



## [剑指 Offer 35. 复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/description/?favorite=xb9nqhhg)

请实现 `copyRandomList` 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 `next` 指针指向下一个节点，还有一个 `random` 指针指向链表中的任意节点或者 `null`。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        // 使用虚拟头结点开始复制
        // 首先复制next节点，用一个哈希表保存复制前后节点的关系
        unordered_map<Node *, Node *> myMap;
        Node * res = new Node(-1);
        Node * dummy = res;
        // 复制next节点
        for(auto cur = head; cur; cur = cur->next) {
            // 通过值创建新的节点
            dummy->next = new Node(cur->val);
            // 移动
            dummy = dummy->next;
            // 记录映射关系
            // cout << "cur " << cur->val << endl;
            myMap[cur] = dummy;
        }
        // for(auto m :myMap) {
        //     cout << "head = " << m.first->val << " " << "dummy = " << m.second->val << endl;
        // }
        dummy = res->next;
        for(auto cur = head; cur; cur = cur->next) {
            // 通过之前的映射关系，找到 复制之后的 random指针
            // 如果不映射，那么cur->random 指向的就是原来的内存
            dummy->random = myMap[cur->random];
            dummy = dummy->next;
        }
            
        
        return res->next;
    }
};
```

```cpp
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */

func copyRandomList(head *Node) *Node {
    //哈希表
    if head==nil {
        return nil
    }
    hashtable := make(map[*Node]*Node)
    //先将值 复制过来   保证 random 有指向
    for cur:=head; cur!=nil; cur=cur.Next {
        hashtable[cur] = &Node{Val:cur.Val}
    }
    //复制 next和random 
    for cur:=head; cur!=nil; cur=cur.Next {
        hashtable[cur].Next = hashtable[cur.Next]
        hashtable[cur].Random = hashtable[cur.Random]
    }
    return hashtable[head]
}
```



## [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/description/?favorite=xb9nqhhg)

输入一棵二叉搜索树，将该二叉搜索树转换成一个**排序**的**循环双向**链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if(root == nullptr) return nullptr;
        // 思路: 中序遍历 每次返回 左端和右端
        auto sides = dfs(root);
        // 循环
        sides.first->left = sides.second, sides.second->right = sides.first;
        return sides.first;
    }
    pair<Node*, Node*> dfs(Node * root) {
        if(root->left == nullptr && root->right == nullptr) return {root, root};
        // 左右子树都不为空
        if(root->left && root->right) {
            // 递归左边和右边
            auto lsides = dfs(root->left), rsides = dfs(root->right);
            cout << lsides.second->val << " " << rsides.first->val << endl;
            // 左边的 最右边那个节点 和 根节点 建立联系
            lsides.second->right = root, root->left = lsides.second;
            // 右边的 最左边那个节点 和 根节点 建立联系
            rsides.first->left = root, root->right = rsides.first;
            return {lsides.first, rsides.second};
        }
        // 右不空 左为空
        if(root->right) {
            // 递归 右边
            auto rsides = dfs(root->right);
            // 右边的 最左边那个节点 和 根节点 建立联系
            rsides.first->left = root, root->right = rsides.first;
            return {root, rsides.second};
        }
        // 左不空 右为空
        if(root->left) {
            // 递归 左边 
            auto lsides = dfs(root->left) ;
            // 左边的 最右边那个节点 和 根节点 建立联系
            lsides.second->right = root, root->left = lsides.second; 
            return {lsides.first, root};
        }
        // 执行不到
        return {};
    }
};
```



## [剑指 Offer 37. 序列化二叉树](https://leetcode.cn/problems/xu-lie-hua-er-cha-shu-lcof/description/?favorite=xb9nqhhg)

请实现两个函数，分别用来序列化和反序列化二叉树。

你需要设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if(root == nullptr) return "";
        string res;
        encode(root, res);
        return res;
    }
    void encode(TreeNode* root, string &res) {
        if(root == nullptr) {
            res += "# ";
            return;
        } 
        // 前序遍历
        res += to_string(root->val) + " ";
        encode(root->left, res);
        encode(root->right, res);
        return;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data.size() == 0) return nullptr;
        int u = 0;
        return decode(data, u);

    }

    TreeNode * decode(string &data, int &u) {
        if(data.size() == 0) return nullptr;
        int k = u;
        // 用来记录这个数字的个位的位置 的后一个空格
        while(data[k] != ' ') k ++;
        // 说明为空指针
        if(data[u] == '#') {
            u = k + 1;
            return nullptr;
        }
        // 数据
        int val = 0;
        // 判断是不是有负号
        bool flag = false;
        if(data[u] == '-') {
            flag = true;
            u += 1;
        }
        for(int i = u; i < k; i ++ ) val = val * 10 + data[i] - '0';
        // 跳过这个数字
        u = k + 1;
        if (flag) val = -val;
        auto root = new TreeNode(val);
        root->left = decode(data, u);
        root->right = decode(data, u);
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

## [剑指 Offer 38. 字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/description/)

输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

```cpp
class Solution {
public:
    vector<string> res;
    vector<bool> used;
    vector<string> permutation(string s) {
        string tmp;
        used.resize(s.size());
        // 排序 为了让重复的元素在一起
        sort(s.begin(), s.end());
        backTrace(s, 0, tmp);
        return res;
    }

    // 回溯
    void backTrace(string s, int u, string &tmp) {
        // 填充到最后一个了
        if(u == s.size()) {
            res.push_back(tmp);
            return;
        }
        // 每一次填充一个空位
        for(int i = 0; i < s.size(); i ++ ) {
            // 1 是否使用过 
            // 2 !used[i - 1] 表示还没用 s[i] = s[i - 1] 表示和前面元素相同，说明和前面的情况是一样的
            // 这两种情况需要剪枝
            if(used[i] || (i > 0 && !used[i - 1] && s[i] == s[i - 1])) {
                continue;
            }
            used[i] = true;
            tmp.push_back(s[i]);
            backTrace(s, u + 1, tmp);
            used[i] = false;
            tmp.pop_back();
        }
    }
};
```

