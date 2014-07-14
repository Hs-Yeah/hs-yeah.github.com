---
layout: post
title: ACM题目：Milking Cows
categories : [ACM]
tag: [ACM, ICPC, USACO]
---

#题目

####`Problem Description`
	Three farmers rise at 5 am each morning and head for the barn to milk three cows. The first farmer begins milking his cow at time 300 (measured in seconds after 5 am) and ends at time 1000. The second farmer begins at time 700 and ends at time 1200. The third farmer begins at time 1500 and ends at time 2100. The longest continuous time during which at least one farmer was milking a cow was 900 seconds (from 300 to 1200). The longest time no milking was done, between the beginning and the ending of all milking, was 300 seconds (1500 minus 1200).

	Your job is to write a program that will examine a list of beginning and ending times for N (1 <= N <= 5000) farmers milking N cows and compute (in seconds):

	The longest time interval at least one cow was milked.
	The longest time interval (after milking starts) during which no cows were being milked.

####`Input`
<pre>
	<table border="1">
		<tbody>
			<tr>
				<td>Line 1:</td>
				<td>The single integer</td>
			</tr>
			<tr>
				<td>Lines 2..N+1:</td>
				<td>Two non-negative integers less than 1000000, the starting and ending time in seconds after 0500</td>
			</tr>
		</tbody>
	</table>
</pre>

####`Output`
	A single line with two integers that represent the longest continuous time of milking and the longest idle time.

####`Sample Input`
	3
	300 1000
	700 1200
	1500 2100

####`Sample Output`
	900 300

#解题思路
	把每对数据看成区间的两个端点，判断前后两个区间的重叠情况（有交集或收尾连接），再进行操作（如果有交集或收尾连接则合并成一个区间）。
	The longest time interval at least one cow was milked就是处理完之后每个区间长度中最长的那一个。
	The longest time interval (after milking starts) during which no cows were being milked就是处理完之后每两个区间之间间距中最长的那一个。

#解题代码
{% highlight C++ %}
//
//  main.cpp
//  Milking Cows
//
//  Created by Yeah on 14-6-5.
//  Copyright (c) 2014年 Yeah. All rights reserved.
//


#include <fstream>
#include <vector>
#include <algorithm>

using namespace std;

bool cmp(pair<int, int> p1, pair<int, int> p2)
{
    return p1.first < p2.first;
}

int main() {
    ifstream fin ("milk2.in");
    ofstream fout ("milk2.out");
    
    unsigned int N(0);
    fin >> N;
    vector<pair<int, int> > v;
    while (N--)
    {
        unsigned int Time_Begin(0), Time_End(0);
        fin >> Time_Begin >> Time_End;
        v.push_back(make_pair(Time_Begin, Time_End));
    }
    
    if (v.size() > 1)
    {
        sort(v.begin(), v.end(), cmp);
        
        for (vector<pair<int, int> >::iterator it(v.begin()); it != v.end() - 1; ++it)
        {
            vector<pair<int, int> >::iterator it_next(it + 1);
            if(it->first < it_next->second && it->second > (it_next)->first)//有交集
            {
                if (it->first < (it_next)->first && it->second > it_next->second)
                {
                    v.erase(it_next);
                    it = v.begin();
                }
                else if (it->first > (it_next)->first && it->second < it_next->second)
                {
                    v.erase(it);
                    it = v.begin();
                }
                else if (it->first < (it_next)->first && it->second < it_next->second)
                {
                    it_next->first = it->first;
                    v.erase(it);
                    it = v.begin();
                }
                else if (it->first > (it_next)->first && it->second > it_next->second)
                {
                    it->first = it_next->first;
                    v.erase(it);
                    it = v.begin();
                }
            }
        }
        
        int Max_Last(0), Max_NoMilk(0);
        pair<int, int> range(make_pair(v.begin()->first, v.begin()->second));
        for (vector<pair<int, int> >::iterator it(v.begin()); it != v.end(); ++it)
        {
            range.first = it->first < range.first ? it->first : range.first;
            if (it->first > range.second)
            {
                Max_NoMilk = it->first - range.second > Max_NoMilk ? it->first - range.second : Max_NoMilk;
                range.first = it->first;
                range.second = it->second;
            }
            range.second = it->second > range.second ? it->second : range.second;
            Max_Last = range.second - range.first > Max_Last ? range.second - range.first : Max_Last;
        }
        fout << Max_Last << ' ' << Max_NoMilk << endl;
    }
    else
    {
        fout << v.begin()->second - v.begin()->first << ' ' << 0 << endl;
    }
    
    return 0;
}
{% endhighlight %}


######`题目链接`：[USACO Section 1.2 PROB Milking Cows]()