# 45. 跳跃游戏 II
经过 [983. 最低票价](https://leetcode-cn.com/problems/minimum-cost-for-tickets/) 的摧残 ,  这道题看上去简单多了 . 虽然难度是困难 , 但是没感觉比983更麻烦.

刚开始的时候不知道为什么 , 感觉从前往后推比较麻烦 , 想了想就从后往前推了

## 从后往前推

```java
public int jump(int[] nums) {
    int pre = nums.length - 1, count = 0;
    while (pre > 0) {
        int minIndex = Integer.MAX_VALUE;
        for (int j = pre - 1; j >= 0; j--) {
            if (nums[j] + j >= pre) {
                minIndex = j;
            }
        }
        count++;
        pre = minIndex;
    }
    return count;
}
```

从后往前推的话 ,每次需要把剩余的步骤再重新遍历一次  , 所以复杂度的确比较高 .提交之后显示耗时比较高 . 所以想换种思路实现

## 从前往后推

想了想 ,发现从前往后推也不算很麻烦 ,思路如下:

* 确定每一步青蛙可以出现的下标 ,  如起始的时候只能出现在0这个位置 ,但是第一步之后就可以出现在[1,a[0]] 这个区间中
* 确定下一步能达到的最大下标 , 结合本次的上限 ,确定下一次的范围
* 重复前两个步骤 ,直到能达到的最大坐标不小于数组最大坐标

代码如下: 

```java
public int jump(int[] nums) {
    int first = 0, last = 0, count = 0;
    while (last < nums.length - 1) {
        int max = 0;
        for (int i = first; i <= last; i++) {
            int temp = nums[i] + i;
            if (temp > max) {
                max = temp;
            }
        }
        first = last + 1;
        last = max;
        count++;
    }
    return count;
}
```

## 总结

两种dp思路 , 耗时差距还是挺明显的 .有时候还是要多考虑一下复杂度问题 , 另外两个注意的点

* dp的初始状态和 转移方程要注意 , 避免错误的累加
* 数组长度 ,数组下标 一定要区分清楚 .避免混用.