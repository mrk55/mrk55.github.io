---
layout: post
title: LeetCode 322~312 题解
tags: leetcode algorithm
categories: algorithm
---

`声明:本博文内容均为作者原创，版权所有，转载请注明出处`

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
    for (int t = 0; t <= n / 2 + 1; t++)//t的判断条件稍显多余，此处可以对t的初值优化
    {
        if (t * t - 1 < n && n <= t * (t + 2))
            return t;
    }
    return 0;
}
{% endhighlight %}

###**318. Maximum Product of Word Lengths**

`2016-01-19 更新`

题目的信息在[这里](http://mrkangi.github.io/2015-12-29/leetcode/#problem-318-maximum-product-of-word-lengths)

此题也是可以暴力解决的，但是效率比较低。分析题目后可以发现，如果能够对每个`word`通过某种hash函数求出其hash值，
通过此值来判断两个`word`之间是否包含公共字符，将提高不少效率。

**算法思路**

我采用的hash函数的算法如下：`[a-z]`这26个字母分别代表32位无符号整形的 低1~低26 位，当`word`中包含`[a-z]`中的某个字符时，
相应的二进制位为`1`，例如：`"ab"`的hash值为`3`，即`00000000 00000000 00000000 00000011B`。

得到了hash值之后，接下来的工作就是对每两个hash值进行按位与运算，若结果为`0`，则相应的两个`word`没有公共字符，此时再寻找最大乘积。


程序如下：
{% highlight csharp linenos %}
public int MaxProduct(string[] words)
{
    int result = 0, tmpLen = 0;
    uint[] hashCodes = new uint[words.Length];
    for (int i = 0; i < words.Length; i++)
    {
        hashCodes[i] = HashCode(words[i]);
    }
    for (int i = 0; i < hashCodes.Length - 1; i++)
    {
        for (int j = i + 1; j < hashCodes.Length; j++)
        {
            if ((hashCodes[i] & hashCodes[j]) == 0)//哈希值按位与为0，即没有公共字符
            {
                tmpLen = words[i].Length * words[j].Length;
                result = tmpLen > result ? tmpLen : result;//得到长度乘积最大的值
            }
        }
    }
    return result;
}
//hash函数
private uint HashCode(string s)
{
    uint code = 0, mask;
    foreach (char c in s)
    {
        mask = 1;
        mask <<= (int)(c - 'a');
        code |= mask;
    }
    return code;
}
{% endhighlight %}

在`LeetCode`上提交之后，发现效率还可以，不过应该还有可优化的地方。

> **未完待续**

[equ1]:  {{"/equ1.gif" | prepend: site.imgrepo }}
[equ2]:  {{"/equ2.gif" | prepend: site.imgrepo }}