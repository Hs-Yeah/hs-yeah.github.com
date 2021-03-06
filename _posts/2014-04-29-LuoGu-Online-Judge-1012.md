---
layout: post
title: ACM 题目：连接若干整数以组成最大整数
categories : [ACM]
tag: [ACM, ICPC, 洛谷Online Judge]
---

# 题目

#### `Problem Description`
	设有n个正整数（n≤20），将它们联接成一排，组成一个最大的多位整数。  
	例如：n=3时，3个整数13，312，343联接成的最大整数为：34331213  
	又如：n=4时，4个整数7，13，4，246联接成的最大整数为：7424613

#### `Input`
	第一行，一个正整数n。
	第二行，n个正整数。

#### `Output`
	一个正整数，表示最大的整数

#### `Sample Input`
	3
	13 312 343

#### `Sample Output`
	34331213

# 解题思路
	一看到题目，就想到使用排序。
	一开始只想到，在将数字转换成字符串之后（或者直接输入成字符串），针对两个字符串进行比较，可是交上去只过了一组数据，后面再怎么微调，都还有一组数据过不了_(:зゝ∠)_  
	后来经过点拨，才发现需要将比较大小的方法改一下，改成比较两个字符串的两种拼接组合，才把题目给AC了╮(￣▽￣")╭

# 解题代码

<!--lint disable-->

{% highlight C++ %}
//
//  main.cpp
//  拼数
//
//  Created by Yeah on 14-4-29.
//  Copyright (c) 2014年 Yeah. All rights reserved.
//

#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

bool cmp(string a, string b)
{
    return a + b > b + a;
}

int main()
{
    unsigned int n(0);
    while (cin >> n)
    {
        string num;
        vector<string> v;
        while (n--)
        {
            cin >> num;
            v.push_back(num);
        }
        sort(v.begin(), v.end(), cmp);
        for (vector<string>::iterator it(v.begin()); it != v.end(); it++)
        {
            cout << *it;
        }
        cout << endl;
    }
    return 0;
}


{% endhighlight %}

<!--lint enable-->

###### `题目链接`：[洛谷OJ 1012](http://oj.luogu.org:8888/problemshow.php?pid=1012)
