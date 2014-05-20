---
layout: post
title: ACM题目：分数
categories : [ACM]
tag: [ACM, ICPC, Sicily Online Judge]
---

#题目

####`Problem Description`
	In Sicily Online Judge System, there is a special user rank list. Every user gets a score for every problem he solved, and the fewer people have solved this problem, the higher score he can get. The following table shows how many scores you can get:
	Score   10    8     6     4     2     1
	Solved 1-10 11-30 31-50 51-75 76-100 >100
	Your job is to calculate the total score for a given user.

####`Input`
	The first line contains an integer np(1<=np<300) which is the number of problems in Online Judge. The second line contains np integers representing the number of users who have solved this problem from problem 1000 to problem 1000+np-1.
	The third line contains an integer t(t<=10), which is the number of test cases.
	Each test case begins with an integer n, which is the number of problems the user has solved. Then it is followed by n distinct integers which are the problem ids. Problem id is labeled from 1000.

####`Output`
	for each test case, print the total score he can get on a single line.

####`Sample Input`
	10
	100 10 11 3 45 7 34 200 70 1
	4
	2 1000 1001
	2 1001 1002
	0
	3 1000 1007 1008

####`Sample Output`
	12
	18
	0
	7

#解题思路
	这个直接按题目说的做就行了╮(￣▽￣")╭

#解题代码
{% highlight C++ %}
//
//  main.cpp
//  分数
//
//  Created by Yeah on 14-5-20.
//  Copyright (c) 2014年 Yeah. All rights reserved.
//

#include <iostream>
#include <vector>
using namespace std;

unsigned int Score(unsigned int Num_Solved)
{
    if (Num_Solved >= 1 && Num_Solved <= 10)
    {
        return 10;
    }
    else if (Num_Solved >= 11 && Num_Solved <= 30)
    {
        return 8;
    }
    else if (Num_Solved >= 31 && Num_Solved <= 50)
    {
        return 6;
    }
    else if (Num_Solved >= 51 && Num_Solved <= 75)
    {
        return 4;
    }
    else if (Num_Solved >= 76 && Num_Solved <= 100)
    {
        return 2;
    }
    else if (Num_Solved > 100)
    {
        return 1;
    }
    return 0;
}

int main()
{
    unsigned int np(0);
    cin >> np;
    vector<unsigned int> v(np, 0);
    unsigned int num_solved(0);
    for (vector<unsigned int>::iterator it(v.begin()); it != v.end(); it++)
    {
        cin >> num_solved;
        *it = Score(num_solved);
    }
    unsigned int t(0);
    cin >> t;
    while (t--)
    {
        unsigned int n(0);
        cin >> n;
        unsigned int Sum(0), problem_id(1000);
        while (n--)
        {
            cin >> problem_id;
            problem_id -= 1000;
            Sum += *(v.begin() + problem_id);
        }
        cout << Sum << endl;
    }
    return 0;
}
{% endhighlight %}


######`题目链接`：[]()暂无