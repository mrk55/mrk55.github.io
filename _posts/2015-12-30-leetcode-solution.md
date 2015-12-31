---
layout: post
title: LeetCode 322~312 题解
tags: leetcode algorithm
categories: algorithm
---

###**322. Coin Change**
题目的信息在[这里](http://mrkangi.github.io/2015-12-29/leetcode/#problem-322-coin-change)

看完题目，首先想到的是用“**动态规划**”解决，“**回溯法**”应该也是可以的。我的解法用了**带备忘录的自底向上的非递归动态规划算法**，该方法在**《算法导论》**里有介绍。

**算法思路**
算法思想是典型的动态规划，就不多做解释了。
{% highlight csharp linenos %}
int CoinChange(int[] coins, int amount)
{
    int[] memo = new int[amount + 1];//备忘录
    memo[0] = 0;
    for (int i = 1; i <= amount; i++)
    {
        memo[i] = -1;
        for (int j = 0; j < coins.Length; j++)
        {
            if (i - coins[j] >= 0 && memo[i - coins[j]] != -1)
            {//除去一枚硬币后的数量有解
                if (memo[i] == -1)//当前数量未解
                    memo[i] = memo[i - coins[j]] + 1;//直接应用
                else if (memo[i - coins[j]] + 1 < memo[i])//当前数量已解，求其最优解
                    memo[i] = memo[i - coins[j]] + 1;
            }
        }
    }
    return memo[amount];
}
{% endhighlight %}

###**321. Create Maximum Number**
题目的信息在[这里](http://mrkangi.github.io/2015-12-29/leetcode/#problem-321-create-maximum-number)

此题考虑用“**分支限界法**”，搜索解空间树，找出符合条件的解，然后求出最优解。

> 算法的实现过两天更新。

###**319. Bulb Switcher**

`2015-12-31 更新`

题目的信息在[这里](http://mrkangi.github.io/2015-12-29/leetcode/#problem-319-bulb-switcher)

此题有直观方法可解，但是效率较低。仔细观察后发现是有规律的。

**算法思路**

例如：

> n = 3  => 011

> n = 4  => 0110

> n = 5  => 01101

> n = 6  => 011011

> ...

> n = 8  => 01101111

> ...

> n = 15 => 011011110111111

> ...

以上0表示灯亮，1表示灯灭。

令t表示结果中亮灯的个数，可以看到，以上`n=3,8,15`的示例中`1`的个数为
![equ1] [equ1]，`0`的个数为t，
所以总数为t(t+1)+t=t(t+2)。当n满足![equ2] [equ2]时，t的值就是当前的亮灯的数量。

程序如下：
{% highlight csharp linenos %}
int BulbSwitcher(int n)
{
    for (int i = 0; i <= n / 2 + 1; i++)//i的判断条件稍显多余，此处可以对i的初值优化
    {
        if (i * i - 1 < n && n <= i * (i + 2))
            return i;
    }
    return 0;
}
{% endhighlight %}

[equ1]:  {{"/equ1.gif" | prepend: site.imgrepo }}
[equ2]:  {{"/equ2.gif" | prepend: site.imgrepo }}