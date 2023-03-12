# Day3 代码的完整性



## [剑指 Offer 16. 数值的整数次方](https://leetcode.cn/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/?favorite=xb9nqhhg)

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数（即，xn）。不得使用库函数，同时不需要考虑大数问题。

时间复杂度为 O(n)

```cpp
class Solution {
public:
    double Power(double base, int exponent) {
        if(exponent == 0) return 1.0;
        double res = 1;
        // 特殊情况
        if(exponent < 0) return 1 / Power(base, abs(exponent));

        for(int i = 0; i < exponent; i ++ ) {
            res *= base;
        }

        return res;
    }
};
```

时间复杂度为 O(logn) 递归

```cpp
class Solution {
public:
    double Power(double base, int exponent) {
        if(exponent == 0) return 1.0;
        if(exponent < 0) return 1 / Power(base, abs(exponent));

        double tmp = Power(base, exponent / 2);
        if(exponent % 2 == 1) return base * tmp * tmp;
        else return tmp * tmp;
    }
};
```

## [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode.cn/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/description/?favorite=xb9nqhhg)

输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

```cpp
class Solution {
public:
    vector<int> printNumbers(int n) {
        vector<int> res;
        long long tmp = 1;
        for(int i = 0; i < n; i ++ ) tmp *= 10;
        for(int i = 1; i < tmp; i ++ ) res.push_back(i);

        return res;
    }
};
```

## [剑指 Offer 18. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/?favorite=xb9nqhhg)

```cpp
/**
 * struct ListNode {
 *	int val;
 *	struct ListNode *next;
 *	ListNode(int x) : val(x), next(nullptr) {}
 * };
 */
#include <clocale>
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @param val int整型 
     * @return ListNode类
     */
    ListNode* deleteNode(ListNode* head, int val) {
        // write code here
        if(head == nullptr) return nullptr;

        ListNode * dummy = new ListNode(-1);
        dummy->next = head;
        ListNode * cur = dummy;
        while(cur->next) {
            // 删除
            while(cur->next && cur->next->val == val) {
                cur->next = cur->next->next;
            }
            cur = cur->next;
        }
        return dummy->next;
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
func deleteNode(head *ListNode, val int) *ListNode {
    //特殊情况 head为nil
    if head==nil {
        return nil
    }
    //第一个节点可能被删除 所以需要借助哑巴结点
    dummy := &ListNode{}
    dummy.Next = head
    head = dummy
    for dummy!=nil {
        //找到和val相等的结点
        if dummy.Next!=nil && dummy.Next.Val == val  {
            dummy.Next = dummy.Next.Next
        }
        dummy = dummy.Next
    }
    return head.Next
}
```



## [剑指 Offer 19. 正则表达式匹配](https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/description/)

请实现一个函数用来匹配包含`'. '`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。

```cpp
class Solution {
public:
		/*
            dp五部曲:(参考最高赞的思路)
            设原始串s的长度为m,模式串p的长度为n
            注意定义:'*'表示它前面的(一个)字符可以出现任意次（含0次）!注意是一个
            1.状态定义:设dp[i][j]为考虑s[0,i-1],p[0,j-1]时,是否能匹配上,匹配上就为true
            2.状态转移:
                2.1 p[j-1]为非'*'
                    2.1.1 若p[j-1]==s[i-1](必定为'a'-'z'),继续看dp[i-1][j-1]
                    2.1.2 若p[j-1]为'.',直接看dp[i-1][j-1]
                2.2 p[j-1]为'*'
                    2.2.1 若p[j-2]==s[i-1](必定为'a'-'z'),则继续向前看dp[i-1][j]
                        因为对于p[0,j-1]来说,s[i-1]是冗余匹配可以由p[j-2]*补充
                    2.2.2 p[j-2]为'.',则'.'匹配了s[i-1],可以继续往前看dp[i-1][j]
                        注意这里是".*"的情形,也就是"万能串",生成"......"可以匹配任何非空s
                    2.2.3 此时p[j-2]为'a'-'z',且p[j-2]!=s[i-1],"p[j-2]*"直接废掉,看dp[i][j-2]
                其中2.1.1和2.1.2可以合并为一种情形;2.2.1和2.2.2可以合并为一种情形
            3.初始化:
                3.1 空的p 
                    3.1.1 可以匹配空的s,dp[0][0]=true
                    3.1.2 不可以匹配非空的s,dp[i][0]=false,i∈[1,m-1]
                3.2 空的s
                    3.2.1 可以匹配空的s,dp[0][0]=true
                    3.2.2 可能可以匹配非空的p,要经过计算如"a*b*c*"
                3.3 非空的p与非空的s,要经过计算
            4.遍历顺序:显然是正序遍历
            5.返回形式:直接返回dp[m][n]就表示考虑s[0,m-1],j[0,n-1]是否能匹配上
        */
    bool isMatch(string s, string p) {
        
        const int m = s.size();
        const int n = p.size();
        if(!m && !n) return true;
        if(!n && n) return false;
         // dp 表示字符串 s 的前 i 个字符和字符串 p 的前j 个字符是否匹配
        // 那么答案就是 dp[m][n]
        bool dp[m + 1][n + 1];
        memset(dp, false, sizeof dp);
        dp[0][0] = true;

        // 空的s 匹配 空的p，可能匹配不空的p 需要计算(a*b*)
        // 空的p 匹配 空的s， 不可以匹配非空的s
        for(int i = 1; i <= m; i ++ ) dp[i][0] = false;

        for(int i = 0; i <= m; i ++ ) {
            for(int j = 1; j <= n; j ++ ) {
                // p[j - 1] 不是 *
                if(p[j - 1] != '*')  {
                    if(i > 0 && (p[j - 1] == s[i - 1] || p[j - 1] == '.')){
                        // p与s都向前一位匹配
                        dp[i][j] = dp[i - 1][j - 1];
                    }
                }else {
                    // p[j - 1] = '*' 看*前面的和 s[i-1]比较
                    if(i > 0 && j > 1 && (s[i - 1] == p[j - 2] || p[j - 2] == '.')) {
                        // p不动，s向前移动一位
                        dp[i][j] = dp[i - 1][j];
                    }
                     // 这里用|=的原因是:2.2中满足任意一种情形就可以判定dp[i][j]=true
                    if(j > 1) dp[i][j] |= dp[i][j - 2];
                }
            }
        }

        return dp[m][n];

    }
};
```

[参考链接](https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solutions/521347/zheng-ze-biao-da-shi-pi-pei-by-leetcode-s3jgn/)

## [剑指 Offer 20. 表示数值的字符串](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/description/?favorite=xb9nqhhg)

请实现一个函数用来判断字符串是否表示**数值**（包括整数和小数）。

**数值**（按顺序）可以分成以下几个部分：

1. 若干空格
2. 一个 **小数** 或者 **整数**
3. （可选）一个 `'e'` 或 `'E'` ，后面跟着一个 **整数**
4. 若干空格

**小数**（按顺序）可以分成以下几个部分：

1. （可选）一个符号字符（`'+'` 或 `'-'`）
2. 下述格式之一：
   1. 至少一位数字，后面跟着一个点 `'.'`
   2. 至少一位数字，后面跟着一个点 `'.'` ，后面再跟着至少一位数字
   3. 一个点 `'.'` ，后面跟着至少一位数字

**整数**（按顺序）可以分成以下几个部分：

1. （可选）一个符号字符（`'+'` 或 `'-'`）
2. 至少一位数字

部分**数值**列举如下：

- `["+100", "5e2", "-123", "3.1416", "-1E-16", "0123"]`

部分**非数值**列举如下：

- `["12e", "1a3.14", "1.2.3", "+-5", "12e+5.4"]`

```cpp
class Solution {
public:
    bool isNumber(string s) {
        s += '\0';   //指针移动过程中，最后会有一位的溢出，加上一位空字符防止字符串下标越界
        bool isNum = false; //该变量表示从0开始，到i位置的字符串是否构成合法数字，初始化为false

        int i = 0;  //检测指针初始化为0

        while(s[i] == ' ') ++i; //滤除最前面的空格，指针后移

        if(s[i] == '+' || s[i] == '-') ++i; //一个‘-’或‘+’为合法输入，指针后移

        while(s[i] >= '0' && s[i] <= '9'){  //此处如果出现数字，为合法输入，指针后移，同时isNum置为true
            isNum = true;  //显然,在此处，前面的所有字符是可以构成合法数字的
            ++i;
        }

        if(s[i] == '.') ++i;    //按照前面的顺序，在此处出现小数点也是合法的，指针后移（此处能否构成合法字符取决于isNum）

        while(s[i] >= '0' && s[i] <= '9'){  //小数点后出现数字也是合法的，指针后移
            isNum = true;   //无论前面是什么，此处应当是合法数字
            ++i;
        }

        //上面的部分已经把所有只包含小数点和正负号以及数字的情况包括进去了，如果只判断不含E或e的合法数字，到此处就可以停止了

        if(isNum && (s[i] == 'e' || s[i] == 'E')){ //当前面的数字组成一个合法数字时（isNum = true），此处出现e或E也是合法的
            ++i;
            isNum = false; //但到此处，E后面还没有数字，根据isNum的定义，此处的isNum应等于false;

            if(s[i] == '-' || s[i] == '+') ++i; //E或e后面可以出现一个‘-’或‘+’，指针后移

            while(s[i] >= '0' & s[i] <= '9') {
                ++i;
                isNum = true; //E后面接上数字后也就成了合法数字
            }
        }
        while(s[i] == ' ') i ++;

        //如果字符串为合法数字，指针应当移到最后，即是s[i] == '\0' 同时根据isNum判断数字是否合法
        //整个过程中只有当i位置处的输入合法时，指针才会移动
        return (s[i] == '\0' && isNum);
    }
};
```

分析科学计数法，然后分析小数，按照顺序模拟



## [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/description/)

左右指针

```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        if(nums.size() == 0) return {};
        int left = 0, right = nums.size() - 1;

        while(left < right) {
            if(nums[left] % 2 == 1 && nums[right] % 2 == 0) left ++, right --;
            else if(nums[left] % 2 == 0 && nums[right] % 2 == 0) right --;
            else if(nums[left] % 2 == 1 && nums[right] % 2 == 1) left ++;
            else if(nums[left] % 2 == 0 && nums[right] % 2 == 1) swap(nums[left], nums[right]);
        }
        return nums;
    }
};
```

双指针

```golang
func exchange(nums []int) []int {
    if len(nums) == 0 {
        return nil
    }
    // 指针 j 指向当前最后一个奇数的下一个位置
    j := 0
    for i := 0; i < len(nums); i ++ {
        if nums[i] & 1 == 1 {	// 表示奇数
            nums[i], nums[j] = nums[j], nums[i]
            j ++;
        }
    }
    return nums;
}
```

