---
title: 最长回文子串Manacher算法
cover: "../image/1.jpg"
--- 


数据预处理： 首先回文子串有两种形式 奇数 与 偶数 也就有两种对应的指针操作方式

假定有字符数组 ababaabc 改成 # a # b # a # b # a # a # b # c # 将偶数数组变成奇数统一处理 索性改成 ^ # a # b # a # b # a # a # b # c # $，头尾清晰
这样就可以通过把每个字符作为回文子串的中心向两边扩展，找出最长回文子串 时间复杂度是 O(n^2)

现在需要我们观察回文子串的规律，简化计算

``` css
        0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8
        ^ # a # b # a # b # a # a # b # c # $
  0     ^                                      P[0]  = 0
  1       ^                                    P[1]  = 0
  2       --^--                                p[2]  = 1
  3           ^                                P[3]  = 0
  4       ------^------                        P[4]  = 3
  5               ^                            P[5]  = 0
  6       ----------^----------                P[6]  = 5
  7                   ^                        P[7]  = 0
  8               ------^------                P[8]  = 3
  9                       ^                    P[9]  = 0
  10                      --^--                P[10] = 1
  11                  --------^--------        P[11] = 2
  12                          --^--            P[12] = 0
  13                              ^            P[13] = 0
  14                              --^--        P[14] = 1
  15                                  ^        P[15] = 0
  16                                  --^--    P[16] = 1
  17                                      ^    P[17] = 0
  18                                        ^  P[18] = 0 
情况一：
``` css
  10                      --^--                P[10] = 1
  11                  --------^--------        P[11] = 2
  12                          --^--            P[12] = 0
```

第 10 行，第 12 行 都是 第 11 行 的子串，完全包含在 第 11 行 之中，由于回文串的对称性 此时直接有 P[10] = P[12]

情况二：
``` css
  4       ------^------                        P[4]  = 3
  6       ----------^----------                P[6]  = 5
  8               ------^------                P[8]  = 3
```
第 4 行，第 8 行 都是 第 6 行 的子串，分别位于字符串的两端，当我们知道 第 4 行 的信息之后，我们知道 第 8 行 至少有 第 4 行 那么长，至于会不会更长，继续试着向两边扩展即可 此时需要干两件事 1. 将 P[8] = P[4]; 2. 继续向外扩展

情况三： 8 ------^------ P[8] = 3 11 --------^-------- P[11] = 2 14 --^-- P[14] = 1 第 8 行 部分与 第 11 行 重叠，第 14 行 是 第 11 行 的子串，非常简单，舍弃超出部分 8 --^-- P[8] = 3 11 --------^-------- P[11] = 2 14 --^-- P[14] = 1 当成这样处理即可 定义遍历指针 i ，指向回文的中心的指针 center 和 指向回文串右边界的指针 r 此时需要干两件事 1. 将 P[8] = r - i ; 2. 继续向外扩展

好，我们现在已经理解了 Manacher 算法的精髓了 我们思考一下算法该怎么写

P[i] 计算过程
``` css
        0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8
        ^ # a # b # a # b # a # a # b # c # $
  0     ^                                      P[0]  = 0  -- 起始，不必计算，更新 r
  1       ^                                    P[1]  = 0  -- 暴力计算，一次计算，更新 r
  2       --^--                                p[2]  = 1  -- 暴力计算，二次计算，更新 r
  3           ^                                P[3]  = 0  -- 情况二，一次计算
  4       ------^------                        P[4]  = 3  -- 暴力计算，四次计算，更新 r
  5               ^                            P[5]  = 0  -- 情况一，零次计算
  6       ----------^----------                P[6]  = 5  -- 情况二，六次计算，更新 r
  7                   ^                        P[7]  = 0  -- 情况一，零次计算
  8               ------^------                P[8]  = 3  -- 情况二，一次计算
  9                       ^                    P[9]  = 0  -- 情况一，零次计算
  10                      --^--                P[10] = 1  -- 情况二，一次计算
  11                  --------^--------        P[11] = 2  -- 情况二，五次计算，更新 r
  12                          --^--            P[12] = 0  -- 情况一，零次计算
  13                              ^            P[13] = 0  -- 情况一，零次计算
  14                              --^--        P[14] = 1  -- 情况三，一次计算
  15                                  ^        P[15] = 0  -- 情况二，一次计算
  16                                  --^--    P[16] = 1  -- 暴力计算，两次计算，更新 r
  17                                      ^    P[17] = 0  -- 情况二，一次计算
  18                                        ^  P[18] = 0  -- 终止
```
我列出了每次计算面对的情况，计算的次数以及是否需要 r 我希望大家思考 当新计算出的 r 与旧的 r 相等时，是否应该更新 center ？ 当然不应该，我们肯定更倾向于选择更长的回文串

是这样吗？ 我们思考一种情况
``` css
  2       --^--                                p[2]  = 1
  3           ^                                P[3]  = 0
  4       ------^------                        P[4]  = 3
  5               ^                            P[5]  = 0
  6       ----------^----------                P[6]  = 5
  7                   ^                        P[7]  = 0
  8               ------^------                P[8]  = 3
  9                       ^                    P[9]  = 0
  10                      --^--                P[10] = 1 
```
第 6 行 较长有什么用呢，有用的只是 i 到 r 这一小截而已，不更新是因为都一样，没必要更新 所以只有当我们发现了更右边的 r 更新即可

``` kotlin
class Solution {

    private fun formatString(s: String): String{
        val tString : StringBuffer = StringBuffer("^#")
        for ( i in s.indices ){
            tString.append(s[i])
            tString.append('#')
        }
        return tString.append('$').toString()
    }
    
    private fun extend(s: String, leftIndex: Int, rightIndex: Int): Int {
        var r = 0
        var left = leftIndex
        var right = rightIndex
        while (left > 0 && right < s.length && s[left] == s[right]){
           r ++
           left --
           right ++
        }
        return r
    }
    
    fun longestPalindrome(s: String): String {
        val tString = formatString(s)
        var maxR = 0
        var maxCenter = 0
        var center = -1
        var R = 0
        val p = Array<Int>(tString.length) { 0 }
    
        for ( i in tString.indices ){
            val iMirror = center - (i - center)
            var rightIndex = i
            var leftIndex = i
            val maxLength = R - i
    
            val hasMirrorIndex = i < R && center - maxLength > 0
            val case1CompletelyIncluded = hasMirrorIndex && i > center && p[iMirror] < R - i
            val case2NotCompletelyInclude = hasMirrorIndex && !case1CompletelyIncluded
            
            if(!hasMirrorIndex){
                p[i] = 0
                rightIndex = i + 1
                leftIndex = i - 1
            } else {
                if(case1CompletelyIncluded) {
                    p[i] = p[iMirror]
                    continue
                } else if (case2NotCompletelyInclude) {
                    p[i] = maxLength
                    rightIndex = R + 1
                    leftIndex = i - (rightIndex - i)
                }
            }
            
            p[i] += extend(tString, leftIndex, rightIndex)
            
            // 更新最右边的 R 和 center
            if(i + p[i] > R){
                R = i + p[i]
                center = i
            }
            
            // 判断是不是最长的回文串
            if(p[i] > maxR){
                maxR = p[i]
                maxCenter = i
            }
        }
    
        if(maxR == 0) return ""
    
        val start = maxCenter - maxR
        val end = maxCenter + maxR
        return tString.substring(start..end).replace("#", "")
    }
}
```