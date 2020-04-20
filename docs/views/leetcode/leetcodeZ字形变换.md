---
title: leetcode Z字形变换
date: 2020-04-20
tags:
 - leetcode
categories:
 - 技术分享
---

# leetcode Z字形变换

## 题目

> 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
>
> 比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
>
> L   C   I   R
> E T O E S I I G
> E   D   H   N
> 之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
>
> 请你实现这个将字符串进行指定行数变换的函数：
>
> string convert(string s, int numRows);
> 示例 1:
>
> 输入: s = "LEETCODEISHIRING", numRows = 3
> 输出: "LCIRETOESIIGEDHN"
> 示例 2:
>
> 输入: s = "LEETCODEISHIRING", numRows = 4
> 输出: "LDREOEIIECIHNTSG"
> 解释:
>
> L     D     R
> E   O E   I I
> E C   I H   N
> T     S     G
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/zigzag-conversion

## 解决过程

### 方法一

利用数列作为临时转换空间

```java
class Solution {
    public String convert(String s, int numRows) {
        //特殊判定
        if(numRows < 2) return s;
        //初始化空间
        List<StringBuilder> rows = new ArrayList<StringBuilder>();
        for(int i = 0; i < numRows; i++) rows.add(new StringBuilder());
        
        //输入数据 把s输入到rows
        int i = 0, flag = -1;
        for(char c : s.toCharArray()) {
            rows.get(i).append(c);
            if(i == 0 || i == numRows -1) flag = - flag;
            i += flag;
        }
        //整理数据 把rows输入到res
        StringBuilder res = new StringBuilder();
        for(StringBuilder row : rows) res.append(row);
        return res.toString();
    }
}


```

### 方法二

利用规律（算法）

#### 分析

我们先假定有 numRows=4 行来推导下，其中 2*numRows-2 = 6 , 我们可以假定为 step=2*numRows-2 ，我们先来推导下规则：

![img](https://pic.leetcode-cn.com/d610b140dd0789204efe699672dc72a83e7b826da0165bbf083d24fc97ecdea7-image.png)

第0行： 0 - 6 - 12 - 18

==> 下标间距 6 - 6 - 6 ==> 下标间距 step - step - step

第1行： 1 - 5 - 7 - 11 - 13

==> 下标间距 4 - 2 - 4 - 2 ==> 下标间距step-2*1(行)-2*1(行)-step-2*1(行)-2*1(行)

第2行： 2 - 4 - 8 - 10 - 14
==> 下标间距 2 - 4 - 2 - 4 ==> 下标间距step-2*2(行)-2*2(行)-step-2*2(行)-2*2(行)

第3行：3 - 9 - 15 - 21

==> 下标间距间距 6 - 6 - 6 ==>下标间距step - step - step

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1){
            return s;
        }
        int k = 2*numRows - 2;
        char[] chars = new char[s.length()];
        int idx = 0;
        //第一行 间隔 0 + 2*numRows - 2 
        for(int j = 0;j < s.length();j+=k){
            chars[idx] = s.charAt(j);
            idx++;
        }
        
        //中间行 间隔 k - 2 * i
        for(int i = 1;i < numRows-1;i++){
            int j = i;
            int interval = k-2*i;
            while(j<s.length()){
                chars[idx] = s.charAt(j);
                idx++;
                j+=interval;
                interval = k - interval;
            }
        }

        //最后一行 间隔 0 + 2*numRows - 2 
        for(int j = numRows - 1;j<s.length();j+=k){
            chars[idx] = s.charAt(j);
            idx++;
        }

        return new String(chars);
    }
}
```

