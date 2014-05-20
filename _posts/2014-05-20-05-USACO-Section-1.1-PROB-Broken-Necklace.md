---
layout: post
title: ACM题目：Broken Necklace
categories : [ACM]
tag: [ACM, ICPC, USACO]
---

#题目

####`Problem Description`
	You have a necklace of N red, white, or blue beads (3<=N<=350) some of which are red, others blue, and others white, arranged at random. Here are two examples for n=29:

                1 2                               1 2
            r b b r                           b r r b
          r         b                       b         b
         r           r                     b           r
        r             r                   w             r
       b               r                 w               w
      b                 b               r                 r
      b                 b               b                 b
      b                 b               r                 b
       r               r                 b               r
        b             r                   r             r
         b           r                     r           r
           r       r                         r       b
             r b r                             r r w
            Figure A                         Figure B
	                       r red bead
	                       b blue bead
	                       w white bead
                        
	The beads considered first and second in the text that follows have been marked in the picture.

	The configuration in Figure A may be represented as a string of b's and r's, where b represents a blue bead and r represents a red one, as follows: brbrrrbbbrrrrrbrrbbrbbbbrrrrb .

	Suppose you are to break the necklace at some point, lay it out straight, and then collect beads of the same color from one end until you reach a bead of a different color, and do the same for the other end (which might not be of the same color as the beads collected before this).

	Determine the point where the necklace should be broken so that the most number of beads can be collected.

####`Example`
	For example, for the necklace in Figure A, 8 beads can be collected, with the breaking point either between bead 9 and bead 10 or else between bead 24 and bead 25.

	In some necklaces, white beads had been included as shown in Figure B above. When collecting beads, a white bead that is encountered may be treated as either red or blue and then painted with the desired color. The string that represents this configuration can include any of the three symbols r, b and w.

	Write a program to determine the largest number of beads that can be collected from a supplied necklace.
	
####`Input`
	Line 1:	 N, the number of beads
	Line 2:	 a string of N characters, each of which is r, b, or w


####`Output`
	A single line containing the maximum of number of beads that can be collected from the supplied necklace.

####`Sample Input`
	29
	wwwbbrwrbrbrrbrbrwrwwrbwrwrrb

####`Sample Output`
	11
	
####`Output Explanation`
	Consider two copies of the beads (kind of like being able to runaround the ends). The string of 11 is marked.
                Two necklace copies joined here
                                 v
	wwwbbrwrbrbrrbrbrwrwwrbwrwrrb|wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
                           ******|*****
                           rrrrrb bbbbb  <-- assignments
                           rrrrr#bbbbbb  
                           5 x r  6 x b  <-- 11 total

#解题思路
	笨方法，按照题目的提示，将Necklace复制一遍之后，从头到尾在每个空隙之间断开，统计断开之后左边跟右边Necklace的Maximum of Number of Beads，左边的就从字符串后面开始扫过去，右边的就从前面开始扫过去。
	需要注意的是，如果Necklace全是'w'，则可能统计方法不适用（我写的代码就不适用……），还有就是需要特别处理断点两侧是'w'的情况。

#解题代码
{% highlight C++ %}
//
//  main.cpp
//  Broken Necklace
//
//  Created by Yeah on 14-5-18.
//  Copyright (c) 2014年 Yeah. All rights reserved.
//

#include <fstream>
#include <string>
using namespace std;

unsigned int Max_Left(string s)
{
    unsigned int Max_Length(0);
    
    if (s.empty())
    {
        return 0;
    }
    else
    {
        char color(*s.rbegin());
        for (string::reverse_iterator rit(s.rbegin()); rit != s.rend(); ++rit)
        {
            if (color == 'w')//处理断点左侧是'w'的情况(即使是多余一个也可以处理)
            {
                if (rit + 1 != s.rend())
                {
                    color = *(rit + 1);
                    Max_Length++;
                }
            }
            else if (*rit == color || *rit == 'w')
            {
                Max_Length++;
            }
            else
                break;
        }
    }
    return Max_Length;
}

unsigned int Max_Right(string s)
{
    unsigned int Max_Length(0);
    if (s.empty())
    {
        return 0;
    }
    else
    {
        char color(*s.begin());
        for (string::iterator it(s.begin()); it != s.end(); ++it)
        {
            if (color == 'w')//处理断点右侧是'w'的情况(即使是多余一个也可以处理)
            {
                if (it + 1 != s.end())
                {
                    color = *(it + 1);
                    Max_Length++;
                }
            }
            else if (*it == color || *it == 'w')
            {
                Max_Length++;
            }
            else
                break;
        }
    }
    return Max_Length;
}

int main() {
    ifstream fin ("beads.in");
    ofstream fout ("beads.out");
    
    unsigned int N(0);
    fin >> N;
    string Necklace;
    fin >> Necklace;
    Necklace.append(Necklace);
    
    unsigned int Whith_Count(0);//统计'w'的个数，如果全是'w'，则不需要统计（况且统计方法也不适用于这种情况）
    for (string::iterator it(Necklace.begin()); it != Necklace.end() - N; ++it)
    {
        if (*it == 'w')
        {
            Whith_Count++;
        }
    }
    if (Whith_Count == N)
    {
        fout << N << endl;
    }
    else
    {
        unsigned int Max_Length(0);
        for (string::size_type pos(0); pos != N; ++pos)
        {
            unsigned int Max_Length_Left (Max_Left (Necklace.substr(0, pos)));
            unsigned int Max_Length_Right(Max_Right(Necklace.substr(pos)));
            if (Max_Length < (Max_Length_Left + Max_Length_Right))
            {
                Max_Length = Max_Length_Left + Max_Length_Right;
            }
        }
        Max_Length = Max_Length > N ? N : Max_Length;
        fout << Max_Length << endl;
    }
    
    return 0;
}
{% endhighlight %}

######`题目链接`：[USACO Section 1.1 PROB Broken Necklace](http://cerberus.delos.com:790/usacoprob2?a=AoVKOjQ1mC6&S=beads)