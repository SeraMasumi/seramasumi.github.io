---
layout: default
title: Dynamic Programming & Memorization
nav_order: 3
parent: Algorithms
---

# Dynamic Programming & Memorization

## 由来

LC377 Combination Sum IV

方法一：Dynamic Programming（6ms）

```java
public class Solution {
    // combinations[target] 
    // = sum(combinations[target-nums[i]]), i : 0 -> n
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i = 1; i < dp.length; i++) {
            for(int j = 0; j < nums.length; j++) {
                if(i - nums[j] >= 0) {
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
}
```

方法二：Recursion+Memorization （1ms）

```java
public class Solution {
    private int[] dp;
    public int combinationSum4(int[] nums, int target) {
        dp = new int[target + 1];
        Arrays.fill(dp, -1);
        dp[0] = 1;
        return helper(nums, target);
    }

    private int helper(int[] nums, int target) {
        if (dp[target] != -1) {
            return dp[target];
        }
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            if (target >= nums[i]) {
                res += helper(nums, target - nums[i]);
            }
        }
        dp[target] = res;
        return res;
    }
}
```

方法二显然比方法一快很多，刚开始不知道是为什么，觉得两个方法里都没有重复计算，反而recursion的递归调用应该更花时间才对。想到第一学期算法课上讲的rod-cutting问题，里面也有dp和recursion with mem这两种方法，于是做了调查。

## 结论

> What is difference between memoization and dynamic programming?

**Memoization** is a term describing an optimization technique where you cache previously computed results, and return the cached result when the same computation is needed again.

**Dynamic programming** is a technique for solving problems recursively and is applicable when the computations of the subproblems overlap.

Dynamic programming is *typically* implemented using tabulation, but can also be implemented using memoization. So as you can see, neither one is a "subset" of the other.

A reasonable follow-up question is: **What is the difference between tabulation (the typical dynamic programming technique) and memoization?**

When you solve a dynamic programming problem using tabulation you solve the problem "**bottom up**", i.e., by solving all related sub-problems first, typically by filling up an *n*-dimensional table. Based on the results in the table, the solution to the "top" / original problem is then computed.

If you use memoization to solve the problem you do it by maintaining a map of already solved sub problems. You do it "**top down**" in the sense that you solve the "top" problem first (which typically recurses down to solve the sub-problems).

A good slide from ~~here~~ (link is now dead, slide is still good though):

> - If all subproblems must be solved at least once, a bottom-up dynamic-programming algorithm usually outperforms a top-down memoized algorithm by a constant factor
>   - No overhead for recursion and less overhead for maintaining table
>   - There are some problems for which the regular pattern of table accesses in the dynamic-programming algorithm can be exploited to reduce the time or space requirements even further
> - If some subproblems in the subproblem space need not be solved at all, the memoized solution has the advantage of solving only those subproblems that are definitely required

stackoverflow上面这个问题讲的比较清楚了，DP是bottom-up的方法，依次从小到大解决每一个subproblem，知道达到top（即原本的问题）。而recersion with mem是top-down的，从大问题出发，我需要哪个subproblem的答案再去解这个subproblem。

因此，应该选用哪种方式解题取决于问题的结构：如果不一定要求解每个subproblem，那么recursion with mem是更有效率的。如果每个subproblem必然要被求解至少一次，考虑到recursion的递归调用开销，dp是更加简洁快速的。

回到combination sum IV这个题目，我们最终的答案可能根本就不包含input array中的一些元素，有很多subproblem是无需计算的，所以recursion with mem会是最优的解法。