# Day9 空间效率



## [剑指 Offer 49. 丑数](https://leetcode.cn/problems/chou-shu-lcof/description/?favorite=xb9nqhhg)

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        // 动态规划 dp表示到当前所表示的 丑数
        if(n <= 6) return n;
        vector<int> dp(n, 0);
        dp[0] = 1;  // 第一个丑数是 1
        int i = 0, j = 0, k = 0;    //用三个指针分别指向2，3, 5 的倍数  开始都指向1
        for(int x = 1; x < n; x ++ ) {
            int tmp = min(dp[i] * 2, min(dp[j] * 3, dp[k] * 5));
            dp[x] = tmp;
            //如果有两个乘积一样得到的 那么都要移动
            if(tmp == dp[i] * 2 ) i ++;
            if(tmp == dp[j] * 3) j ++;
            if(tmp == dp[k] * 5) k ++;
        }
        return dp[n - 1];
    }
};
```

```cpp
func nthUglyNumber(n int) int {
    //动态规划
    //思路：我们知道每个丑数都是由之前的序列的每个值 *2 *3 *5 得到的，但是这样的话会有重复，我们需要得到一个不重复并且按照从小到大的顺序排列的，所以我们可以建立索引来控制他们的速度，每一次取最小的那个值
    dp := make([]int, n)
    //初始化
    dp[0] = 1
    //记录 索引 到哪里了
    a, b, c := 0, 0, 0
    //遍历到n
    for i:=1; i<n; i++ {
        n2, n3, n5 := dp[a]*2, dp[b]*3, dp[c]*5
        dp[i] = min(min(n2, n3), n5)
        //如果是因为这个数乘积得到的 那么就要移动到下一位
        //如果有两个乘积一样得到的 那么都要移动
        if n2 == dp[i] {
            a++
        }
        if n3 == dp[i] {
            b++
        }
        if n5 == dp[i] {
            c++
        }
    }
    return dp[n-1]
}

func min(x, y int) int {
    if x<y {
        return x
    }
    return y
}
```

## [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/description/)

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

```cpp
class Solution {
public:
    char firstUniqChar(string s) {
        if(s.size() == 0) return ' ';
        // 必须要初始化
        int res[26] = {0};
        cout << i << endl;
        for(int i = 0; i < s.size(); i ++ ) {
            res[s[i] - 'a'] ++;
        }
        // 在遍历一遍
        for(int i = 0; i < s.size(); i ++ ) {
            if(res[s[i] - 'a'] == 1) return s[i];
        }
        return ' ';
    }
};
```

## [剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/description/?favorite=xb9nqhhg)

```cpp
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        return mergeSort(nums, 0, nums.size() - 1);
    }

    // 返回 逆序对的 个数
    int mergeSort(vector<int>& nums, int left, int right) {
        if(left >= right) return 0;
        // 总数 为 左边内部逆序对 + 右边内部逆序对 + 两边的
        int mid = (left + right) >> 1;
        int res = mergeSort(nums, left, mid) + mergeSort(nums, mid + 1, right);
        // 使用一个临时数组
        vector<int> tmp;
        int i = left, j = mid + 1;
        // 左右两段 
        while(i <= mid && j <= right) {
            if(nums[i] <= nums[j]) tmp.push_back(nums[i ++]); // 正序
            else {
                // 逆序
                // 因为是两个有序数组 左边有序 那么这个数之后的数 和 右边那个数都构成逆序
                res += mid - i + 1;
                tmp.push_back(nums[j ++]);
            }
        }
        // 剩余的数
        while(i <= mid) tmp.push_back(nums[i ++]);
        while(j <= right) tmp.push_back(nums[j ++]);
        // 复制数组
        for(int i = left, j = 0; i <= right; i ++, j ++ ) nums[i] = tmp[j];
        return res;
    }
};
```

```golang
func reversePairs(nums []int) int {
    return mergeSort(nums, 0, len(nums)-1)
}

func mergeSort(nums []int, start, end int) int {
    if start >= end {
        return 0
    }
    mid := start + (end - start)/2
    cnt := mergeSort(nums, start, mid) + mergeSort(nums, mid + 1, end) //cnt包括整体+左半+右半
    tmp := []int{}  //辅助数组,用于存储排好序的数
    i, j := start, mid + 1 //左指针从左半部分的左端开始遍历 右指针从右半部分的左端开始遍历
    for i <= mid && j <= end {  //指针超出自己的那部分则退出循环
        if nums[i] <= nums[j] { //当左元素小于等于右元素 说明在右半部分的右元素的左边的元素均小于左元素
            tmp = append(tmp, nums[i])
            cnt += j - (mid + 1)    //因此cnt需要加上右元素左边部分的元素数量
            i++
        } else {
            tmp = append(tmp, nums[j])  //当左元素大于右元素 不能直接开始计数 
            j++                         //因为不确定右元素右边还有没有比左元素更小的元素
        }
    }
    for ; i <= mid; i++ {   //处理另一半未遍历完的数据
        tmp = append(tmp, nums[i])
        cnt += end - (mid + 1) + 1
    }
    for ; j <= end; j++ {   //同上
        tmp = append(tmp, nums[j])
    }
    for i := start; i <= end; i++ { //将排序好的数组元素依次赋值给nums数组
        nums[i] = tmp[i - start]
    }
    return cnt
}
```



## [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/description/?favorite=xb9nqhhg)

输入两个链表，找出它们的第一个公共节点。

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(headA == nullptr || headB == nullptr) return nullptr;

        auto curA = headA, curB = headB;
        while(curA != curB) {
            if(curA) curA = curA->next;
            else curA = headB;

            if(curB) curB = curB->next;
            else curB = headA;
        } 
        return curA;
    }
};
```

