# leet-code
## 涉及到的知识点

### 图

* 并查集

* dfs/bfs

* Floyd最短路径

  需要注意是中专点不变 , 起点和终点变化; 即起点再最外层

* 拓扑排序

  可以用来判断是否有环

* 邻接表/邻接矩阵

* 快慢指针

* 链表是否有环 , 及入环点

### 链表/数组

* 滑动窗
* 双指针

### 其他

* 动态规划
* 贪心
* 位运算

## 题解

### [45.跳跃游戏](./45. 跳跃游戏 II\README.md)

### [146.LRU缓存机制](./146. LRU缓存机制/README.md)

需要自己实现链表 , 头尾元素先固定能减轻判断

### [287. 寻找重复数](./287. 寻找重复数README.md)

几种知识点拼起来的一道题 ,快慢指针,链表入环这些知识都有涉及到.自己解开之后还是很开心

### [859. 亲密字符串](./859. 亲密字符串/README.md)

### [5425. 切割后面积最大的蛋糕](./5425. 切割后面积最大的蛋糕/README.md)

题不算难, int溢出/位运算优先级各被坑了一次 , 这个需要注意

### [1462. 课程安排 IV](./1462. 课程安排 IV/README.md)

现在想起来这道题是拓扑排序的经典应用 , 答案是自己想出来的, 前前后后经历了七八个大小版本 三种思路 ,幸运的是最后还是想到了最优解

### [198. 打家劫舍](./198. 打家劫舍/README.md)

动态规划基础入门

## TODO

### 待理解

- [ ]  [5188. 树节点的第 K 个祖先](https://leetcode-cn.com/problems/kth-ancestor-of-a-tree-node/)

- [x]  [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

  没有很好的方法 , 复杂度都是基于O(n²) , 我想用二分法反而增加了复杂度 . 不过一些细节可以优化 , 比如去重, 记录上次的边界

- [ ]  [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

  用栈实现了, 但是效率不高 , 思考用动态规划实现 , 以及思考多种括号

- [ ]  [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

  被一个简单题虐了N遍. 

- [x]  [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

  只用了暴力实现 ,前缀和也需要尝试下 ; 其实就是[437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/) 拆出来的问题.

- [ ]  [1456. 定长子串中元音的最大数目](https://leetcode-cn.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

  复杂度不理想

- [x] [5437. 不同整数的最少数目](https://leetcode-cn.com/problems/least-number-of-unique-integers-after-k-removals/)

  复杂度不理想 

  思路大体上没问题 , 但是其中存在很多的优化空间

- [ ]  [1466. 重新规划路线](https://leetcode-cn.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/)

  复杂度不理想

- [x] [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

  复杂度不理想

  很典型的拓扑排序, 不过优化后的dfs 其实也可以实现. 不过理解起来需要耗费一点精力

- [x] [1471. 数组中的 k 个最强值](https://leetcode-cn.com/problems/the-k-strongest-values-in-an-array/)

  复杂度不理想

  先要排序 ,其次要思考left和right指针的关系 :`arr[left]<=arr[right]` 所以不需要使用绝对值判断.

- [x]  [1457. 二叉树中的伪回文路径](https://leetcode-cn.com/problems/pseudo-palindromic-paths-in-a-binary-tree/)

  复杂度不理想
  
  利用了位运算, 
  
- [ ] [1477. 找两个和为目标值且不重叠的子数组](https://leetcode-cn.com/problems/find-two-non-overlapping-sub-arrays-each-with-target-sum/)

- [ ] [1478. 安排邮筒](https://leetcode-cn.com/problems/allocate-mailboxes/)

- [ ] [1473. 给房子涂色 III](https://leetcode-cn.com/problems/paint-house-iii/)

- [ ]  [1458. 两个子序列的最大点积](https://leetcode-cn.com/problems/max-dot-product-of-two-subsequences/)


### 待写文档

- [ ]  [1449. 数位成本和为目标值的最大数字](https://leetcode-cn.com/problems/form-largest-integer-with-digits-that-add-up-to-target/)

  虽然自己也实现了一边 , 中间也有一些小优化并感到沾沾自喜 , 但不得不说 ,和最优解比起来还是相形见绌

- [ ]  [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

  经典滑动窗

- [ ]  [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

  这道题替换的思想还是没想到

- [ ]  [319. 灯泡开关](https://leetcode-cn.com/problems/bulb-switcher/)

  很巧妙的脑筋急转弯

- [ ]  [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

  又一个滑动窗

- [ ]  [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

  这个题其实真的不简单

- [ ]  [1371. 每个元音包含偶数次的最长子字符串](https://leetcode-cn.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/)

  貌似别人的回答又非常优秀
