---
title: Best-time-buy-and-sell-stock --- leetcode 买卖股票问题
date: 2016-01-24 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---

# level 1 
## 121. Best Time to Buy and Sell Stock

这题是第一层，因为只能买卖一次，维护一个全局最小值和一个全局最大值就好，最后返回一个全局最优解即可。

####code[java]
```

public class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0)
            return 0;
        int minmoney = prices[0];
        int maxmonty = 0;
        for (int i = 1; i < prices.length; i++) {
            if(prices[i] - minmoney > 0) {
                if(prices[i] - minmoney > maxmonty)
                    maxmonty = prices[i] - minmoney;
            }
            if(prices[i] < minmoney)
                minmoney = prices[i];
        }
        return maxmonty;
    }
}

```


# level 2
## 122. Best Time to Buy and Sell Stock II

这题也是比较简单的，因为可以随买随卖，所以只要后一天的价格比前一天的高，就卖就好，我觉得比 level I 还简单。

####code[java]
```

public class Solution {
    public int maxProfit(int[] prices) {
        int total = 0;
        for(int i = 0; i < prices.length - 1; i++) {
            if(prices[i+1] - prices[i] > 0)
                total += (prices[i+1] - prices[i]);
        }
        return total;
    }
}

```

# level 3
## 122. Best Time to Buy and Sell Stock III

这题还是需要技巧的，因为只能够买卖两次，而且买另一个之前需要卖掉前一个。所以这题我们没办法只用O(1)的空间了。这题而且需要两个数组，一个数组表示前一个 stock 买卖的情况，另一个数组表示后一个 stock 买卖的情况。最后再遍历两个数组，求两个数组相加后最大 profit。

第一个数组就和 level I 一样，把当前的全局最优解放到对应下标的数组就好。

第二个数组注意下标，因为进行的第二次交易，所以我们要从后往前遍历，记录下全局最大的 price 和到当前坐标下的全局最优解。

最后遍历两个数组，每一个下标的结果都加起来即可。

####code[java]
```

/**
 * @author zhengyu
 * @date 2016年1月24日
 */
public class Solution {
    /**
     * @param args
     */
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        int[] test = {1,2,4};
        int[] test2 = {4,6,3,5,6,7,8,3,4,5,3,5,6,2,5,8};
        System.out.println(maxProfit(test));
        System.out.println(maxProfit(test2));
    }
    
    public static int maxProfit(int[] prices) {
        if(prices.length == 0)
            return 0;
        int len = prices.length;
        int[] first = new int[len];
        int[] second = new int[len];
        
        int minprofit = prices[0];
        int maxprofit = 0;
        for(int i = 1; i < len; i++) {
            maxprofit = Math.max(maxprofit, prices[i] - minprofit);
            first[i] = maxprofit;
            minprofit = Math.min(minprofit, prices[i]);
        }
        
        int maxprice = prices[len - 1];
        maxprofit = 0;
        int index = len - 2;
        for(int i = len - 2; i > -1; i--) {
            maxprofit = Math.max(maxprofit, maxprice - prices[i]);
            second[index--] = maxprofit;
            maxprice = Math.max(maxprice, prices[i]);
        }
        
        int result = 0;
        for(int i = 0; i < len; i++) 
            result = Math.max(first[i] + second[i], result);
        return result;
    }

}

```

# level 4
## 188. Best Time to Buy and Sell Stock IV

这个应该是这个系列最难的题目了。还是前面的条件，但是这次能买卖多少次是用户自己输入的。

