---
title: leetcode-395
date: 2022-05-20 02:38:27
tags:
mathjax: true
---

### 题目描述

给你一个字符串 `s` 和一个整数 `k` ，请你找出 `s` 中的最长子串， 要求该子串中的每一字符出现次数都不少于 `k` 。返回这一子串的长度。

#### 示例1

```
输入：s = "aaabb", k = 3
输出：3
解释：最长子串为 "aaa" ，其中 'a' 重复了 3 次。
```

#### 示例2

```
输入：s = "ababbc", k = 2
输出：5
解释：最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```

#### 提示

```
- 1 <= s.length <= 104
- s 仅由小写英文字母组成
- 1 <= k <= 105
```



### 解题思路

至少有 K 个重复字符的最长子串，

- 由于字符串是连续的，因此不满足题意的字符串一定包含少于k个的字符，若其内部包含满足题意的子串，正是被这少于k个的字符所间隔开的；

- 从上述总结的题目特点，可以找寻判断字符串的层次顺序，
- 因此判断当前输入s，若不满足题意，按照其中一个少于k个的字符t将其分割为若干子串，进而判断分割后的子串是否满足题意，即所有字符个数是否均大于等于k，即将其一个“大问题”分治为若干“子问题”；
- 若当前输入s不满足题意，且存在m个少于k个的字符t（m > 0），无论先按照m中的哪一个字符分割输入s，都是等价的；因为无论用哪个t分割，下钻到哪一层，对于少于k个的字符t，在s的子串中的个数都是小于k的，因此都是作为分割字符进行切分子串进行下钻，判断其子串的；满足题意的子串都是不包含这些少于k个的字符t的，无非是选择分割字符的顺序影响了下钻当前子串的过程，但不影响当前满足题意的子串本身的属性（字符串内容），即不影响求解满足题意的子串长度；
- 由于题解要求出最长子串的长度，因此求出分割后的各个满足题意的子串长度（子问题），取最大值即可。



### 代码

```Python3
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:
        if len(s) < k:
            return 0

        for c in set(s):
            if s.count(c) < k:
                return max(self.longestSubstring(_s, k) for _s in s.split(c) if _s)
        return len(s)
```



### 复杂度分析

- 时间复杂度： 
  $\omicron\big(N\times\mid\Sigma\mid\big)$ 
  其中 $N$ 为字符串的长度，$\Sigma$ 为字符集，本题字符串仅包含小写因此小写字符集的模$\mid\Sigma\mid = 26$。由于每次进入下一层（下钻）也就是进入递归之前，进行**完全去除某种字符**在题目中表现为去除不符题意的字符，进而实现分治，进入下一层递归重复此操作，可见递归最大深度为$\mid\Sigma\mid$。

- 空间复杂度：$\omicron\big(\mid\Sigma\mid^2\big)$ 。空间复杂度则是所有层递归所开辟的空间，因此计算为"每层递归开辟的空间 $\times$ 递归层数"，每层递归需要遍历字符集大小的空间即为$\mid\Sigma\mid$。
