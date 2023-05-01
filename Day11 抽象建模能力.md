# Day11 抽象建模能力



## [剑指 Offer 60. n个骰子的点数](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/description/?favorite=xb9nqhhg)

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。 

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。



```golang
func dicesProbability(n int) []float64 {
    res := make([]float64, 0)
    sum := 1.0
    for i := 0; i < n; i ++ {
        sum *= 6
    }
    for i := n; i <= 6 * n; i ++ {
        tmp := (float64)(dfs(n, i)) / sum
        res = append(res, tmp)
    }
    return res
}

func dfs(n, sum int) int {
    // 边界条件
    if sum < 0 {
        return 0
    }
    if n == 0 {
        if sum != 0 {
            return 0
        }else {
            return 1
        }
    }
    // 递归计算
    var res int = 0
    for i := 1; i <= 6; i ++ {
        res += dfs(n - 1, sum - i)
    }
    return res
}
```

递归方法，会超时



动态规划

```cpp
class Solution {
public:
    vector<double> dicesProbability(int n) {
        // dp[i][j] 表示 方案总数，i表示次数，j表示和
        vector<vector<int>> dp(n + 1, vector<int>(n * 6 + 1, 0));
        // 初始化 0次为0 方案为1
        dp[0][0] = 1;
        // 遍历 1-n 次
        for(int i = 1; i <= n; i ++ ) {
            // 遍历和 n-6*n
            // 之所以从1开始 而不是n，是因为要先计算小问题
            for(int j = 1; j <= 6 * n; j ++ ) {
                // 每一次 选择点数，注意因为选择点数就会减去，所以j必须大于k
                for(int k = 1; k <= 6; k ++ ) {
                    // 状态转移方程 之所以是 += 是因为有k种情况
                    if(j >= k) dp[i][j] += dp[i - 1][j - k];
                }
            }
        }
        // 分母
        double sum = 1.0;
        for(int i = 0; i < n; i ++ ) sum *= 6;

        // 选择最后一层
        vector<double> res;
        for(int i = n; i <= 6 * n; i ++ ) {
            double tmp = (double) (dp[n][i]) / sum;
            res.push_back(tmp);
        }
        return res;
    }
};
```

## [面试题61. 扑克牌中的顺子](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/description/)

从**若干副扑克牌**中随机抽 `5` 张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

```cpp
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        // 思路：找到最小值和最大值，判断 相减是否小于5。另外通过map判断是否有重复数字
        int minv = 14, maxv = 0;
        unordered_map<int, bool> umap;
        for(int num : nums) {
            if(num == 0) continue;  // 0继续
            if(num < minv) minv = num;
            if(num > maxv) maxv = num;
            if(umap[num]) return false;  // 重复
            umap[num] = true;
        }
        return maxv - minv < 5;
    }
};
```

```golang
func isStraight(nums []int) bool {
    hashTable := make(map[int]bool)
    minv, maxv := 14, 0
    for _, v := range nums {
        if v==0 {
            continue
        }
        if v<minv {
            minv = v
        }
        if v>maxv {
            maxv = v
        }
        
        if hashTable[v] {
            return false
        }
        hashTable[v] = true
    }
    return maxv-minv<5
}
```

简单一句话：没有重复的情况下，最小和最大相差小于5就是顺子

## [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/description/?favorite=xb9nqhhg)

```golang
func lastRemaining(n int, m int) int {
    // dp[i] 表示 0-i 这 i-1 个数字，每次删除第m个 后的结果
    dp := make([]int, n + 1)
    // 初始化 i=1的时候 结果肯定是0
    dp[1] = 0
    for i := 2; i <= n; i ++ {
        // 递推方程
        dp[i] = (dp[i - 1] + m) % i
    }
    return dp[n]
}
```

这一题使用的是动态规划方法。表示 `[n-1, m]` 问题 可以求解出 `[n, m]` 问题， 方法主要是根据 `[n-1, m]` 问题的序列 映射成 `[n,m]` 问题的序列。

需要删除的数字为 `(m-1) % n`, 下一轮开始的数字为 `t = m % n`

所以删除后的序列为 `t, t + 1, t + 2 ....... t -1, t - 2`    (根据圆圈可以知道往后移动两个数字就是t-2)

`[n-1, m]` 问题的序列为  `0,1,2.......n-1, n-2`

映射关系为 `x = (x + t) % n`  x表示[n-1,m]问题的每一个数字，映射成 删除后的序列的每一个数字，这样就将[n-1,m]问题转化为了`[n,m]`问题

所以 `dp[n] = (dp[n-1] + m%n) % n` 化简得 `dp[n] = (dp[n-1] + m) % n`



## [剑指 Offer 63. 股票的最大利润](https://leetcode.cn/problems/gu-piao-de-zui-da-li-run-lcof/description/?favorite=xb9nqhhg)

```golang
func maxProfit(prices []int) int {
    if len(prices) == 0 {
        return 0
    }
    var res = 0
    minv := prices[0]
    for i := 0; i < len(prices); i ++ {
        res = max(res, prices[i] - minv)
        minv = min(minv, prices[i])
    }
    return res
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}


func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}
```

用一个变量保存前面的最小值，然后依次和这个最小值相减，这个结果取最大值就是结果

## [剑指 Offer 64. 求1+2+…+n](https://leetcode.cn/problems/qiu-12n-lcof/description/?favorite=xb9nqhhg&languageTags=golang)

```cpp
class Solution {
public:
    int sumNums(int n) {
        n && (n += sumNums(n - 1));
        return n;
    }
};
```

## [剑指 Offer 65. 不用加减乘除做加法](https://leetcode.cn/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/?favorite=xb9nqhhg)

```cpp
class Solution {
public:
    int add(int a, int b) {
        int carry = 0;
        while(b) {
            // 进位
            carry = a&b << 1;
            // 异或
            a ^= b;
            // 进位赋给 b，加上进位
            b = carry;
        }
        return a;
    }
};
```

## [剑指 Offer 66. 构建乘积数组](https://leetcode.cn/problems/gou-jian-cheng-ji-shu-zu-lcof/?favorite=xb9nqhhg)

```cpp
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        if(a.size() == 0) return {};
        // res记录答案
        vector<int> res(a.size(), 1);
        for(int i = 1; i < a.size(); i ++ ) {
            // 从前往后计算，每次结果是 上一次的乘积乘上上一次的那个数
            res[i] = res[i - 1] * a[i - 1];
        }
        // 从后往前
        int rightSum = 1;
        for(int i = a.size() - 2; i >= 0; i --) {
            rightSum *= a[i + 1];
            res[i] *= rightSum;
        }
        return res;
    }
};
```

## [剑指 Offer 67. 把字符串转换成整数](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/description/?favorite=xb9nqhhg)

```golang
const(
    INT_MAX=1<<31-1
    INT_MIN=-1<<31
)

func strToInt(str string) int {
    if len(str)==0{
        return 0
    }
     i := 0
    for i<len(str)&&str[i]==' '{
        i++
    }
    c:=1
    if i>=len(str){
        return 0
    }
    if  i<len(str)&&str[i]=='-'{
        c=-1
        i++
    }else if  i<len(str)&&str[i]=='+'{
        i++
    }
    res:=Atoi(str[i:],c )
    return res
}
func Atoi(str string,c int)int{
    res:=0
    for i:=0;i<len(str)&&str[i]>='0'&&str[i]<='9';i++{
        res=res*10+int(str[i]-'0')
        if res*c>INT_MAX{
            return INT_MAX
        }else if res*c<INT_MIN{
            return INT_MIN
        }
    }
    return c*res
}
```

## [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/description/?favorite=xb9nqhhg)

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr || q == nullptr || p == nullptr) return nullptr;
        return helper(root, p, q);
    }
    TreeNode* helper(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr) return nullptr;
        // 都大于
        if(p->val > root->val && q->val > root->val) return helper(root->right, p, q);
        // 都小于
        if(p->val < root->val && q->val < root->val) return helper(root->left, p, q);
        return root;
    }
};
```

## [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/description/?favorite=xb9nqhhg)

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr) return nullptr;
        if(root == q || root == p) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if(left && right) return root;
        if(left) return left;
        if(right) return right;
        return nullptr;
    }
};
```

