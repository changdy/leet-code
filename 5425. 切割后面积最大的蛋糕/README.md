#### 5425. 切割后面积最大的蛋糕

代码不多解释了,先排序,然后求出两边的最大间隔即可 ,策略上使用了贪心
```java
class Solution {
    public int K = 1_000_000_007;
    public int maxArea(int h, int w, int[] horizontalCuts, int[] verticalCuts) {
        long hMax = getMaxGutter(h, horizontalCuts) % K;
        long wMax = getMaxGutter(w, verticalCuts) % K;
        return (int) ((hMax * wMax) % K);
    }

    public long getMaxGutter(int number, int[] arr) {
        Arrays.sort(arr);
        if (arr.length > 0) {
            int max = arr[0];
            for (int i = 1; i < arr.length; i++) {
                int temp = arr[i] - arr[i - 1];
                if (temp > max) {
                    max = temp;
                }
            }
            return Math.max(max, number - arr[arr.length - 1]);
        } else {
            return number;
        }
    }
}
```

但是这道题却是第191场周赛里通过率最低的 , 包括我也栽了,早早的写了代码, 反复提交都是错的, 甚至怀疑人生.结束后舍友和我说Bigdecimal , 瞬间恍然大悟,猜测可能发生了溢出.通过之后这道题我感觉有两个坑:

* int相乘有可能会发生溢出

* 运算符优先级需要注意

先说第一个,比赛时, 我分别求出最大间隔之后,先进行了求余操作,之后再次相乘.但是相乘仍有可能发生溢出.因为两个10^9 + 7相乘远远大于2^31 , 后者是个2开头的10位数字而前者是1开头的19位数字, 因此相乘前一定要先转成long类型

再说第二个, 朋友和我说完之后 我匆匆忙忙把int改成了long , 代码如下:

```java
public int maxArea(int h, int w, int[] horizontalCuts, int[] verticalCuts) {
    long hMax = getMaxGutter(h, horizontalCuts) % K;
    long wMax = getMaxGutter(w, verticalCuts) % K;
    return (int) (hMax * wMax) % K;
}
```


能看出来哪里有问题吗? 在java中强转的优先级是比较高的,这里`return`的时候是先计算了两个long类型的乘积,转成了int之后再求余,而我想要的是先乘积再求余,最后转成int.修改后的代码如下
```java
 // return (int) (hMax * wMax) % K;
    return (int) ((hMax * wMax) % K);
```
另外,如果一道题,当数字较小时运行正确 , 较大时不正确 ,也需要注意下是否发生了溢出.

最后在刷算法题时,会碰到在业务逻辑中很难见到的位运算,强转运算;尤其是两者结合的时候一定要注意其优先级.前两天还被int i = 0;8 << 1 + ++i;虐的生活不能自理.但是如果你也不能立刻说出答案,或许也需要补习下优先级的知识.