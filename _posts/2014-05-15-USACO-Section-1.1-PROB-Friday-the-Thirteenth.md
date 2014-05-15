---
layout: post
title: ACM题目：Friday the Thirteenth
categories : [ACM]
tag: [ACM, ICPC, USACO]
---

#题目

####`Problem Description`
	Is Friday the 13th really an unusual event?

	That is, does the 13th of the month land on a Friday less often than on any other day of the week? To answer this question, write a program that will compute the frequency that the 13th of each month lands on Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, and Saturday over a given period of N years. The time period to test will be from January 1, 1900 to December 31, 1900+N-1 for a given number of years, N. N is positive and will not exceed 400.
	
	Note that the start year is NINETEEN HUNDRED, not 1990.
	
	There are few facts you need to know before you can solve this problem:
	
	January 1, 1900 was on a Monday.
	Thirty days has September, April, June, and November, all the rest have 31 except for February which has 28 except in leap years when it has 29.
	Every year evenly divisible by 4 is a leap year (1992 = 4*498 so 1992 will be a leap year, but the year 1990 is not a leap year)
	The rule above does not hold for century years. Century years divisible by 400 are leap years, all other are not. Thus, the century years 1700, 1800, 1900 and 2100 are not leap years, but 2000 is a leap year.
	Do not use any built-in date functions in your computer language.
	
	Don't just precompute the answers, either, please.

####`Input`
	One line with the integer N.

####`Output`
	Seven space separated integers on one line. These integers represent the number of times the 13th falls on Saturday, Sunday, Monday, Tuesday, ..., Friday.

####`Sample Input`
	20

####`Sample Output`
	36 33 34 33 35 35 34

#解题思路
	每日一水题，终有一日成大犇~（大误

#解题代码
{% highlight C++ %}
//
//  main.cpp
//  Friday the Thirteenth
//
//  Created by Yeah on 14-5-15.
//  Copyright (c) 2014年 Yeah. All rights reserved.
//


#include <fstream>
#include <map>

using namespace std;

bool isLeapYear(unsigned int Year)
{
    if (Year % 400 == 0)
    {
        return true;
    }
    else
    {
        if (Year % 4 == 0 && Year % 100 != 0)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
}

int main() {
    ofstream fout ("friday.out");
    ifstream fin ("friday.in");
    unsigned int N;
    fin >> N;
    
    map<string, unsigned int> Week_Days;
    Week_Days["Sunday"]    = 0;
    Week_Days["Monday"]    = 0;
    Week_Days["Tuesday"]   = 0;
    Week_Days["Wednesday"] = 0;
    Week_Days["Thursday"]  = 0;
    Week_Days["Friday"]    = 0;
    Week_Days["Saturday"]  = 0;
    
    map<unsigned int, unsigned int> Common_Year;
    Common_Year[1]  = 31;
    Common_Year[2]  = 28;
    Common_Year[3]  = 31;
    Common_Year[4]  = 30;
    Common_Year[5]  = 31;
    Common_Year[6]  = 30;
    Common_Year[7]  = 31;
    Common_Year[8]  = 31;
    Common_Year[9]  = 30;
    Common_Year[10] = 31;
    Common_Year[11] = 30;
    Common_Year[12] = 31;
    
    map<unsigned int, unsigned int> Leap_Year(Common_Year);
    Leap_Year[2] = 29;
    
    unsigned int Days_cnt(13);
    for (unsigned int Year(1900); Year != 1900 + N; ++Year)
    {
        for (unsigned int month(1); month <= 12; ++month)
        {
            switch (Days_cnt % 7)
            {
                case 1:
                    Week_Days["Monday"]   ++;
                    break;
                    
                case 2:
                    Week_Days["Tuesday"]  ++;
                    break;
                    
                case 3:
                    Week_Days["Wednesday"]++;
                    break;
                    
                case 4:
                    Week_Days["Thursday"] ++;
                    break;
                    
                case 5:
                    Week_Days["Friday"]   ++;
                    break;
                    
                case 6:
                    Week_Days["Saturday"] ++;
                    break;
                    
                default:
                    Week_Days["Sunday"]   ++;
                    break;
            }
            if (isLeapYear(Year))
            {
                Days_cnt += Leap_Year[month];
            }
            else
            {
                Days_cnt += Common_Year[month];
            }
        }
    }
    fout << Week_Days["Saturday"]  << ' '
         << Week_Days["Sunday"]    << ' '
         << Week_Days["Monday"]    << ' '
         << Week_Days["Tuesday"]   << ' '
         << Week_Days["Wednesday"] << ' '
         << Week_Days["Thursday"]  << ' '
         << Week_Days["Friday"]    << endl;
    return 0;
}
{% endhighlight %}


######`题目链接`：[USACO Section 1.1 PROB Friday the Thirteenth](http://cerberus.delos.com:790/usacoprob2?a=xW1PDjhmReW&S=friday)