title: 开始搞ACM
id: 143
categories:
  - 未分类
date: 2014-12-11 21:43:56
tags:
---

上个月开始搞ACM(ICPC)，虽然只是做一些水题。一道题苦想半天调试N遍最后AC，这感觉 特别爽哟。

随手拿最近的一些水题放上来吧，当做纪念，可能几年后我成为了大神，人们来嘲笑我说，这么烂的代码竟然是他曾经写的。

* * *

#### Codeforces Round #237 (Div. 2)  - Valera and Fruits

<div id="vj_description" class="hiddable">

Description

<div class="textBG">

Valera loves his garden, where <span class="tex-span">_n_</span> fruit trees grow.

This year he will enjoy a great harvest! On the <span class="tex-span">_i_</span>-th tree <span class="tex-span">_b_<sub class="lower-index">_i_</sub></span> fruit grow, they will ripen on a day number <span class="tex-span">_a_<sub class="lower-index">_i_</sub></span>. Unfortunately, the fruit on the tree get withered, so they can only be collected on day <span class="tex-span">_a_<sub class="lower-index">_i_</sub></span> and day <span class="tex-span">_a_<sub class="lower-index">_i_</sub> + 1</span> (all fruits that are not collected in these two days, become unfit to eat).

Valera is not very fast, but there are some positive points. Valera is ready to work every day. In one day, Valera can collect no more than<span class="tex-span">_v_</span> fruits. The fruits may be either from the same tree, or from different ones. What is the maximum amount of fruit Valera can collect for all time, if he operates optimally well?

</div>
</div>
<div id="vj_input" class="hiddable">

Input

<div class="textBG">

The first line contains two space-separated integers <span class="tex-span">_n_</span> and <span class="tex-span">_v_</span><span class="tex-span">(1 ≤ _n_, _v_ ≤ 3000)</span> — the number of fruit trees in the garden and the number of fruits that Valera can collect in a day.

Next <span class="tex-span">_n_</span> lines contain the description of trees in the garden. The <span class="tex-span">_i_</span>-th line contains two space-separated integers <span class="tex-span">_a_<sub class="lower-index">_i_</sub></span> and <span class="tex-span">_b_<sub class="lower-index">_i_</sub></span><span class="tex-span">(1 ≤ _a_<sub class="lower-index">_i_</sub>, _b_<sub class="lower-index">_i_</sub> ≤ 3000)</span> — the day the fruits ripen on the <span class="tex-span">_i_</span>-th tree and the number of fruits on the <span class="tex-span">_i_</span>-th tree.

</div>
</div>
<div id="vj_output" class="hiddable">

Output

<div class="textBG">

Print a single integer — the maximum number of fruit that Valera can collect.

</div>
</div>
<div id="vj_sampleInput" class="hiddable">

Sample Input

<div class="textBG">
<div class="input">
<div class="title">Input</div>
<pre>2 3
1 5
2 3</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>8</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>5 10
3 20
2 20
1 20
4 20
5 20</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>60</pre>
</div>
</div>
</div>
<div id="vj_hint" class="hiddable">

Hint

<div class="textBG">

In the first sample, in order to obtain the optimal answer, you should act as follows.

*   On the first day collect <span class="tex-span">3</span> fruits from the <span class="tex-span">1</span>-st tree.
*   On the second day collect <span class="tex-span">1</span> fruit from the <span class="tex-span">2</span>-nd tree and <span class="tex-span">2</span> fruits from the <span class="tex-span">1</span>-st tree.
*   On the third day collect the remaining fruits from the <span class="tex-span">2</span>-nd tree.
In the second sample, you can only collect <span class="tex-span">60</span> fruits, the remaining fruit will simply wither.

</div>
</div>
```
#include<stdio.h>
#include<string.h>
#include <algorithm>
using namespace std;
int main()
{
	long int n,v,vv,a[3008],b[3008] ,i,ii,t,o,y,iy;
	while (scanf("%d%d", &n, &v) != EOF)
	{
		o = 0; vv = v; y = 0;
		for (i = 0; i < n; i++)
		{
			cin >> a[i]>>b[i];
		}
		for (i = 0; i < n; i++)
			for (ii = i + 1; ii < n; ii++)
			{
				if (a[i]>a[ii])
				{ 
					t = a[i]; a[i] = a[ii]; a[ii] = t; 
					t = b[i]; b[i] = b[ii]; b[ii] = t;
				}

			}

		//
		//
		//

		for (i = 0; i < n; i++)
		{
			if (i != 0)
			{
				if (a[i] != a[i - 1]) { v = vv; }

			}
			if (v != 0 && i != 0)
			{
				for (iy = y; iy < i; iy++)
				{
					if (b[iy] != 0)
					{
						if (v <= b[iy])
						{
							o += v; b[iy] -= v; v = 0;
						}
						else { o += b[iy]; v -= b[iy]; b[iy] = 0; }
					}
				}
			}
			if (a[i] - a[i - 1] > 1) v = vv;
			if (v != 0)
			{
				if (v <= b[i])
				{
					o += v; b[i] -= v; v = 0;
				}
				else { o += b[i]; v -= b[i]; b[i] = 0; }
			}
			if (a[i - 1] != a[i]&&i!=0) y = i;

		}

		//
		//
		//

		v = vv; 

		for (iy = y; iy < i; iy++)
		{
			if (b[iy] != 0)
			{
				if (v <= b[iy])
				{
					o += v; b[iy] -= v; v = 0;
				}
				else { o += b[iy]; v -= b[iy]; b[iy] = 0; }
			}
		}

		cout << o << endl;

	}
	return(0);
}
```
&nbsp;

* * *

#### Codefoces 436B-Om Nom and Spiders

<div id="vj_description" class="hiddable">

Description

<div class="textBG">

Om Nom really likes candies and doesn't like spiders as they frequently steal candies. One day Om Nom fancied a walk in a park. Unfortunately, the park has some spiders and Om Nom doesn't want to see them at all.

<center>![](http://espresso.codeforces.com/5be91a371adfaffc95d78b5f3caf8c1149be2c29.png)</center>The park can be represented as a rectangular <span class="tex-span">_n_ × _m_</span> field. The park has <span class="tex-span">_k_</span> spiders, each spider at time 0 is at some cell of the field. The spiders move all the time, and each spider always moves in one of the four directions (left, right, down, up). In a unit of time, a spider crawls from his cell to the side-adjacent cell in the corresponding direction. If there is no cell in the given direction, then the spider leaves the park. The spiders do not interfere with each other as they move. Specifically, one cell can have multiple spiders at the same time.

Om Nom isn't yet sure where to start his walk from but he definitely wants:

*   to start walking at time 0 at an upper row cell of the field (it is guaranteed that the cells in this row do not contain any spiders);
*   to walk by moving down the field towards the lowest row (the walk ends when Om Nom leaves the boundaries of the park).
We know that Om Nom moves by jumping. One jump takes one time unit and transports the little monster from his cell to either a side-adjacent cell on the lower row or outside the park boundaries.

Each time Om Nom lands in a cell he sees all the spiders that have come to that cell at this moment of time. Om Nom wants to choose the optimal cell to start the walk from. That's why he wonders: for each possible starting cell, how many spiders will he see during the walk if he starts from this cell? Help him and calculate the required value for each possible starting cell.

</div>
</div>
<div id="vj_input" class="hiddable">

Input

<div class="textBG">

The first line contains three integers <span class="tex-span">_n_, _m_, _k_</span><span class="tex-span">(2 ≤ _n_, _m_ ≤ 2000; 0 ≤ _k_ ≤ _m_(_n_ - 1))</span>.

Each of the next <span class="tex-span">_n_</span> lines contains <span class="tex-span">_m_</span> characters — the description of the park. The characters in the <span class="tex-span">_i_</span>-th line describe the <span class="tex-span">_i_</span>-th row of the park field. If the character in the line equals "<span class="tex-font-style-tt">.</span>", that means that the corresponding cell of the field is empty; otherwise, the character in the line will equal one of the four characters: "<span class="tex-font-style-tt">L</span>" (meaning that this cell has a spider at time 0, moving left), "<span class="tex-font-style-tt">R</span>" (a spider moving right), "<span class="tex-font-style-tt">U</span>" (a spider moving up), "<span class="tex-font-style-tt">D</span>" (a spider moving down).

It is guaranteed that the first row doesn't contain any spiders. It is guaranteed that the description of the field contains no extra characters. It is guaranteed that at time 0 the field contains exactly <span class="tex-span">_k_</span> spiders.

</div>
</div>
<div id="vj_output" class="hiddable">

Output

<div class="textBG">

Print <span class="tex-span">_m_</span> integers: the <span class="tex-span">_j_</span>-th integer must show the number of spiders Om Nom will see if he starts his walk from the <span class="tex-span">_j_</span>-th cell of the first row. The cells in any row of the field are numbered from left to right.

</div>
</div>
<div id="vj_sampleInput" class="hiddable">

Sample Input

<div class="textBG">
<div class="input">
<div class="title">Input</div>
<pre>3 3 4
...
R.L
R.U</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>0 2 2</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>2 2 2
..
RL</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>1 1</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>2 2 2
..
LR</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>0 0</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>3 4 8
....
RRLL
UUUU</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>1 3 3 1</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>2 2 2
..
UU</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>0 0</pre>
</div>
</div>
</div>
<div id="vj_hint" class="hiddable">

Hint

<div class="textBG">

Consider the first sample. The notes below show how the spider arrangement changes on the field over time:
<pre>...        ...        ..U       ...
R.L   ->   .*U   ->   L.R   ->  ...
R.U        .R.        ..R       ...

</pre>
Character "<span class="tex-font-style-tt">*</span>" represents a cell that contains two spiders at the same time.

*   If Om Nom starts from the first cell of the first row, he won't see any spiders.
*   If he starts from the second cell, he will see two spiders at time 1.
*   If he starts from the third cell, he will see two spiders: one at time 1, the other one at time 2.
</div>
</div>
```
#include<iostream>
#include<stdio.h>
using namespace std;
int main()
{
	int n, m, k,i,j,fuck;
	char in[2002][2002];
	while (scanf("%d%d%d", &n,&m,&k) != EOF)
	{	
		fuck = 0;
		for (i = 0; i < n; i++)
			for (j = 0; j < m; j++)
			{
				cin >> in[i][j];
			}

		for (j = 0; j < m; j++)
		{
			for (i = 1; i < n; i++)
			{
				if(i+i<n)if (in[i + i][j] == 'U')fuck++;
				if (j - i>=0&&j-i<m)if (in[i][j - i] == 'R')fuck++;
				if (j + i<m)if (in[i][j + i] == 'L')fuck++;
			}
			cout << fuck << ' '; fuck = 0;
		}
        cout<<endl;

	}
	return(0);
}
```
&nbsp;

* * *

#### Codeforces 435B Pasha Maximizes

<div id="vj_description" class="hiddable">

Description

<div class="textBG">

Pasha has a positive integer <span class="tex-span">_a_</span> without leading zeroes. Today he decided that the number is too small and he should make it larger. Unfortunately, the only operation Pasha can do is to swap two adjacent decimal digits of the integer.

Help Pasha count the maximum number he can get if he has the time to make at most <span class="tex-span">_k_</span> swaps.

</div>
</div>
<div id="vj_input" class="hiddable">

Input

<div class="textBG">

The single line contains two integers <span class="tex-span">_a_</span> and <span class="tex-span">_k_</span><span class="tex-span">(1 ≤ _a_ ≤ 10<sup class="upper-index">18</sup>; 0 ≤ _k_ ≤ 100)</span>.

</div>
</div>
<div id="vj_output" class="hiddable">

Output

<div class="textBG">

Print the maximum number that Pasha can get if he makes at most <span class="tex-span">_k_</span> swaps.

</div>
</div>
<div id="vj_sampleInput" class="hiddable">

Sample Input

<div class="textBG">
<div class="input">
<div class="title">Input</div>
<pre>1990 1</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>9190</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>300 0</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>300</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>1034 2</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>3104</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre class="">9090000078001234 6</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre class="">9907000008001234
</pre>
</div>
</div>
</div>
```
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<cstring>
using namespace std;
int main()
{
	long int n, x[100008], y[100008],  i = 0, j,xxx[100008],yyy[100008],u;
	while (scanf("%d", &n) != EOF)
	{

		memset(xxx, 0, sizeof(xxx));
		memset(yyy, 0, sizeof(yyy));
		for (i = 0; i < n; i++)
		{
			cin >> x[i] >> y[i];

			xxx[x[i]]++;
			yyy[y[i]]++;
		}
		for (i = 0; i < n; i++)
		{
			u = n - xxx[y[i]] - 1;
			cout << 2*n - 2 - u  << ' ' << u << endl;
		}

	}
	return(0);
}
```
&nbsp;

* * *

#### codeforce 432B - Football Kit

<div id="vj_description" class="hiddable">

Description

<div class="textBG">

Consider a football tournament where <span class="tex-span">_n_</span> teams participate. Each team has two football kits: for home games, and for away games. The kit for home games of the <span class="tex-span">_i_</span>-th team has color <span class="tex-span">_x_<sub class="lower-index">_i_</sub></span> and the kit for away games of this team has color <span class="tex-span">_y_<sub class="lower-index">_i_</sub></span><span class="tex-span">(_x_<sub class="lower-index">_i_</sub> ≠ _y_<sub class="lower-index">_i_</sub>)</span>.

In the tournament, each team plays exactly one home game and exactly one away game with each other team (<span class="tex-span">_n_(_n_ - 1)</span> games in total). The team, that plays the home game, traditionally plays in its home kit. The team that plays an away game plays in its away kit. However, if two teams has the kits of the same color, they cannot be distinguished. In this case the away team plays in its home kit.

Calculate how many games in the described tournament each team plays in its home kit and how many games it plays in its away kit.

</div>
</div>
<div id="vj_input" class="hiddable">

Input

<div class="textBG">

The first line contains a single integer <span class="tex-span">_n_</span><span class="tex-span">(2 ≤ _n_ ≤ 10<sup class="upper-index">5</sup>)</span> — the number of teams. Next <span class="tex-span">_n_</span> lines contain the description of the teams. The <span class="tex-span">_i_</span>-th line contains two space-separated numbers <span class="tex-span">_x_<sub class="lower-index">_i_</sub></span>, <span class="tex-span">_y_<sub class="lower-index">_i_</sub></span><span class="tex-span">(1 ≤ _x_<sub class="lower-index">_i_</sub>, _y_<sub class="lower-index">_i_</sub> ≤ 10<sup class="upper-index">5</sup>; _x_<sub class="lower-index">_i_</sub> ≠ _y_<sub class="lower-index">_i_</sub>)</span> — the color numbers for the home and away kits of the<span class="tex-span">_i_</span>-th team.

</div>
</div>
<div id="vj_output" class="hiddable">

Output

<div class="textBG">

For each team, print on a single line two space-separated integers — the number of games this team is going to play in home and away kits, correspondingly. Print the answers for the teams in the order they appeared in the input.

</div>
</div>
<div id="vj_sampleInput" class="hiddable">

Sample Input

<div class="textBG">
<div class="input">
<div class="title">Input</div>
<pre>2
1 2
2 1</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>2 0
2 0</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>3
1 2
2 1
1 3</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre class="">3 1
4 0
2 2</pre>
</div>
</div>
</div>
```
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<cstring>
using namespace std;
int main()
{
	long int n, x[100008], y[100008],  i = 0, j,xxx[100008],yyy[100008],u;
	while (scanf("%d", &n) != EOF)
	{

		memset(xxx, 0, sizeof(xxx));
		memset(yyy, 0, sizeof(yyy));
		for (i = 0; i < n; i++)
		{
			cin >> x[i] >> y[i];

			xxx[x[i]]++;
			yyy[y[i]]++;
		}
		for (i = 0; i < n; i++)
		{
			u = n - xxx[y[i]] - 1;
			cout << 2*n - 2 - u  << ' ' << u << endl;
		}

	}
	return(0);
}
```
&nbsp;

* * *

#### Codeforces 431B Shower Line

<div id="vj_description" class="hiddable">

Description

<div class="textBG">

Many students live in a dormitory. A dormitory is a whole new world of funny amusements and possibilities but it does have its drawbacks.

There is only one shower and there are multiple students who wish to have a shower in the morning. That's why every morning there is a line of five people in front of the dormitory shower door. As soon as the shower opens, the first person from the line enters the shower. After a while the first person leaves the shower and the next person enters the shower. The process continues until everybody in the line has a shower.

Having a shower takes some time, so the students in the line talk as they wait. At each moment of time the students talk in pairs: the<span class="tex-span">(2_i_ - 1)</span>-th man in the line (for the current moment) talks with the <span class="tex-span">(2_i_)</span>-th one.

Let's look at this process in more detail. Let's number the people from 1 to 5\. Let's assume that the line initially looks as 23154 (person number 2 stands at the beginning of the line). Then, before the shower opens, 2 talks with 3, 1 talks with 5, 4 doesn't talk with anyone. Then 2 enters the shower. While 2 has a shower, 3 and 1 talk, 5 and 4 talk too. Then, 3 enters the shower. While 3 has a shower, 1 and 5 talk, 4 doesn't talk to anyone. Then 1 enters the shower and while he is there, 5 and 4 talk. Then 5 enters the shower, and then 4 enters the shower.

We know that if students <span class="tex-span">_i_</span> and <span class="tex-span">_j_</span> talk, then the <span class="tex-span">_i_</span>-th student's happiness increases by <span class="tex-span">_g_<sub class="lower-index">_ij_</sub></span> and the <span class="tex-span">_j_</span>-th student's happiness increases by <span class="tex-span">_g_<sub class="lower-index">_ji_</sub></span>. Your task is to find such initial order of students in the line that the total happiness of all students will be maximum in the end. Please note that some pair of students may have a talk several times. In the example above students 1 and 5 talk while they wait for the shower to open and while 3 has a shower.

</div>
</div>
<div id="vj_input" class="hiddable">

Input

<div class="textBG">

The input consists of five lines, each line contains five space-separated integers: the <span class="tex-span">_j_</span>-th number in the <span class="tex-span">_i_</span>-th line shows <span class="tex-span">_g_<sub class="lower-index">_ij_</sub></span> (<span class="tex-span">0 ≤ _g_<sub class="lower-index">_ij_</sub> ≤ 10<sup class="upper-index">5</sup></span>). It is guaranteed that <span class="tex-span">_g_<sub class="lower-index">_ii_</sub> = 0</span> for all <span class="tex-span">_i_</span>.

Assume that the students are numbered from 1 to 5.

</div>
</div>
<div id="vj_output" class="hiddable">

Output

<div class="textBG">

Print a single integer — the maximum possible total happiness of the students.

</div>
</div>
<div id="vj_sampleInput" class="hiddable">

Sample Input

<div class="textBG">
<div class="input">
<div class="title">Input</div>
<pre>0 0 0 0 9
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
7 0 0 0 0</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre>32</pre>
</div>
<div class="input">
<div class="title">Input</div>
<pre>0 43 21 18 2
3 0 21 11 65
5 2 0 1 4
54 62 12 0 99
87 64 81 33 0</pre>
</div>
<div class="output">
<div class="title">Output</div>
<pre class="">620</pre>
</div>
</div>
</div>
```
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<cstring>
using namespace std;
int main()
{
	long int o[5][5], i,j, ii, iii, a, b, c, d, e,m=0,max=0;
	while (scanf("%d", &o[0][0]) != EOF)
	{
		for (i = 0; i < 5; i++)
			for (j = 0; j < 5; j++)
			{
				if (i == 0 && j == 0)j++;
				cin >> o[i][j];
			}
		for (a = 0; a < 5; a++)
			for (b = 0; b < 5;b++)
				for (c = 0; c < 5; c++)
					for (d = 0; d < 5; d++)
						for (e = 0; e < 5; e++)
						{
							if(a!=b&&a!=c&&a!=d&&a!=e&&b!=c&&b!=d&&b!=e&&c!=d&&c!=e&&d!=e)
							m = o[a][b] + o[b][a] + o[b][c] + o[c][b] + (o[c][d] + o[d][c]) * 2 + (o[d][e] + o[e][d]) * 2;
							if (m>max) max = m;
						}
		cout << max<<endl;
	}
	return(0);
}
```
&nbsp;