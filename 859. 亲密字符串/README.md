# [859. 亲密字符串](https://leetcode-cn.com/problems/buddy-strings/)

这道题是我在leetcode上面做的第二道题. 

比较简单 , 但是同时也有一些小的需要注意的地方 

* 长度大于1的字符串与其本身也构成亲密字符串(此处需要注意 , 避免踩坑)

* 如果两个字符串相等 , 则只需判断字符串中是否包含重复字母  . 如果字符串长度大于26 ,则可以构成条件 , 如果小于26 , 则需要进一步判断 , 这里由于元素的范围即英文字母已知 , 则可以使用数组判断是否重复
* 两个字符串不相等就没有太多技巧了 , 只能按位比较了

``` java
class Solution {
  public boolean buddyStrings(String A, String B) {
        if (A.length() != B.length()) {
            return false;
        }
        if (A.equals(B)) {
            if (A.length() > 26) {
                return true;
            } else {
                boolean[] count = new boolean[26];
                for (int i = 0; i < A.length(); i++) {
                    int value = A.charAt(i) - 'a';
                    if (count[value]) {
                        return true;
                    } else {
                        count[value] = true;
                    }
                }
                return false;
            }
        }
        int pre = -1, after = -1;
        for (int i = 0; i < A.length(); i++) {
            if (A.charAt(i) != B.charAt(i)) {
                if (pre == -1) {
                    pre = i;
                } else if (after == -1) {
                    after = i;
                } else {
                    return false;
                }
            }
        }
        return after > 0 && A.charAt(pre) == B.charAt(after) && B.charAt(pre) == A.charAt(after);
    }
}
```

