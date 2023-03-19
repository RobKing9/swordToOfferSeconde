

# Day8 时间效率



## [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode.cn/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/description/)

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        if(nums.size() == 0) return -1;
        int size = nums.size();
        unordered_map<int, int> myMap;
        for(auto num : nums) {
            myMap[num] ++;
            if(myMap[num] > size / 2) return num;
        }
        return -1;
    }
};
```

```golang
func majorityElement(nums []int) int {
    //通过map记录次数
    numap := make(map[int] int)
    half := len(nums)/2
    for _, v := range nums {
        if numap[v]>=half {
            return v
        }
        numap[v]++
    }
    return -1
}
```

## [剑指 Offer 40. 最小的k个数](https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/description/?favorite=xb9nqhhg)

```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(arr.size() == 0 || k < 0 || k > arr.size()) return {};
        sort(arr.begin(), arr.end());

        vector<int> res;
        int i = 0;
        while(k --) { 
            res.push_back(arr[i ++]);
        }
        return res;
    }
};
```

## [剑指 Offer 41. 数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/description/?favorite=xb9nqhhg)

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

```cpp
class MedianFinder {
public:
    // 思路：用一个大跟堆保存 较小的一半数据，堆顶为 最大值。
    // 用一个小根堆保存 较大的一半数据，堆顶为最小值
    // 这样判断元素个数，如果是奇数，那么中位数就是 大跟堆的堆顶
    // 如果是偶数，那么中位数就是 两个堆顶的平均值

    // 维护两个堆
    // 如果大跟堆的堆顶 大于 小根堆的堆顶了，那么说明逆序了，交换两个堆顶
    // 如果大跟堆的长度 大于 小根堆的长度 超过2  说明大跟堆元素太多，弹出一个 放到小根堆中

    priority_queue<int> max_heap;   // 默认是大跟堆
    priority_queue<int, vector<int>, greater<int>> min_heap;    // 小根堆

    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        // 不管怎样先放到大跟堆，不对的话在调整
        max_heap.push(num);
        // 逆序
        if(min_heap.size() != 0 && max_heap.top() > min_heap.top()) {
            // 将两个堆顶都拿出 弹出去 然后在放回去
            int minv = min_heap.top(), maxv = max_heap.top();
            min_heap.pop(), max_heap.pop();
            min_heap.push(maxv), max_heap.push(minv);
        }
        // 数量过多
        if(max_heap.size() >= min_heap.size() + 2) {
            // 拿出大跟堆的
            int tmp = max_heap.top();
            max_heap.pop();
            min_heap.push(tmp);
        }
    }
    // 结果
    double findMedian() {
        // 奇数个
        if(( max_heap.size() + min_heap.size() ) & 1) return max_heap.top();
        else return (max_heap.top() + min_heap.top()) / 2.0;    // 偶数个
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```



## [剑指 Offer 42. 连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/description/?favorite=xb9nqhhg)

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 动态规划
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        int res = dp[0];
        // 最大值 要么是当前值，要么是当前值加上前面所有元素的最大值 两者取最大值
        // dp[i] = max(nums[i], nums[i] + dp[i - 1])
        for(int i = 1; i < nums.size(); i ++ ) {
            dp[i] = max(nums[i], dp[i - 1] + nums[i]);
            // 取每个dp的最大值
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 动态规划
        // vector<int> dp(nums.size(), 0);
        // 优化 只要保存前面的最大值即可，相当于滚动数组
        int preMaxv = nums[0];
        // dp[0] = nums[0];
        int res = nums[0];
        // 最大值 要么是当前值，要么是当前值加上前面所有元素的最大值 两者取最大值
        // dp[i] = max(nums[i], nums[i] + dp[i - 1])
        for(int i = 1; i < nums.size(); i ++ ) {

            preMaxv = max(nums[i], preMaxv + nums[i]);
            // 取每个dp的最大值
            res = max(res, preMaxv);
        }
        return res;
    }
};
```

```golang
func maxSubArray(nums []int) int {
    max := nums[0]
    for i:=1; i<len(nums); i++ {
        if(nums[i]+nums[i-1]>nums[i]) {
            nums[i] = nums[i]+nums[i-1] 
        }
        if nums[i]>max {
            max = nums[i]
        }
    }
    return max
}
```

## [剑指 Offer 43. 1～n 整数中 1 出现的次数](https://leetcode.cn/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/description/?favorite=xb9nqhhg)

输入一个整数 `n` ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

```cpp
class Solution {
public:
    int countDigitOne(int n) {
        // 思路：用数组将每一位存储下来，计算每一位为 1 的时候出现的次数
        if(!n) return 0;
        // 存储到数组
        vector<int> nums;
        while(n) nums.push_back(n % 10), n /= 10;
        // 翻转保证开始是高位
        reverse(nums.begin(), nums.end());
        int res = 0;
        // 遍历每一位
        for(int i = 0; i < nums.size(); i ++ ) {
            // 求出当前位 左边和右边
            // 并且求出 右边的指数级
            int left = 0, right = 0, t = 1;
            // 左边
            for(int j = 0; j < i; j ++ ) left = left * 10 + nums[j];
            // 右边
            for(int j = i + 1; j < nums.size(); j ++ ) right = right * 10 + nums[j], t *= 10;

            res += t * left;
            if(nums[i] == 1) res += right + 1;
            else if(nums[i] > 1) res += t;

        }
        return res;
    }
};
```

## [剑指 Offer 44. 数字序列中某一位的数字](https://leetcode.cn/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/description/)

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

```cpp
class Solution {
public:
    int findNthDigit(int n) {
        if(n < 10) return n;
        // 思路：先判断在哪个区间(几位数)，然后判断是哪个数，然后判断是在这个数的哪个位置
        // 寻找是几位数 digit，以及是这个几位数的开始start的多少位 
        // cnt 表示当前位数有多少位 比如 1->10  2->9*10*2(cnt) 3->9*100*3(cnt)
        long long int digit = 1, start = 1, cnt = 9;
        while(n > cnt) {
            n -= cnt;
            digit ++;   // 位数增加
            start *= 10;    // 改变开始的位置
            cnt = 9 * start * digit;    // 改变当前位的 总位数
        }
        // 上一步结束之后可以知道 
        // n在几位数 digit，这个几位数的开始位置是start，n已经变成了从开始位置到之前位的距离了
        // 这个地方之所以要减去1 是因为我们要求下标从零开始的
        int num = start + (n - 1) / digit;

        return int(to_string(num)[(n-1) % digit] - '0');
    }
};
```

```golang
func findNthDigit(n int) int {
    digit,digitNum,count := 1,1,9
    for n>count{
        n -= count
        digit++
        digitNum *= 10
        count = 9*digit*digitNum
    }
    num := digitNum + (n-1)/digit

    index := (n-1)%digit

    numStr := strconv.Itoa(num)
    return int(numStr[index]-'0')
}
```

## [剑指 Offer 46. 把数字翻译成字符串](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/description/)

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

```cpp
class Solution {
public:
    int translateNum(int num) {
        // 动态规划 dp表示到目前的组合次数
        // 主要判断加一位之后 最后 那个两位数能不能结合到一起翻译
        // 如果不可以的话，那就是和上一个的组合次数一样
        // 如果可以的话，dp[i] = dp[i - 1] + dp[i - 2]
        // 这个表达式的意思就是说要么不组合 是dp[i-1] 要么组合 dp[i-2]
        if(num < 0) return -1;
        string strNum = to_string(num);
        vector<int> dp(strNum.size(), 0);
        dp[0] = 1;
        for(int i = 1; i < strNum.size(); i ++) {
            int tmp = (strNum[i - 1] - '0') * 10 + (strNum[i] - '0');
            // cout << tmp << ' ' << strNum[i - 1] << endl;
			// 注意前一位为0的特殊情况，因为两者也不能结合
            if(tmp > 25 || strNum[i - 1] - '0' == 0) dp[i] = dp[i - 1];
            else {
                if(i == 1) dp[i] = dp[i - 1] + 1;
                else dp[i] = dp[i - 1] + dp[i - 2];
            }
        }
        return dp[strNum.size() - 1];
    }
};
```

```golang
func translateNum(num int) int {
    if num<10{
        return 1
    }
    if num>=10 &&num<=25{
        return 2
    }
    s := strconv.Itoa(num)
    dp := make([]int, len(s))
    dp[0]=1
    if s[:2]>="10"&&s[:2]<="25"{    //边界
        dp[1]=2
    }else{
        dp[1]=1
    }
    for i:=2;i<len(s);i++{
        newnum:=s[i-1:i+1]
        if newnum>="10" && newnum<="25"{
            dp[i]=dp[i-1]+dp[i-2]     //递归关系式
        }else{
            dp[i]=dp[i-1]
        }
    }
    return dp[len(s)-1]

}
```





## [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/description/)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.size() == 0) return 0;
        unordered_map<char, int> myMap;
        int res = 1, left = 0;
        for(int i = 0; i < s.size(); i ++ ) {
            if(myMap.find(s[i]) != myMap.end()) {
                left = max(left, myMap[s[i]] + 1);
            }
            myMap[s[i]] = i;
            res = max(res, i - left + 1);
        }
        return res;
    }
};
```

```golang
func lengthOfLongestSubstring(s string) int {
    //滑动窗口+双指针
    left := 0
    result := 0
    hashMap := make(map[rune]int)
    for i, v := range s {
        //窗口中的值重复了
        if _, ok := hashMap[v]; ok {
            //移动窗口 左边 +1是为了避免重复
            //取最大值 是防止窗口左边界 移动之前的位置  “abba”
            left = max(left, hashMap[v]+1)
        }
        hashMap[v] = i
        //更新 窗口的值 取最大
        result = max(result, i-left+1)
    }
    return result
}

func max(x, y int) int {
    if x<y {
        return y
    }
    return x
}
```
