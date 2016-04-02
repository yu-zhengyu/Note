---
title: 总结(VI) dynamic-programming review --- 动态规划题目总结
date: 2016-01-07 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---

动态规划的题目，一般可以在网上搜索***《背包九讲》***这个帖子，讲的还是挺好的，由浅入深。下面就把自己觉得遇到的比较典型的题目记录下来。
<!--more-->
***
### 1. Coin Change

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:
coins = [1, 2, 5], amount = 11
return 3 (11 = 5 + 5 + 1)

Example 2:
coins = [2], amount = 3
return -1.

这个题目就是凑硬币。一开始我没往动态规划上面想。我就觉得每次取最大，不行就倒退。后来发现这个想法不现实。还是只能一个一个求出来，然后由下往上求解。

####code[java] 非递归

基本就是bottom-up的解法。先凑1元，得出凑出1元最少需要多少个，然后求2元......以此类推，到求出amount。

```

public class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount == 0)
            return 0;
        int[] result = new int[amount + 1];        
        int sum = 0;
        while(++sum <= amount) {
            int min = -1;
            for(int i = 0; i < coins.length; i++) {
                if(sum >= coins[i] && result[sum - coins[i]] != -1) {
                    int t = result[sum - coins[i]] + 1;
                    if(min < 0)
                        min = result[sum - coins[i]] + 1;
                    else {
                        if(result[sum - coins[i]] + 1 < min)
                            min = result[sum - coins[i]] + 1;
                    }
                }
            }
            result[sum] = min;
        }
        return result[amount];
    }
}

```


####code[java] 递归

和非递归差不多，不过是由up到bottom。假设当前我们已经能得到当前的钱数，那么，遍历一遍coin，看前一个能凑成当前钱数最小的，那么当前的最小组合数就是lastsum+1。

```

public class Solution {
	public int coinChange(int[] coins, int amount) {
		if (amount < 1)
			return 0;
		return helper(coins, amount, new int[amount]);
	}

	private int helper(int[] coins, int rem, int[] count) { // rem: remaining
															// coins after the
															// last step;
															// count[rem]:
															// minimum number of
															// coins to sum up
															// to rem
		if (rem < 0)
			return -1; // not valid
		if (rem == 0)
			return 0; // completed
		if (count[rem - 1] != 0)
			return count[rem - 1]; // already computed, so reuse
		int min = Integer.MAX_VALUE;
		for (int coin : coins) {
			int res = helper(coins, rem - coin, count);
			if (res >= 0 && res < min)
				min = 1 + res;
		}
		count[rem - 1] = (min == Integer.MAX_VALUE) ? -1 : min;
		return count[rem - 1];
	}
}



```

***
