---
layout: post
title: ACM题目：Greedy Gift Givers
categories : [ACM]
tag: [ACM, ICPC, USACO]
---

#题目

####`Problem Description`
	A group of NP (2 ≤ NP ≤ 10) uniquely named friends has decided to exchange gifts of money. Each of these friends might or might not give some money to any or all of the other friends. Likewise, each friend might or might not receive money from any or all of the other friends. Your goal in this problem is to deduce how much more money each person gives than they receive.

	The rules for gift-giving are potentially different than you might expect. Each person sets aside a certain amount of money to give and divides this money evenly among all those to whom he or she is giving a gift. No fractional money is available, so dividing 3 among 2 friends would be 1 each for the friends with 1 left over -- that 1 left over stays in the giver's "account".

	In any group of friends, some people are more giving than others (or at least may have more acquaintances) and some people have more money than others.

	Given a group of friends, no one of whom has a name longer than 14 characters, the money each person in the group spends on gifts, and a (sub)list of friends to whom each person gives gifts, determine how much more (or less) each person in the group gives than they receive.

####`Input`
<pre>
	<table border="1">
	    <tbody>
	        <tr>
	            <td align="middle" style="width:131px">Line 1:</td>
	            <td>The single integer, NP</td>
	        </tr>
	        <tr>
	            <td align="middle">Lines 2..NP+1: </td>
	            <td>Each line contains the name of a group member</td>
	        </tr>
	        <tr>
	        	<td align="middle">Lines NP+2..end: </td>
	        	<td>The first line in the group tells the person's name who will be giving gifts.
The second line in the group contains two numbers: The initial amount of money (in the range 0..2000) to be divided up into gifts by the giver and then the number of people to whom the giver will give gifts, NGi (0 ≤ NGi ≤ NP-1).
If NGi is nonzero, each of the next NGi lines lists the the name of a recipient of a gift.</td>
	        </tr>
	    </tbody>
	</table>
</pre>

####`Output`
	The output is NP lines, each with the name of a person followed by a single blank followed by the net gain or loss (final_money_value - initial_money_value) for that person. The names should be printed in the same order they appear on line 2 of the input.

	All gifts are integers. Each person gives the same integer amount of money to each friend to whom any money is given, and gives as much as possible that meets this constraint. Any money not given is kept by the giver.

####`Sample Input`
	5
	dave
	laura
	owen
	vick
	amr
	dave
	200 3
	laura
	owen
	vick
	owen
	500 1
	dave
	amr
	150 2
	vick
	owen
	laura
	0 2
	amr
	vick
	vick
	0 0

####`Sample Output`
	dave 302
	laura 66
	owen -359
	vick 141
	amr -150

#解题思路
	思路嘛，跟题目要求的一样，就是获得一个名字，然后对号入座，分别统计
	一开始代码写着写着卡住了，因为是想着将一大堆东西放在一起，后来把索引部分跟储存数据部分分开了之后才豁然开朗起来……

#解题代码
{% highlight C++ %}
//
//  main.cpp
//  Greedy Gift Givers
//
//  Created by Yeah on 14-5-4.
//  Copyright (c) 2014年 Yeah. All rights reserved.
//

#include <fstream>
#include <string>
#include <map>
#include <vector>

using namespace std;

int main() {
    ofstream fout ("gift1.out");
    ifstream fin ("gift1.in");
    unsigned int NP(0);
    while (fin >> NP)
    {
        map<string, vector<pair<string, int> >::size_type> m;//First成员为名字name，second成员为输入顺序pos，此map用作索引，数据储存在v中
        vector<pair<string, int> > v;                        //First成员为name，second成员为最后有的金钱数final_money_value
        for (unsigned int pos(0); pos != NP; ++pos)
        {
            string name;
            fin >> name;
            m.insert(make_pair(name, pos));
            v.push_back(make_pair(name, 0));
        }
        map<string, int> init_money;                         //储存每个人一开始有的金钱数initial_amount_of_money
        for (unsigned int i(0); i != NP; ++i)
        {
            string name;
            fin >> name;
            unsigned int initial_amount_of_money(0), NGi(0);
            fin >> initial_amount_of_money >> NGi;
            init_money.insert(make_pair(name, initial_amount_of_money));
            (v.begin() + m[name])->second += initial_amount_of_money;
            if (NGi == 0)
            {
                continue;
            }
            else
            {
                (v.begin() + m[name])->second -= initial_amount_of_money / NGi * NGi;
                for (unsigned int j(0); j != NGi; ++j)
                {
                    string receiver_name;
                    fin >> receiver_name;
                    (v.begin() + m[receiver_name])->second += initial_amount_of_money / NGi;
                }
            }
        }
        for (vector<pair<string, int> >::iterator it(v.begin()); it != v.end(); ++it)
        {
            fout << it->first << ' ' << it->second - init_money[it->first] << endl;
        }
    }
    return 0;
}
{% endhighlight %}


######`题目链接`：[USACO Section 1.1 PROB Greedy Gift Givers](http://cerberus.delos.com:790/usacoprob2?a=6V5QW9LPmtD&S=gift1)