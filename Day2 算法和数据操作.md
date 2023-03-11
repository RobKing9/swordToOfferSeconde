# Day2 算法和数据操作



## 递归和循环

### [剑指 Offer 10- I. 斐波那契数列](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/description/)

### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode.cn/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/?favorite=xb9nqhhg)

使用循环的方法

```cpp
class Solution {
public:
    int fib(int n) {
        vector<int> res;
        res.push_back(0);
        res.push_back(1);
        for(int i = 2; i <= n; i ++ ) {
            res.push_back((res[i - 1] + res[i - 2]) % 1000000007);
        }
        return res[n];
    }
};
```

```golang
func fib(n int) int {
   if n == 0 || n == 1 {
       return n
   }
   a := make([]int, n+1)
   a[0] = 0
   a[1] = 1
   for i := 2; i <= n; i++ {
       a[i] = (a[i-1] + a[i-2])%1000000007
   }
   return a[n]
}
```

记忆式搜索

```cpp
class Solution {
public:
    unordered_map<int, int> myMap;
    int fib(int n) {
        // 递归 结束条件
        if(n == 0) return 0;
        if(n <= 2) return 1;
        // 记录
        if(myMap.find(n) != myMap.end()) return myMap[n];
        int res = (fib(n - 1) + fib(n - 2)) % 1000000007;
        myMap[n] = res;
        return res;
    }
};
```

## 查找和排序

[剑指 Offer 11. 旋转数组的最小数字](https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/description/)

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。

给你一个可能存在 **重复** 元素值的数组 `numbers` ，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的**最小元素**。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一次旋转，该数组的最小值为 1。 

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` 旋转一次 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

```cpp
class Solution {
public:
    int minArray(vector<int>& numbers) {
        // 二分
        int left = 0, right = numbers.size() - 1;
        while(left < right) {
            int mid = (left + right) >> 1;
            // 中间值小于最右边的值，说明在 最右边值的 左边
            if(numbers[mid] < numbers[right]) {
                right = mid;
            }else if(numbers[mid] > numbers[right]) {
                left = mid + 1;
            }else right --;
        }
        return numbers[left];
    }
};
```

```golang
func minArray(numbers []int) int {
    //二分法
    left, right := 0, len(numbers)-1
    for left<right {
        mid := (left+right)/2
        if numbers[mid]<numbers[right] {
            right = mid
        }else if numbers[mid]>numbers[right] {
            left = mid+1
        }else {
            right--
        }
    }
    return numbers[left]
}
```

注意几个细节：

1. 循环结束的条件是 不等于。这和之前二分查找目标值不一样
2. 中间值和最右值相等的时候，将右指针 左移一步，这样不会影响最小值。
3. 返回的值就是最后的最小值，此时left == right

## 回溯法

## [剑指 Offer 12. 矩阵中的路径](https://leetcode.cn/problems/ju-zhen-zhong-de-lu-jing-lcof/description/)

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

```cpp
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        // 深度优先搜索
        for(int i = 0; i < board.size(); i ++){
            for(int j = 0; j < board[0].size(); j ++ ) {
                if( dfs(board, i, j, word, 0)) return true;
            }
        }
        return false;
    }
    // 沿着四个方向 扫描
    bool dfs(vector<vector<char>>& board, int r, int c, string word, int k) {
        // 超出范围
        if(!isArea(board, r, c)) return false;

        // 单词不匹配
        if(board[r][c] != word[k]) return false;

        // 匹配完毕
        if(k == word.size() - 1) return true;

        // 标记已经匹配过了
        board[r][c] = '0';

        // 沿着四个方向 扫描  只要有一个成功就可以了
        bool res = dfs(board, r - 1, c, word, k + 1) || dfs(board, r + 1, c, word, k + 1) || dfs(board, r, c - 1, word, k + 1) || dfs(board, r, c + 1, word, k + 1);
        // 回溯
        board[r][c] = word[k];

        return res;
    }
    bool isArea(vector<vector<char>>& board, int r, int c) {
        return r >= 0 && c >= 0 && r < board.size() && c < board[0].size();
    }
};
```

```golang
func exist(board [][]byte, word string) bool {
    for i:=0; i<len(board); i++ {
        for j:=0; j<len(board[0]); j++ {
            if dfs(board, i, j, word, 0) {
                return true
            }
        }
    }
    return false
}

//DFS 深度遍历
func dfs(board [][]byte, r, c int, word string, k int) bool {
    //越界 剪枝
    if !inArea(board, r, c) {
        return false
    }
    //值不相等 剪枝
    if board[r][c] != word[k] {
        return false
    }
    //字符串已经 遍历完毕 true
    if k == len(word)-1 {
        return true
    }
    //标记已经访问过了
    board[r][c] = '0'
    //上下左右遍历
    result := dfs(board, r-1, c, word, k+1) || dfs(board, r+1, c, word, k+1) || dfs(board, r, c-1, word, k+1) || dfs(board, r, c+1, word, k+1)
    //回溯
    board[r][c] = word[k]
    return result
    
}

func inArea(board [][]byte, r, c int) bool{
    return r>=0 && r<len(board) && c>=0 && c<len(board[0])
}
```

思路就是从任何一个点开始，往四个方向匹配单词，每个匹配完了的标记为已经访问过了。

注意几个细节：

1. 两个剪枝，不在范围，不匹配
2. 记得回溯
3. 四个方向只要一个满足条件就可以了
4. 遍历完毕返回true

## 动态规划和贪心

### [剑指 Offer 14- I. 剪绳子](https://leetcode.cn/problems/jian-sheng-zi-lcof/description/)

给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

贪心

```cpp
class Solution {
public:
    int cuttingRope(int n) {
        // 贪心，尽可能多的3
        // 证明：当 n >= 5   3*(n - 3) 一定会大于n
        // 当n = 4， 拆成 2 * 2
        // 边界
        if(n <= 3) return n - 1;
        int res = 1;
        if(n % 3 == 1) res *= 4, n -= 4;
        if(n % 3 == 2) res *= 2, n -= 2;
        while(n) res *= 3, n -= 3;

        return res; 
    }
};
```

动态规划

```cpp
class Solution {
public:
    int cuttingRope(int n) {
        if(n < 2) return 0;
        if(n <= 3) return n - 1;

        // vector<int> dp(n + 1, 0);
        int * dp = new int[n + 1];
        dp[0] = 0, dp[1] = 1, dp[2] = 1, dp[3] = 2;

        for(int i = 4; i <= n; i ++ ) {
            // j 表示第二刀
            for(int j = 1; j <= i / 2; j ++ ) {
                // 两种情况
                // 一种第二刀剪j，就不剪了
                // 另一种情况，第二刀继续剪
                dp[i] = max(dp[i], max(j * (i - j), j * dp[i - j]));
            }
        }
        return dp[n];
    }
};
```

```golang
//剪成的每段用一个数组 arr := make([]int, 0) len(arr) = m m什么时候最合理
//最大乘积 遍历数组 for range arr {v} 怎么剪乘积最大？剪成几段？每段多大？
//方法一：穷举所有结果 找到最大的一种 
//动态规划：
//1、穷举 (2->1 1*1) (3->2 1*2) (4->4 2*2) (5->6 2*3) (6->9 3*3) (7->12 2*2*3) (8->18 2*3*3)
//dp[n] =
//2、边界 3、规律 4、状态转移方程 
//0->0 1->1 if(n<=1) return n
func cuttingRope(n int) int {
    dp := make([]int, n+1) 
    dp[2] = 1
    for i:=3; i<=n; i++ {
        for j:=1; j<=i/2; j++ {
            dp[i] = max(dp[i], max(j*dp[i-j], j*(i-j)))
        }
    }
    return dp[n]
}

func max(a, b int) int {
    if a<b {
        return b
    }else {
        return a
    }
}
```

### [剑指 Offer 14- II. 剪绳子 II](https://leetcode.cn/problems/jian-sheng-zi-ii-lcof/description/?favorite=xb9nqhhg)

```golang
class Solution {
public:
    int cuttingRope(int n) {
         // 贪心，尽可能多的3
        // 证明：当 n >= 5   3*(n - 3) 一定会大于n
        // 当n = 4， 拆成 2 * 2
        // 边界
        if(n <= 3) return n - 1;
        long int res = 1;
        if(n % 3 == 1) res *= 4, n -= 4;
        if(n % 3 == 2) res *= 2, n -= 2;
        while(n) res = (res * 3) % 1000000007, n -= 3;

        return res; 
    }
};
```



## [剑指 Offer 15. 二进制中1的个数](https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/description/)

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while(n) {
            n -= n & -n;	// n & -n 可以找到最后一位 1    -n等于n取反+1
            res ++;
        }
        return res;
    }
};
```

```golang
func hammingWeight(num uint32) int {
    flag := 0
    for num!=0 {
        num = num&(num-1)
        flag++
    }
    return flag
}
```

