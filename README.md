# 剑指offer简单思路与总结

## [剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

需要找到重复的数字。用**哈希表法**（k为数组的值，value为bool），遍历数组，每一个值标为true，如果第二次访问了那么直接返回即可

## [剑指 Offer 04. 二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

在有规律的二维数组查找目标值是否存在。用**类比二叉搜索树法**，将左下角或者右上角的元素看做是根节点（观察可以知道符合二叉搜索树的特征）；该坐标作为条件遍历，如果大于或者小于该值，移动行或者列，找到直接返回即可。

## [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

将空格替换为（%20）。可以直接使用**库方法**，`strings.Replace(s, " ", "%20", -1)`；或者遍历字符串，每次将值转换为string类型加入到新字符串，遇到空格，加入%20，返回即可

## [剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

将链表的值用数组反向输出。可以直接遍历链表，将值加入到数组，然后反转数组；也可以先反转链表，然后将值加入到数组。

## [剑指 Offer 07. 重建二叉树](https://leetcode.cn/problems/zhong-jian-er-cha-shu-lcof/)

根据前序和中序遍历结果构建二叉树。首先前序遍历的第一个元素是根节点；在中序遍历结果中找到这个值，得到他的索引；通过这个索引可以分别在先序和中序划定左右子树，最后递归得到左右子树，返回根节点即可。

## [剑指 Offer 09. 用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

用两个栈实现队列。从整体上来看就是实现入队和出队的操作

- 入队：这时候用第一个栈，直接将元素放进去即可
- 出队：如果只有一个栈，以为着只能去栈头，不满足我们出队先进先出的条件，所以我们需要第二个栈，这个栈主要是将第一个栈中的元素出栈放到第二个栈，这样一来第二个栈的栈头就是我们需要的队头了。

## [剑指 Offer 10- I. 斐波那契数列](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/)

## [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode.cn/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

青蛙一次可以跳一个台阶或两个，一共有多少种跳法。两道题是一样的，这里不可以用递归的方法，会超时，所以只能用非递归，用数组保存元素，先是第一个和第二个已知的，然后循环，求`a[i]=a[i-1]+a[i-2]`，当然按照题目的意思需要取模，最后返回`a[n]`即可

## [剑指 Offer 11. 旋转数组的最小数字](https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

找到旋转数组最小值。使用**二分法**，如果中间值大于最右边的值，那么最小值在中间值的右边，`left=mid+1`（因为不可能是中间值，所以可以+1），如果中间值小于最右边的值，那么最小值在中间值的左边，`right=mid`（因为可能这个中间值就是最小值）；最后是相等的情况，考虑到有重复元素的存在，所以将right向右移动一步即可，最后返回`nums[left]`

## [剑指 Offer 12. 矩阵中的路径](https://leetcode.cn/problems/ju-zhen-zhong-de-lu-jing-lcof/)

在一个网格中找单词是否存在。采用**深度搜索遍历**的方法，如果越界，值与单词字母不相等都返回`false`，如果访问过就标为`'0'`,用一个布尔值来记录上下左右的返回值（只要有一边满足条件即可），直到单词匹配完毕，才返回`true`，另外如果出现不满足的回溯，将标记过的返回原字母

## [剑指 Offer 14- I. 剪绳子](https://leetcode.cn/problems/jian-sheng-zi-lcof/)

将绳子分成m段，每段相乘，求乘积最大值。我使用的方法是，开始从两段开始，分别找每种情况的最大值，可以知道，分出来的每段越近，乘积越大。所以可以依次求出每段的长度，求出每种情况的最大值，取最大即可。

另外一种方法是动态规划，dp表示长度为i的绳子，最大乘积；对于一根绳子，一种情况是剪成两段：先剪`j`，然后剪`i-j`, `j*(i-j)`第二种情况是`i-j`继续剪  `j*dp[i-j]`

## [面试题13. 机器人的运动范围](https://leetcode.cn/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

满足条件数位和不大于k。使用的是深度优先遍历，用标记数组来标记访问过的元素。

## [剑指 Offer 14- II. 剪绳子 II](https://leetcode.cn/problems/jian-sheng-zi-ii-lcof/)

和上一题题目一样，只不过要对最后的结果取对。使用的方法是快速幂。

## [剑指 Offer 15. 二进制中1的个数](https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

输入一个二进制，求出有多少个一。我们可以知道，一个数和这个数减一取与，会让这个数少一个一，依据这个原理，我们可以用一个计数器来计数，只要这个数不为0，每次计数器加一，最后返回即可。

## [剑指 Offer 16. 数值的整数次方](https://leetcode.cn/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

求x的n次方。使用**递归+二分**的方法，递归结束的条件是n为0，返回1即可。每次递归将n变为一半（可以理解为二分），保存每次递归的值，判断n是否为奇数，如果是奇数，那么除了这个递归值相乘之外还需要乘以x，因为是奇数，n/2会丢失一个值。

## [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode.cn/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

打印到10的几次方（参数）的所有数。首先求出这个数，然后从1开始循环打印出来即可。

## [剑指 Offer 18. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

在链表中找到指定的值，删除这个节点。考虑到第一个节点可能会被删除，所以需要借助哑巴结点，然后往后遍历，遇到值相等，指向下一个结点，最后返回`head.Next`即可。

## ？？[剑指 Offer 19. 正则表达式匹配](https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)

使用的是动态规划，实在看不懂题解。。。。。。。。。。。。。。。

## ？？[剑指 Offer 20. 表示数值的字符串](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

和上一题类似，看不懂。。。。。。。。。。。。。。。。。。。。。

## [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

最后的数组奇数在前面，偶数在后面。我的方法是，先将偶数放在原数组的最后面，把奇数放在新数组中，最后在指定原数组偶数那一段，`append`到新数组即可。

## [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

找到倒数第`k`个节点。我的方法是，找到正数第`len-k+1`个，首先遍历链表求出它的长度，然后再次遍历链表，这一次借用一个标志`flag`，当`flag=len-k+1`时证明到了，退出循环，返回此时的节点即可。

## [剑指 Offer 24. 反转链表](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/)

反转链表，真的很简单的题目。我们需要做的就是把一个节点的下一个节点变为它的上一个节点，下一结点被改变了，那么如果要循环就需要一个节点来保存这个值，另外由于不是双链表，没有上一个节点，所以需要一个来保存上一个节点，保持循环就可以了。

## [剑指 Offer 25. 合并两个排序的链表](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

两种方法解决

- 递归：用`l1`来综合两个节点，下一个节点取决于谁小，`l2`小的话就交换`l1`和`l2`
- 哑巴节点：用一个哑巴节点来辅助，判断两个链表的值，让哑巴节点指向小的，最后两个不为空的加入到哑巴节点的后面即可

## [剑指 Offer 27. 二叉树的镜像](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/)

## [剑指 Offer 28. 对称的二叉树](https://leetcode.cn/problems/dui-cheng-de-er-cha-shu-lcof/)

## [剑指 Offer 26. 树的子结构](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/)

三个题目都是用递归方法，具体参考`二叉树_递归目录下的`递归.md`文件

## [剑指 Offer 29. 顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

这一题要求我们顺时针把矩阵的值打印出来。也就是按照右下左上的顺序打印。我们采用的方法是给四边`r, l, t, b`设置边界，均可以取到，当我们从左往右打印的时候，上边界就会往下移动一步，其他类似；直到上边界值大于下边界，左边界值大于右边界，我们`break`退出循环即可。

## [剑指 Offer 30. 包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/)

这一题就是增加一个栈，栈顶都是最小值。首先是`push`，普通栈就直接可以将元素`push`进去即可，但是最小栈只能`push`比栈顶小的元素，这时候`pop`就有讲究了，因为需要`pop`的值不一定在最小栈中，所以需要进行比较，如果在，才会`pop`，最后就是最小值，直接取最小栈的栈顶即可。

## [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

这一题就是给我们栈的压入和弹出序列，让我们来判断弹出序列合不合理。这一题我们采用的方法是**模拟栈**， 遍历入栈序列加入到一个新的栈，然后弹出栈序列和新栈的栈顶比较，如果相等，就移动弹出序列的指针，模拟栈栈顶出栈，直到不相等；最后我们看一下模拟栈是否为空，为空就说明匹配完了，返回`true`。

## [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

## [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

## [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

这一题的难度是逐渐增加的，主要考察的是**层次遍历**。第一道题是一层一层的加入到一个一维数组中，第二道题是每一层加入到一个二维数组，第三道题是偶数层的需要逆序输出。这三题都是考察层序遍历，都使用队列的方式实现，遍历每一层，每一层都有一个固定的元素个数，遍历他们，取对头，将队头的值加入到数组，然后出队，看一下这个队头有没有左右子树，有的话加入到指针数组。第三题主要是用一个标志位，如果是`true`的话，就需要反转，我们将这一层的数组结果反转一下就好了，然后`flag=!flag`

## [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

给定一个二叉搜索树的后序遍历，判断是否合理。我们采用的方法是**分治递归法**, 因为后序遍历的最后一个元素肯定是根元素，那么前面的元素就被分成两份，左半部分是小于最后元素的，右半部分是大于的，所以我们关键要找到这个下标，可以遍历数组，直到值大于最后一个值的时候`break`，我们就可以获得这个索引，然后如果满足的话，就意味着从这个索引往后都是大于最后一个元素值的，所以我们继续遍历，如果有元素没有大于，直接返回`false`，之后我们就递归左半部分和右半部分，执行一样的操作即可！

## [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode.cn/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

给定一个目标值，这二叉树中找到一条路径，路径上的值相加等于目标值。我们采用的方法是**回溯法**, 遍历二叉树，每次都将元素值加入到一个一维数组中，然后目标值依据这个值减小，当最后满足`target=0`时（当然左右子树也要是空），我们将一维数组的值`copy`到另一个数组中（主要是为了防止之后的回溯），将`copy`数组加入到最后的结果二维数组中，然后递归左右子树，最后不满足的话就要撤销选择，将元素从一维数组中`pop`出去,最后返回二维数组即可.

## [剑指 Offer 35. 复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

这一题要求我们复制一个多了一个随机结点的复杂链表。我们采用的方法是**哈希表**，首先我们将值放到哈希表中，因为随机结点的指向是随机的，所以先要有值，再一次遍历，增加`Next`指针和`Random`指针`hashtable[cur].Random = hashtable[cur.Random]`。

## [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

因为是二叉搜索树，所以中序遍历是有序的，我们可以进行中序遍历，先遍历左节点，当第一次当遍历到根节点的时候，此时`pre`为空将`head = cur`，然后用pre保存当前结点cur的值，之后进行修改pre的right变为cur，cur的left变为pre，每一次pre都保存cur的值，遍历结束之后此时变成了双向链表，但是没有循环，所以我们需要让head的left指向pre即头结点指向尾结点，pre的right指向head即尾结点指向头结点。

## ？？？[剑指 Offer 37. 序列化二叉树](https://leetcode.cn/problems/xu-lie-hua-er-cha-shu-lcof/)

## [剑指 Offer 38. 字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)

需要打印字符串的所有排列。这一题采用的是回溯法，因为可能会有重复的元素，就会出现相同的排列，所以需要避免，一开始对字符串进行排序，然后将字符串变成数组的形式，依次将元素加入到每一种情况的数组，为了避免重复选择，我们借助了一个数组来标记，如果选择了，标记为true，之后遇到之间剪枝，另外如果当前元素与前面的元素值相等，并且之前的那个元素未被标记，也需要剪枝，然后回溯撤销选择，最后当每一种情况的数组长度等于那个数组长度的时候，将结果加入到最终的结果，需要转换，返回结果即可。

## [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode.cn/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

返回数组中超过一半的数字。我们用一个哈希表来计数，当哈希值大于一半的时候返回这个值即可。

## [剑指 Offer 40. 最小的k个数](https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/)

直接从小到大进行排序，返回数组前k个元素即可。

## [剑指 Offer 41. 数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

```cpp
	// 思路：用一个大跟堆保存 较小的一半数据，堆顶为 最大值。
    // 用一个小根堆保存 较大的一半数据，堆顶为最小值
    // 这样判断元素个数，如果是奇数，那么中位数就是 大跟堆的堆顶
    // 如果是偶数，那么中位数就是 两个堆顶的平均值

    // 维护两个堆
    // 如果大跟堆的堆顶 大于 小根堆的堆顶了，那么说明逆序了，交换两个堆顶
    // 如果大跟堆的长度 大于 小根堆的长度 超过2  说明大跟堆元素太多，弹出一个 放到小根堆中
```

## [剑指 Offer 42. 连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

这一题要求我们找到所以子数组和中的最大值。我们直接遍历一遍数组，很容易知道，当后面一个数加上前面一个数的和比自己还要小的时候，那么这样构成的数组肯定不是最大的，我们要从他变大的那一刻开始记录，当相加比自己大的时候，把自己变成这个最大值，然后再去和要求的最大值去比较，最后返回最大值即可。

## [剑指 Offer 43. 1～n 整数中 1 出现的次数](https://leetcode.cn/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

```

```



## ？[剑指 Offer 44. 数字序列中某一位的数字](https://leetcode.cn/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

数字以0123456789101112131415…的格式序列化到一个字符序列中，我们的目标是给定一个位数，判断它的值。

这一题我们将问题分成三步

1. 确定所求数位的所在数字的位数。比如我们要求第19位，那么这个数字是二位数还是三位数

   用一个count来判断，n和count进行比较。start表示每一个位数的开始值，digit表示位数。初始状态

   ```
   digit=1		start=1		count=9
   ```

   循环

   ```
   n -= count	start *= 10		digit += 1		count = 9*start*digit
   ```

   

2. 确定所求数位所在的数字。比如我们要求第19位，我们就先确定第19位是在14这个数字

   ```
   num = start+(n-1)/digit
   ```

   

3. 确定所求数位在 num 的哪一数位。是在14的1还是4

   ```
   将num编程一个字符串，找到下标为 (n-1)%digit	的值
   ```


## ？？[剑指 Offer 45. 把数组排成最小的数](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

使用快速排序的方法。

## [剑指 Offer 46. 把数字翻译成字符串](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

剑指offer下的`动态规划`目录文件

## [剑指 Offer 47. 礼物的最大价值](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/)

从左上一直到右下，找到礼物的最大价值。我们采用动态规划的方法，遍历数组，当前值的dp取上或者左的最大值。最后返回dp即可

## [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

剑指offer`数组_双指针_滑动窗口`目录下的文件。

## [剑指 Offer 49. 丑数](https://leetcode.cn/problems/chou-shu-lcof/)

剑指offer`动态规划`目录下的文件。

## [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

给定一个字符串，返回第一个只出现一次的字符。所以我们需要进行计数，首先可以用哈希表，遍历字符串进行计数，再次遍历返回第一个哈希值为1的字符，但是这种时间复杂度太高了，我们可以用26位数组来进行计数，下标是`s[i]-'a'`, 值就是次数，最后同样再次遍历即可。

## ？[剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

使用的是**归并排序**的方法，在合并的同时，如果左边的数大于右边的数，那么左边剩下的都是逆序对。

## [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

找到两个链表的第一个公共节点。我们采用的方法是双指针，两个指针分别遍历两个链表，当遍历到空节点的时候，就把头结点变成另一个的头结点继续遍历，如果有公共的那么一定会相遇，否则当他们都到nil的时候也会停止。

## [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

直接遍历数组，如果和目标值相等的话，返回的结果加一，最后将结果返回即可。

## [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/)

还是一样遍历数组，因为下标和值是相等的，所以当遇到不相等的情况返回下标即可。注意的是可能会出现没有缺失的情况，这时候返回数组的长度即可，也就是后一位数字

## [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

因为是搜索树，我们可以通过中序遍历将节点的值加入到一个数组，那么这个数组将会是一个递增数组，最后通过这个数组即可找到第k大节点。

## [剑指 Offer 55 - I. 二叉树的深度](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/)

这一题直接使用分治递归，先算出左边子树的高度，再算出右边子树的高度，比较一下，哪一个大加一即可。然后递归的进行这样操作

## [剑指 Offer 55 - II. 平衡二叉树](https://leetcode.cn/problems/ping-heng-er-cha-shu-lcof/)

平衡二叉树要求左右子树的高度不能超过1，我们还是和上一题一样使用分治递归法，先算出左子树的最大深度，再算出右子树的最大深度，然后要求差值绝对值不能大于1，然后递归，左边也得是平衡二叉树，右边也得是平衡二叉树。

## [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

这一题使用的是位运算，具体见位运算目录下的文件。

## [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

这一题使用的是位运算，具体见位运算目录下的文件。

## [剑指 Offer 57. 和为s的两个数字](https://leetcode.cn/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

从一个升序排列的数组找到两个和为target的数。我们使用的是双指针法，左指针和右指针对应的两个数之和与target进行比较，如果大于的话，那么右指针左移动；如果小于target的话，左指针右移动，等于break，返回结果即可。

## [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

这一题使用的是滑动窗口，我们维护一个窗口，在这个窗口里的值总和，如果小于target，右边界移动，和增加，如果大于target，左边界移动，和减小，如果大于等于target，返回结果，然后继续左边界右移动，总和减小，继续寻找其他的结果。

## [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

这一题使用的是双指针。具体见`数组_双指针_滑动窗口`目录下的文件

## [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

直接用一个字符串加入n到最后，再加上到n即可。

## [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

这一题使用的方法是单调双向队列，具体见`数组_双指针_滑动窗口`目录。

## [剑指 Offer 59 - II. 队列的最大值](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/)

用两个队列来实现，一个队列单纯入队，一个队列是单调队列，使得每次队头都是最大值

- push的时候，正常队列直接入队即可，单调队列则需要从队尾判断是否小于将要入队的值，如果小于的话，那么就需要从队尾出队，这就意味着这个队列是一个双向队列，既可以从队尾出队，也可以正常从队头出队
- pop的时候，正常队列每次直接出队即可，单调队列则需要判断正常队列的值和自己的队头是否相等，如果相等的话就要pop出去

## [剑指 Offer 60. n个骰子的点数](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/)

这一题使用的方法是动态规划，具体见`动态规划`目录。

## [剑指 Offer 61. 扑克牌中的顺子](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

这一题要求给的五张牌是一个顺子，也就是连续的数字，而大王小王是0，可以变成任意的值让他们变成顺子。很容易知道除了0之外的其余所有数字，最大值和最小值相差需要小于5才能保证是顺子，另外还需要保证不能有重复的数字出现（除零之外），可以用哈希表进行判断。

## [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

这是一个数学问题，具体见`数学问题`目录

## [剑指 Offer 63. 股票的最大利润](https://leetcode.cn/problems/gu-piao-de-zui-da-li-run-lcof/)

给定一个利润数组，要求我们求出最大股票利润。我们知道的是最大利润应该是之后的利润减去之前的利润的最大差值，我们可以遍历数组，先求出当前最大利润，就是当前利润减去之前的最小利润，然后再求出最小利润，不断的去更新这两个值，最后返回最大利润即可。

## [剑指 Offer 64. 求1+2+…+n](https://leetcode.cn/problems/qiu-12n-lcof/)

这是一道小学数学题目。直接返回 `(1+n)*n/2`即可。

## [剑指 Offer 65. 不用加减乘除做加法](https://leetcode.cn/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

这是一道位运算题目，具体见`位运算`目录。

## [剑指 Offer 66. 构建乘积数组](https://leetcode.cn/problems/gou-jian-cheng-ji-shu-zu-lcof/)

要求我们构造一个乘积数组，每一个值是除了当前数组的值外其他所有的相乘。每个值在自己这个下标的时候就是乘以1，所以就形成了上下两个三角形，我们只需要相乘就好

1. 第一步遍历数组，直接赋值`res[i] = res[i-1] * a[i-1]`
2. 第二步再次遍历数组，用一个辅助来保存值，最后再和res数组的值相乘

## [剑指 Offer 67. 把字符串转换成整数](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

用一个标志来表示正负号，一开始先处理之前的字符，如果是空格就跳过，如果是负号，标志变为-1，然后遍历字符串，`res=res*10+int(str[i]-'0')`,str[i]='0'主要是将数字字符化为数字，然后判断res是否超出32位整数范围，最后返回`flag*res`

## [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

直接判断两个节点和根节点的大小，如果都大于，说明两个节点最近公共祖先在右子树；如果都小于，说明两个节点最近公共祖先在左子树，否者就是根节点，然后递归即可。

## [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

分别在左子树和右子树递归的查找两个节点，如果左右两边都找到的话，说明两个节点在左右两子树，返回root，如果只在左子树找到，返回left即可，在右子树返回右即可。
