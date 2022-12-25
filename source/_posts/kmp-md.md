---
title: KMP算法
tags: [算法, 字符串]
categories: 算法
date: 2022-12-20 12:00:00
typora-root-url: kmp-md
top: [0]
---
一个高效经典的字符串匹配算法，使用空间换时间的方式，目的, 对于**模式串 t**， 找到它在**目标串 s** 中第一个出现的位置。例如 t = "abcab", s = "ababcabd", 算法就应该返回 2为他第一次出现的位置。

**传入参数**

```java
/**
* 传入char字符作为测试
* @param s 目标字符串
* @param t 模式字符串
* @return
*/
```

# 暴力匹配

```java
public int search(String s, String t){
        int j;
        for(int i = 0; i <= s.length() - t.length(); i++){
            for(j = 0; j < t.length(); j++){
                if(s.charAt(i + j) != t.charAt(j)){
                    break;
                }
            }
            if(j == t.length()){
                return i;
            }
        }
        return -1;
    }
```

对于暴力匹配，如果s中未出现匹配的字符串，则它需要同时回退s和t的指针，对之前已经匹配过的字符重新进行匹配，嵌套 for 循环，时间复杂度 O(MN)，空间复杂度O(1)

<!--more-->

# Kmp算法

**核心：部分匹配表(Partial Match Table)** 字符串前缀与后缀的交集中最长元素的长度，称为PMT表。

**最大公共前后缀：** 以abcab例：他的前缀为{a,ab,abc,abca}; 后缀为{b,ab,cab,bcab}。则他们的最大公共前后缀为：ab

对于字符串 t: abcababc，他的PMT如下表表示

| char:  | a    | b    | c    | a    | b    | a    | b    | c    |
| ------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| index: | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    |
| value: | 0    | 0    | 0    | 1    | 2    | 1    | 2    | 3    |

模式字符串长度为8，PMT就会有8个值

PMT的值：例如 index = 4 字符串为 abcab，最大公共前后缀为 ab，长度为2，所以 value = 2。index = 7，最大公共前后缀为 abc,长度为3，value = 3，PMT表中的值就可以按这样获得

如何使用PMT表来实现字符串的匹配

首先，PMT表的值是根据模式串来进行获得的，以上表的字符串ｔ为例子。设模式串指针为 j，目标字符串指针为 i，将模式串和目标字符串的字符一个一个的匹配。根据 PMT 获得的Value值的性质可知，t[value] 之前的值与t[j - value]之后的字符都相等。

![kmp01](kmp01.png)

当遇见 i 与 j 相对应的字符不匹配时，当前 j = 5，回到上一个字符对应的 j = 4 的 Value = 2值可得，也就是令 j = 2。就有如下效果

![kmp02](kmp02.png)

这样就可以达到快速匹配的效果，我们还可以这样优化，因为每次取得 j 为上一次的的值，可以将next数组整体往右移一位，创建一个next数组，记录每一个value值与j，作为kmp的next数组来达到以上的效果。

| char:  | a    | b    | c    | a    | b    | a    | b    | c    |
| ------ | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| index: | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    |
| value: | 0    | 0    | 0    | 1    | 2    | 1    | 2    | 3    |
| next:  | -1   | 0    | 0    | 0    | 1    | 2    | 1    | 2    |

**next数组**

```java
public void getNext(int[] next, char[] chars){
    int i = 0, t = -1;
    next[i] = t;
    while(i < next.length){
        if(t == -1 || chars[i] == chars[t]){
            next[++i] = ++t;
        } else {
            t = next[t];
        }
    }
}
```

> <!--To-->原理待说

**kmp算法**

```java
public int searchKmp(String s, String t){
    int slen = s.length(), tlen = t.length(), j = 0, i = 0;
    int[] next = getNext(t.toCharArray());
    while(i < slen || j < tlen){
        if(j == -1 || s.charAt(i) == t.charAt(j)){
            i++;
            j++;
        }else{
            j = next[j];
        }
    }
    if(j == tlen){
        return i - j;
    }
    return -1;
}
```

**例题：**[28. 找出字符串中第一个匹配项的下标 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)
