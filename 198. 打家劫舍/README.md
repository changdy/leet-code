# 198. 打家劫舍

这是一道很基础的动态规划 ,  做起来并不难 ,根据题意即可知当偷第N个房间的时候 ,思考前一个节点应该是N-2 还是N-3.初版代码如下

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        return calculate(0, 0, 0, nums, 0);
    }

    public int calculate(int former, int mid, int after, int[] nums, int index) {
        if (index >= nums.length) {
            return Math.max(mid, after);
        }
        return calculate(mid, after, Math.max(nums[index] + mid, nums[index] + former), nums, index + 1);
    }
}
```

这样写虽然也是O(n)的时间 ,但是用了递归 , 并且变量有些多 ,我在解答中找到了一个更好的答案:

```java 
class Solution {
    public int rob(int[] nums) {
        int first = 0;
        int second = 0;
        for (int i : nums) {
            int temp = second;
            second = Math.max(second, first + i);
            first = temp;
        }
        return second;
    }
}
```

对比下代码就能理解思想上的差异

我的代码是题意的直译 , 思考的是 第N个节点是和(N-2) 还是(N-3)结合 , 但是解答中的代码对题意进行了再次加工 , 他思考的是 , 每个位置能达到的最大值是多少 , 因此第N个节点 能获得大最大值就是 `Math.max(sum(N-2)+nums(N),sum(N-1))` .

其实第一次看这个答案还是有些不解 , 总感觉这个这样写有些不对,但是理解之后才会发现他的精妙之处 . 这个答案是一直再进行一个剪枝的操作 , 并且这种思路其实也挺常见的在  [983. 最低票价](https://leetcode-cn.com/problems/minimum-cost-for-tickets/) 中 , 也是计算的每一天的最低票价.

