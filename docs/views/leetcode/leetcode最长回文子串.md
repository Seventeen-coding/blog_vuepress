---
title: leetcode最长回文子串
date: 2020-04-20
tags:
 - leetcode
categories:
 - 技术分享
---

# leetcode最长回文子串

## 题目

> 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
>
> 示例 1：
>
> 输入: "babad"
> 输出: "bab"
> 注意: "aba" 也是一个有效答案。
> 示例 2：
>
> 输入: "cbbd"
> 输出: "bb"
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/longest-palindromic-substring

## 概念

回文串(palindromic string)是指这个字符串无论从左读还是从右读,所读的顺序是一样的;简而言之,回文串是左右对称的。

## 要求

输入一个字符串

输出一个回文字符串

## 解决过程

### 方法一(穷举法，暴力破解)

设置一个字符串长度为i,偏移量j，循环判断s[j,i+j]是否为回文数 

```java
class Solution {
    public String longestPalindrome(String s) {
        String ret = "";
        for (int i = s.length(); i > 0; i--)
        {
            for (int j = 0; j + i - 1 < s.length(); j++) 
            {
                String test = s.substring(j, i + j);
                if( isPalindrome(test) == true)
                {
                    ret = test;
                    return ret;
                }
            }
        }
        return ret;
    }
    public boolean  isPalindrome(String s) {
        for(int i = 0; i < s.length()/2;i++)
        {
		    if (s.charAt(i) != s.charAt( s.length() - i - 1)) 
            {
				return false;
			}
        }
        return true;
    }
}
```

### 别人方法二

还没看懂

```java
class Solution {
    public String longestPalindrome(String s) {
        //特殊判定
        if(s == null || s.length() == 0){
            return "";
        }

        char[] str = s.toCharArray();
        int [] range = new int[2];

        for(int i =0; i < str.length;i++){
            i = findCenterIndex(str,i,range);
        }

        return s.substring(range[0],range[1]+1);
    }

    private int findCenterIndex(char[] str,int low,int[] range){
        int hight = low;
        
        while(hight < str.length -1 && str[hight+1]== str[low]){
            hight++;
        }

        int ans = hight;

        while(low > 0 && hight < str.length -1 && str[low -1] == str[hight+1] ){
             hight++;
             low--;
        }

        if(hight - low > range[1] - range[0]){
            range[0] = low;
            range[1] = hight;
        }
        return ans;
    }
}
```

### 别人方法三

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.isEmpty()){
            return "";
        }
        int len = s.length();
        int N = 2 * len + 3;
        // 初始化数组方法1
        char[] chars = new char[N];
        chars[0] = '@';
        chars[N - 2] = '#';
        chars[N - 1] = '$';
        for(int i = 1; i < N - 2; i += 2){
            chars[i] = '#';
            chars[i + 1] = s.charAt(i/2);
        }
        
        int[] radius = new int[N];
        int right = 0;
        int center = 0;
        int result = 0;
        int maxRight = 0;
        int maxCenter = 0;

        for(int i = 1; i < N - 1; i++){
            if(right > i){
                radius[i] = Math.min(right - i, radius[2 * center - i]);
            } else{
                radius[i] = 1;
            }
            while(chars[i + radius[i]] == chars[i - radius[i]]){
                radius[i] ++;
            }
            if(i + radius[i] > right){
                right = i + radius[i];
                center = i;
                if(radius[maxCenter] < radius[center]){
                    maxCenter = center;
                    maxRight = right;
                }
            }
            result = Math.max(result, radius[i]);
        }
        result -= 1;
        String[] ans = String.valueOf(chars).substring(maxCenter - result, maxRight).split("#");
        StringBuilder ansBuild = new StringBuilder();
        for(int i = 0; i < ans.length; i++){
            ansBuild.append(ans[i]);
        }
        return ansBuild.toString();
    }
}
```

### 别人方法三（中心扩展法）

![图 1 ：奇数回文串与偶数回文串](https://pic.leetcode-cn.com/572db4731d6a0e32ee9c14773ed476068bebb88883335bc7415cb0b43762303a.jpg)

```java
class Solution {
    //中心扩展法
    public String longestPalindrome(String s) {
        if (s==null||s.length()==0) return "";
        int l=0,r=0;
        //以偏移中心位置，计算最长回文字符
        for (int i = 0; i < s.length(); i++) {
            //奇数
            int len1=centerExtension(s, i, i);
            //偶数 偶数中心左右两个数 右边用即边+1
            int len2=centerExtension(s, i, i+1);
            //采用最长的回文数 并记录左右下标位置
            int len=Math.max(len1, len2);
            if ( len > r - l){
                l = i - (len-1)/2;
                r = i + len/2;
            }
        }
        //返回结果
        return s.substring(l, r+1);
    }

    private int centerExtension(String s,int left,int right){
        //比较最左右两边是否对等，不断扩散
        while (left >= 0 && right<s.length() && s.charAt(left) == s.charAt(right)){
            left--;
            right++;
        }
       return right-left-1;
    }
}
```

