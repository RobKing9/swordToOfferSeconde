# Day4 代码的鲁棒性



## [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/description/?favorite=xb9nqhhg)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。

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
    ListNode* getKthFromEnd(ListNode* head, int k) {
        if(head == nullptr) return nullptr;
        if(k <= 0) return nullptr;
        // 双指针
        ListNode * cur = head;
        while(k --)  cur = cur->next;

        ListNode * res = head;
        while(cur) {
            res = res->next;
            cur = cur->next;
        } 

        return res;
    }
};
```

## [剑指 Offer 24. 反转链表](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/description/)

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
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr) return nullptr;
        ListNode * cur = head;
        ListNode * pre = nullptr;
        while(cur) {
            ListNode * next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
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
func reverseList(head *ListNode) *ListNode {
    if head==nil {
        return nil
    }
    var pre *ListNode
    //遍历
    for head!=nil {
        //记录next
        next := head.Next
        //改变一条边
        head.Next = pre
        //pre和head向后移动一位
        pre = head
        head = next
    }
    //当head为空时，返回pre
    return pre
}
```



## [剑指 Offer 25. 合并两个排序的链表](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/description/)

```
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == nullptr) return l2;
        if(l2 == nullptr) return l1;
        ListNode * cur = new ListNode(-1);
        ListNode * l3 = cur;
        while(l1 && l2) {
            // 比较大小
            if(l1->val <= l2->val) {
                l3->next = l1;
                l1 = l1->next;
            }else {
                l3->next = l2;
                l2 = l2->next;
            }
            // 不管谁大都要移动l3
            l3 = l3->next;
        }
        // 注意这里是if而不是while
        if(l1 != nullptr) l3->next = l1;
        if(l2 != nullptr) l3->next = l2;
        return cur->next;
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
func mergeTwoLists(l1 *ListNode, l2 *ListNode)  *ListNode {
   if l1 == nil {
       return l2
   }
   if l2 == nil {
       return l1
   }
   if l1.Val>l2.Val {
       l1, l2 = l2, l1
   }
   l1.Next = mergeTwoLists(l1.Next, l2)
   return l1
}
```

## [剑指 Offer 26. 树的子结构](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/description/)

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

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
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(A == nullptr || B == nullptr) return false;
        // 前面两个表示在左右子树任何一棵找到
        return isSubStructure(A->left, B) || isSubStructure(A->right, B) || isSub(A, B);
    }
    bool isSub(TreeNode* A, TreeNode* B) {
        // 已经全部找完了
        if(B == nullptr) return true;
        // A到根节点了，但是B还没有  或者 此时不匹配
        if(A == nullptr || A->val != B->val) return false;
        // 这个时候说明根节点的值相等了，那么同时要满足左右继续相等，这个时候都往下移动
        // 同时满足就是 &&
        return isSub(A->left, B->left) && isSub(A->right, B->right);
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
func isSubStructure(A *TreeNode, B *TreeNode) bool {
    if A==nil || B==nil {
        return false
    }
    //可能 B根 或者左 或者右 和 B的根相等
    return isSub(A, B) || isSubStructure(A.Left, B) || isSubStructure(A.Right, B)
}

func isSub(A *TreeNode, B *TreeNode) bool {
    //递归终止条件
    if B==nil {
        return true
    }
    if A==nil || A.Val!=B.Val{
        return false
    }
    return isSub(A.Left, B.Left) && isSub(A.Right, B.Right)
}
```

