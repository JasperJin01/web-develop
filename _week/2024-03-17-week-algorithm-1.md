---
title: 数据结构&算法（1）
date: 2024-03-17
layout: post
---

- TOC
{:toc}


# 1-1 螺旋矩阵 II（LeetCode）

[螺旋矩阵 II（LeetCode）](https://leetcode.cn/problems/spiral-matrix-ii/)：给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。



**【代码】**

```cpp
// 这道题在while循环中模拟时可以使用一个变量i+t/b/l/r的方式确认num二维的坐标值，稍微优化一下我的代码
#include <iostream>
using namespace std;
int N;
int num[100][100];
int main() {
	cin >> N;
	int k = 1, t = 0, l = 0, r = N - 1, b = N - 1;
	int i = 0, j = 0;
	while (k <= N * N) {
		for (i = t, j = l; j <= r; j++) num[i][j] = k++;
		t++;
		for (i = t, j = r; i <= b; i++) num[i][j] = k++;
		r--;
		for (i = b, j = r; j >= l; j--) num[i][j] = k++;
		b--;
		for (i = b, j = l; i >= t; i--) num[i][j] = k++;
		l++;
	}
	for (i = 0; i < N; i++) {
		for (j = 0; j < N; j++) cout << num[i][j] << " ";
		cout << "\n";
	}	
    return 0;
}
```







# 1-2 最长公共子序列，最长回文子序列；最长公共子串，最长回文子串

🔵 区分：子序列和子串**子序列** **Subsequences** **可以不连续，子串** **Substrings** **必须连续**

**Palindrome 回文**

这其实是4道经典的字符串相关题目。

最长公共子序列：此题为LeetCode原题，中文题目链接[点这里](https://leetcode.cn/problems/longest-common-subsequence/)，英文题目链接[点这里](https://leetcode.com/problems/longest-common-subsequence/)。

最长回文子序列：此题目是LeetCode原题，中文题目链接[点这里](https://leetcode.cn/problems/longest-palindromic-subsequence/)，英文题目链接[点这里](https://leetcode.com/problems/longest-palindromic-subsequence/)。





## 最长公共子序列

str1 “a12bc345def” 
str2 “mnp123qrs45z” 
最长公共子序列——“12345”

最重要的还是要考虑清楚：

### 分析

1. process函数以及参数的含义。调用`process(i,j)`表示**只考虑str1[0...i]和str2[0...j]的最长公共子序列。**
2. 递归（减小问题规模）的方法。如果str[i]和str[j]相同，考虑公共子序列一定算入（**和之后的子串问题形成对比**）。如果不同，则需在`process(i,j-1)`和`process(i-1,j)`中选取最大的。
3. 将递归函数转化为dp二维数组。因为`process(i,j)`需要调用信息只有`process(i-1, j-1)`，`process(i,j-1)`和`process(i-1,j)`，其实就是dp数组的左侧、上侧和左上角。
4. `dp[i][j]`只会参考当前位置上一层和左侧一个格子的dp值，因此也可以考虑把dp数组由二维压缩成一维的。

### 代码（java）

**【暴力递归】**

```java
static int longestCommonSubsequence1(String s1, String s2) {
    if (s1 == null || s2 == null || s1.length() == 0 || s2.length() == 0) return 0;
    char[] str1 = s1.toCharArray();
    char[] str2 = s2.toCharArray();
    return process(str1, str2, str1.length - 1, str2.length - 1);
}
//只考虑str1[0...i]和str2[0...j]的最长公共子序列
static int process(char[] str1, char[]str2, int i,int j) {
    if(i==0 && j == 0)//都只剩一个字符
        return str1[i] == str2[j] ? 1:0;
    else if (i==0) {//str1剩一个字符
        if (str1[i] == str2[j]) return 1;
        else return process(str1,str2,i,j-1);
    }
    else if (j == 0) {//str2剩下一个字符
        if (str1[i] == str2[j]) return 1;
        else return process(str1,str2,i-1,j);
    }
    else {
        //分三种情况考虑：
        // 1.str1的i可以包含在子序列中，str2的j不包含在子序列中
        int p1 = process(str1,str2,i,j-1);
        // 2.str1的i不包含在子序列中，str2的j可以包含在子序列中
        int p2 = process(str1,str2,i-1,j);
        // 3.str1的i和str2的j都包含在子序列中，这种情况的条件是str1[i]和str2[j]相等
        int p3 = str1[i] == str2[j] ? 1+process(str1,str2,i-1,j-1):0;
        // 注意，str1的i和str2的j都不包含在子序列中的情况被包含在了p1和p2两种情况中了
        return Math.max(p1,Math.max(p2,p3));
    }
}
```

**【暴力递归（优化）】**

PS：虽然看上去简单，但是理解起来也困难了。递归的作用是在接触新题时思考解决问题的思路。我觉得没有必要必须优化这么简洁。

```java
//只考虑str1[0...i]和str2[0...j]的最长公共子序列
static int process(char[] str1,char[]str2,int i,int j) {
    if (i < 0 || j < 0) return 0;//越界的情况，这样写避免了上面那样多种情况考虑
    if (str1[i] == str2[j]) {//如果满足这个条件了，那么一定是这种情况的子序列长度最长，则没有必要计算下面的两种情况。
        return 1+process(str1,str2,i-1,j-1);
    }
    int p1 = process(str1,str2,i,j-1);
    int p2 = process(str1,str2,i-1,j);
    return Math.max(p1,p2);
}
```

**【记忆化搜索】**

```java
// https://leetcode.com/problems/longest-common-subsequence/discuss/590781/From-Brute-Force-To-DP
private Integer[][] dp;//注意这里用的是Integer的数组，方便查看是否被填过
public int longestCommonSubsequence(String text1, String text2) {
    dp = new Integer[text1.length()][text2.length()];
    return longestCommonSubsequence(text1, text2, 0, 0);
}

private int longestCommonSubsequence(String text1, String text2, int i, int j) {
    if (i == text1.length() || j == text2.length())
        return 0;

    if (dp[i][j] != null)
        return dp[i][j];

    if (text1.charAt(i) == text2.charAt(j))
        return dp[i][j] = 1 + longestCommonSubsequence(text1, text2, i + 1, j + 1);
    else
        return dp[i][j] = Math.max(
        longestCommonSubsequence(text1, text2, i + 1, j),
        longestCommonSubsequence(text1, text2, i, j + 1)
    );
}
```

**【表依赖】**

```java
static int longestCommonSubsequence2(String s1, String s2) {
    if (s1 == null || s2 == null || s1.length() == 0 || s2.length() == 0)
        return 0;
    char[] str1 = s1.toCharArray();
    char[] str2 = s2.toCharArray();
    int[][] dp = new int[str1.length][str2.length];
    dp[0][0] = str1[0] == str2[0] ? 1 : 0;
    for (int j = 1; j < str2.length; j++) {
        dp[0][j] = (str1[0] == str2[j]) ? 1 : dp[0][j - 1];
    }
    for (int i = 1; i < str1.length; i++) {
        dp[i][0] = (str1[i] == str2[0]) ? 1 : dp[i - 1][0];
    }
    for (int i = 1; i < str1.length; i++) {
        for (int j = 1; j < str2.length; j++) {
            if (str1[i] == str2[j]) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
                continue;
            }
            dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
        }
    }
    return dp[str1.length-1][str2.length-1];
}
//fixme 第一遍写前面两个for的时候str2.length和str1.length弄反了
```







## 最长回文子序列

给定一个字符串str，返回这个宇符串的最长回文子序列长度

比如：str="a12b3c43def2gh1lkpm” 最长回文子序列是〝1234321” 或者“123c321〞，返回长度7



### 分析

* 思路一
    * str和str逆序串的最长公共子序列就是str的最长回文子序列长度
* 思路二：**<font color = red>范围尝试模型</font>**（大概意思就是对一维字符串或其他数组在左、右两个方向都进行尝试，向中间缩小问题规模）
    * `str[l]==str[r]`时，可以确定当前相同的字符一定会被算入最后的回文子序列，也就是最长的。（对比回文子串）

### 代码（java）

**【暴力递归】**

```java
static int longestPalindromeSubseq(String s) {
    if (s == null || s.length() == 0) return 0;
    char[] str = s.toCharArray();
    return process(str, 0, str.length - 1);
}

//只考虑从L到R到回文子序列长度
static int process(char[] str, int L, int R) {
    if (L > R) return 0;
    if (L == R)
        return str[L] == str[R] ? 1 : 0;
    if (str[L] == str[R]) {
        return 2 + process(str, L + 1, R - 1);
    }
    int p1 = process(str, L + 1, R);
    int p2 = process(str, L, R - 1);
    return Math.max(p1, p2);
}
```

**【表依赖】**

```java
static int longestPalindromeSubseq(String s) {
    if (s == null || s.length() == 0) return 0;
    char[] str = s.toCharArray();
    int [][]dp = new int[str.length][str.length];
    for (int i = 0; i < str.length; i++) {
        dp[i][i] = 1;
    }
    for (int i = str.length-1; i >= 0; i--) {
        for (int j = i+1; j < str.length; j++) {
            dp[i][j] = str[i] == str[j] ? 2+dp[i+1][j-1]:
            Math.max(dp[i+1][j],dp[i][j-1]);
        }
    }
    return dp[0][str.length-1];
}
```





# 1-3 表达式求值



## 分析

* 解决问题的思想都是要把中缀表达式进行处理，转化成后缀表达式。一种写法是边转化边计算，这种在读中缀表达式过程中就把能计算的算数算完了，没有输出完整的后缀表达式。当然也可以把中缀表达式彻底转化成后缀表达式，然后再计算。
* 表达式求值问题最关键的三点：两个栈（数字栈、符号栈），数字栈直接压入数字；符号栈中的符号优先级严格递增；输入到反括号计算直到消除对应括号；最后计算直到符号栈空。
* 如果没有「数字是非负整数」的限制，处理问题会很麻烦。比如`3*(-2)`这种就很不好处理负号。

## 代码（c++）

【简化版】

🔵 注意map的初始化方法。

🔵 用map可以使代码更简洁地判断括号的对应。实际上判断操作符的优先级也可以用map。

🔴 一定不要忘记查看栈前先判断一下栈是否为空（具体看代码，这个地方容易错）

```cpp
#include <iostream>
#include <stack>
#include <map>
using namespace std;
int val[6];
stack<int> num;
stack<char> op;
map<char,char> rev = { {')', '('},{']','['},{'}','{'} }; // map可以使用这种方式初始化
// priority
int pri(char c) {
	if (c == '+' || c == '-') return 1;
	if (c == '*' || c == '/' || c =='%') return 2;
	if (c == '^') return 3;
	return -1;
}

int isOp(char c) {
	return c == '+' || c == '-' ||c == '*' || c == '/' || c =='%'||c == '^';
}

int mi(int num1, int num2) {
    int tmp = num1;
    for (int i = 0; i < num2 - 1; i++) num1 *= tmp;
    return num1;
}

void cal() {
	int tmp2 = num.top(); num.pop();
	int tmp1 = num.top(); num.pop();
	char c = op.top(); op.pop();
	if (c == '+') num.push(tmp1 + tmp2);
	else if (c == '-') num.push(tmp1 - tmp2);
	else if (c == '*') num.push(tmp1 * tmp2);
	else if (c == '/') num.push(tmp1 / tmp2);
	else if (c == '%') num.push(tmp1 % tmp2);
	else if (c == '^') num.push(mi(tmp1, tmp2));
}

int main() {
	string ss;
	cin >> ss;
	for (int i = 0; i < 6; i++) cin >> val[i]; //输入a~f
	for (char c : ss) {
		if (c >= 'a' && c <= 'f') { // 表示num
			num.push(val[c-'a']);
		}
		else if (c == '(' || c == '[' || c == '{') {
			op.push(c);
		}
		else if (isOp(c)) {
			// TODO !这里一定不要忘记先判断一下栈是否为空！！
			while (!op.empty() && pri(op.top()) >= pri(c)) cal();
			op.push(c);
		}
		else {
			while (rev[c] != op.top()) cal();
			op.pop();
		}
	}
	while (!op.empty()) cal();
	cout << num.top();
}
```





# 1-4 高精度运算模拟

高精度的加减乘除运算模拟


