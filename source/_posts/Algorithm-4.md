---
title: Algorithm 4
date: 2022-01-22 17:36:10
tags:
   - IT-Basics
   - Algorithm
---


### 简介
算法第四章，示例分析说明动态规划。


### 背包问题
通过这个示例说明动态规划中的最优子结构怎么来的。

有6个不同重量的物品：1kg，5kg，10kg，20kg，50kg，100kg要求用最少的物品，使承重为w的背包最重。
// 枚举策略
能直接想到的方法，我们把不同重量组合全部枚举出来，然后选出一个最合适的；这种思路虽然简单，但实现起来复杂，且耗费时间和空间。
// 贪心策略
那根据我们的一般经验来看，如果要用最少的物品，那我们首先就要选择较重的物品，换句话说我们为了最重目标，先考虑局部的最优解，这种方式就是贪心算法。不过这种方式某些情况下得不出正确的结果。
// 贪心策略局限
比如现在有1kg，5kg和11kg，背包承重15kg，如果按照贪心策略，我们首先选择11kg，接着我们必须要选择4件1kg才能使背包最重，所以我们最重需要5件物品。而实际上，选择3件5kg的物品才是最优解。
// 动态规划
继续分析，背包承重w=15kg时，如果我们取11kg的物品，那背包剩余重量就为4kg；如果取5kg的物品，剩余重量就为10kg；如果取1kg的物品，那背包剩余重量为14kg；如果我们把剩余重量看作一个新的背包，就又出现了相同的问题——用最少的物品，使背包最重；也就是最优子结构，我们用公式表达出来：
{% codeblock %}
// w=15时
取1kg，f(15) = f(15-1) + 1
取5kg，f(15) = f(15-5) + 1
取11kg，f(15) = f(15-11) + 1

// 由此推出，如果w=n时
取1kg，f(n) = f(n-1) + 1
取5kg，f(n) = f(n-5) + 1
取11kg，f(n) = f(n-11) + 1

// 所以，最优子结构就是：
f(n)=min{f(n-1),f(n-5),f(n-11)} + 1

// 代码示例
fun dynamicAlg(n: Int) {
   val f = IntArray(100)
   var count = 0
   for (i in 1..n) {
      if (i - 1 >= 0) count = Math.min(count, f[i - 1] + 1)
      if (i - 5 >= 0) count = Math.min(count, f[i - 5] + 1)
      if (i - 11 >= 0) count = Math.min(count, f[i - 11] + 1)
      f[i] = count
      System.out.printf("f[%d] = %d\n", i, f[i])
   }
}
{% endcodeblock %}


### 斐波那契数列
通过该示例说明下动态规划避免了重复计算。
// 最优子结构
{% asset_img fibonacci_func.png divide conquer algorithm %}

// 重复计算
{% asset_img fibonacci_child.png divide conquer algorithm %}

// 存储结果，避免重复
{% codeblock %}
var  fib = fun (n: Int): Int {
   var dp = intArrayOf()
   if (n == 0) return 0
    if(n==1 || n==2) {
      return 1
   }

   dp[0] = 0
   dp[1] = 1

   var i = 2
   for (i in 2..n) {
      dp[i] = dp[i-1] + dp[i-2]
   }
   return dp[n]
}
{% endcodeblock %}


### 递归
// 和上面动态规划求解斐波那契数列对比看差别
{% codeblock %}
var  fib = fun (n: Int): Int {
   if(n <= 0) {
      return 0
   }

   if(n==1 || n==2) {
      return 1
   }

   return fib(n-2)+fib(n-1)
}
{% endcodeblock %}









































