---
title: 代码快速复习
date: 2024-3-21
layout: post 
---



```c
#include <stdio.h>
int main() {
	printf("Hello world!\n");
	return 0;
}
```
<br>

- TOC
{:toc}




# C语言（C++）复习

如果可以使用C++，主要注意下面C语言用法：

* C语言可以格式化输入`scanf("%d.%d.%d.%d", &a, &b, &c, &d);`
* C语言可以格式化输出`printf("%03d\n", 7);`
* 多次获取值（适用于从文件读入）



## 输入输出

```c
// 输入
scanf("%d", &a);
scanf("%d%d%d", &a, &b, &c);
scanf("%d.%d.%d.%d", &a, &b, &c, &d); // 例如输入 192.108.12.1,则abcd会输入为相应值
scanf("%s", str); 			// char str[50]; （没有地址符号）
gets(str);					// 输入一行字符串
getline(cin, str); 			// 输入一行字符串 string str;
```

**注：使用getline获取一行的数据时，如果之前有输入过字符，会在缓冲区中留下回车符号。此时必须使用`getchar()`把缓冲区中的回车去掉。**

```c++
#include <iostream>
using namespace std;

int main(int argc, char *argv[]) {
	string ss, sstest;
	cin >> ss;
	getchar(); // 要把缓冲区中的回车去掉，没有这行代码sstest为空
	getline(cin, sstest);
	cout << "sstest: " << sstest << endl;
	cout << "sstest's length: " << sstest.length() << endl;
}
```



<br>

```c
// 输出
printf("%d\n", 127);        // 127
printf("%03d\n", 7);		// 007
printf("%f\n", 314.15);     // 314.159000 （float类型）
printf("%.2f\n", 314.15);   // 314.15 指定精度
printf("%lf\n", 314.15);    // 314.159000 （double类型）

printf("%c\n", 'c');        // c
printf("%s\n", str);        // char str[50] （没有地址符号）

printf("%o\n", 177);        // 177  八进制
printf("%x\n", 177);        // b1   十六进制
printf("%e\n", 314.15);     // 3.141590e+02
```

<br>

**Online Judge中连续多组测试数据的输入：**

```c++
while (scanf("%d", &num) != EOF) {
    //...
}
while (gets(str)) { // 输入一行字符串
    //...
}
while (cin >> a >> b) {
    //...
}
while (getline(cin, str)) {
    //...
}
```



## 字符串处理

```c
char str[50];
scanf("%s", str);
printf("%s\n", str);
// 下面的函数在<string.h>头文件中
printf("%d\n", strlen(str));
char str2[50];
strcpy(str2, str1); // 把str1复制到str2中
```



## 文件IO

（IO感觉真的不太常用），这里只介绍通过`fgetc(FILE*)`一个一个字符读取文件，通过`fputc(char, FILE*)`一个一个字符写入文件。

```c
void fileRead() {
    FILE* fp = fopen("文件路径", "r"); //以读(r)的方式打开文件
    char ch;
    while((ch = fgetc(fp)) != EOF) { // 一个一个字符串读入
        // ...
    }
    fclose(fp);
}

char *fgets(char *str, int n, FILE *stream);
// str：指向一个字符数组的指针，用于存储读取到的字符串。
// n：指明最多读取多少个字符，包括最后的空字符。为了安全读取整行，这个值应该是str指向的数组的大小。
// stream：指定从哪个文件流读取数据，通常是一个FILE *类型的文件指针。

void fileWrite() {
    File* fp = fopen("文件路径", "w"); // 以写(w)的方式打开文件，文件不存在会自动创建，文件存在会覆盖掉之前的内容
    string ss = "Input content...";
    for (char c: ss) fputc(c, fp);
    fclose(fp);
}

```





## C++ STL与库函数

`stio()`和`to_string()`

```c++
// stack
stack<int> num;
num.push(9);
// top()返回栈顶元素，pop()删除栈顶元素不返回值
int tmp1 = num.top(); num.pop();


// map
map<char,char> rev = { {')', '('},{']','['},{'}','{'} }; // map可以使用这种方式初始化

// 用stack<pair<int, int>>保存图的邻接链表
#include <iostream>
#include <vector>
using namespace std;

int main() {
	vector<pair<int, int>> edge = { {1,2}, {3,4}, {5,6} };
	edge.push_back({12, 34});
	for (int i = 0; i < edge.size(); i++) 
		cout << edge[i].first << " " << edge[i].second << endl;
}
```







## 牛客网练习题链接

牛客网华中科技大学历年计算机考研复试上机题在线练习[链接点这里](https://www.nowcoder.com/ta/hust-kaoyan)。

【PS】华中科技大学复试的机试环节，是将每道题目写好的代码放入指定的文件夹中保存在PC上，并非像LeetCode/牛客那样的线上测试评分。所以包括牛客网的题目，有些题目的测试数据也是不严谨的。







# 练习题目

## 判断是否为闰年

```cpp
(year % 400 == 0) || (year % 100!=0 && year % 4==0)
```

## 判断是否为质数 IsPrime

```cpp
```

## 输出int整数的32位二进制

```cpp
```





## 表达式求值

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





## 将数值去掉n位后返回最小的那个数

```cpp
#include <iostream>
using namespace std;

// 取出num第index位数字后返回
string remove(int num, int index) {
	string ss = to_string(num);
	string res;
	for (int i = 0; i < index; i++) res.push_back(ss[i]);
	for (int i = index+1; i < ss.length(); i++) res.push_back(ss[i]);
	return res;
}

// a：输入的数，n：要删除的数字的数量
int process(int a, int n) {
	if (n == 0) return a;
	string ss = to_string(a);
	int res = 0x3f3f3f3f;
	for (int i = 0; i < ss.length(); i++) {
		string cur = remove(a, i);
		int tmp = process(stoi(cur), n-1);
		res = min(res, tmp);
	}
	return res;
}

int main() {
	cout << process(692434354, 5);
}
```







## 🔴 图的算法

单源最短路（Dijkstra）

```c++
int vis[n];
int dist[n];
int path[n];
vector<pair<int, int>> edge;

void dijkstra(int v) {
    // 初始化
    for (int i = 0; i < n; i++) {
        vis[i] = 0;
        dist[i] = MAX;
        path[i] = -1;
    }
    dist[v] = 0;
    
    for (int j = 0; j < n; j++) {
        int u = 0;
        int minDist = MAX;
        for (int i = 0; i < n; i++) {
            if (!vis[i] && minDist > dist[i]) {
                u = i;
                minDist =  dist[i];
            }
        }
        vis[u] = 1;
        
        for (int i = 0; i < edge[u].size(); i++) {
            int v = edge[u][i].first, cost = edge[u][i].second;
            if (dist[v] > dist[u] + cost) {
                dist[v] = dist[u] + cost;
                path[v] = u;
            }
        }
    }
    
}
```



最小生成树

```cpp
void Prim() {
    int i, j, k, u;
    int ldist; // 存储最小代价
    struct EdgeNode* p;

    // 初始化
    for (i = 1; i <= n; i++) {
        lowcost[i] = INF;
        mark[i] = 0; // 初始时，MST中没有顶点
        vex[i] = -1;
    }

    // 从顶点1开始构造MST
    mark[1] = 1;
    lowcost[1] = 0;
    for (p = Head[1]; p != NULL; p = p->link) {
        k = p->VerAdj;
        lowcost[k] = p->cost;
        vex[k] = 1;
    }

    // 构造MST的主循环
    for (j = 1; j < n; j++) {
        ldist = INF; // 重置最小代价
        u = -1; // 即将被访问的顶点
        // 确定即将被访问的顶点u
        for (i = 1; i <= n; i++) {
            if (!mark[i] && lowcost[i] < ldist) {
                ldist = lowcost[i];
                u = i;
            }
        }

        // 将顶点u加入MST
        if (u != -1) { // 找到了有效的顶点
            mark[u] = 1;

            // 更新与u相邻顶点到MST的最小代价
            for (p = Head[u]; p != NULL; p = p->link) {
                k = p->VerAdj;
                if (!mark[k] && p->cost < lowcost[k]) {
                    lowcost[k] = p->cost;
                    vex[k] = u;
                }
            }
        }
    }
}
```



并查集



拓扑排序





## 🔴 动态规划（回文子串）的一些内容的复习







## 🔴 分数转换成小数

```java
string fractionToDecimal(int numerator, int denominator) {
    if (numerator == 0) {
        // 分子为0，直接返回"0"
        return "0";
    }

    string result;
    // 如果结果为负数（异号），在结果前加上负号
    if (numerator < 0 ^ denominator < 0) result += '-';

    // 使用long long类型避免溢出，并取绝对值
    long long a = llabs(numerator), b = llabs(denominator);

    // 计算整数部分，并将余数乘10准备计算小数部分
    result += to_string(a / b);
    long long remainder = a % b;
    if (remainder == 0) {
        // 如果能整除，直接返回整数部分
        return result;
    }

    // 处理小数部分
    result += '.';
    unordered_map<long long, int> map; // 用于记录余数出现的位置

    // 当余数不为0且该余数未出现过时，继续进行循环
    while (remainder != 0 && map.find(remainder) == map.end()) {
        map[remainder] = result.size(); // 记录当前余数的位置
        remainder *= 10; // 余数乘10准备下一轮计算
        result += to_string(remainder / b); // 计算新的一位小数
        remainder %= b; // 更新余数
    }

    if (remainder != 0) { // 如果余数非0，说明发现了循环小数
        // 在循环开始的位置插入左括号，并在字符串末尾加上右括号
        result.insert(map[remainder], "(");
        result += ')';
    }

    return result;
}
```















