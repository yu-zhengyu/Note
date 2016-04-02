---
title: N sum question review --- N sum 题目总结
date: 2016-01-04 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---

leetcode里面出了一系列的这种题目，分别是two sum，3 sum，3 sum closed，4 sum。 期中出了two sum是利用hashmap外，其他基本的套路都是差不多的，都是先固定一个数字，然后进行左右夹逼的。时间复杂度基本就是O(n^(N-1)), 期中 N 就是指由多少个sum。
<!--more-->

***
### 1. Two Sum

用一个哈希表,存储每个数对应的下标，因为这个题目需要返回下标，所以我们之前说的方法，先排序，再固定，后夹逼的方法在这题不太方便。所以还是老实用hash，时间是O(n), 空间是O(n).

####code[java]

```

public class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> a = new HashMap<Integer, Integer>();
        int[] result = new int[2];
        for(int i = 0; i < nums.length; i++) {
            if(a.containsKey(target - nums[i])) {
                result[0] = a.get(target - nums[i]) + 1;
                result[1] = i + 1;
                return result;
            }
            else
                a.put(nums[i], i);
        }
        return result;
    }
}
              
```

***
### 15. 3Sum

这题可以用我们所说的那套固定-夹逼的方法了。注意这题需要去掉一些重复的结果。直接上代码了。

PS: 找数字的时候注意去掉重复的。if-else的条件注意细节。

####code[java]

```
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        Arrays.sort(nums);
        if(nums.length < 3)
            return result;
        for(int i = 0; i < nums.length - 2; i++) {
            if(i > 0 && nums[i] == nums[i-1])
                continue;
            int begin = i + 1;
            int end = nums.length - 1;
            int fix = nums[i];
            while(begin < end) {
                if(nums[begin] + nums[end] + fix == 0) {
                    List<Integer> temp = new ArrayList<Integer>();
                    temp.add(fix);
                    temp.add(nums[begin]);
                    temp.add(nums[end]);
                    result.add(temp);
                    while(begin < end && nums[begin] == nums[begin + 1])
                        begin++;
                    while(begin < end && nums[end] == nums[end - 1])
                        end--;
                    begin++;
                    end--;
                }
                else if(nums[begin] + nums[end] + fix < 0)
                    begin++;
                else
                    end--;
            }
        }
        return result;
    }
}
```

***
### 16. 3Sum Closest

这题和上一题思路基本一样，改改条件就可以了。

####code[java]

```
public class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int minDiff = Integer.MAX_VALUE;
        int result = 0;
        Arrays.sort(nums);
        
        for(int a = 0; a < nums.length - 2; a++) {
            int b = a + 1;
            int c = nums.length - 1;
            
            while(b < c) {
                int gap = Math.abs(target - (nums[a] + nums[b] + nums[c]));
                if(gap < minDiff) {
                    minDiff = gap;
                    result = nums[a] + nums[b] + nums[c];
                }
                
                if(nums[a] + nums[b] + nums[c] < target)
                    b++;
                else
                    c--;
            }
        }
        return result;
    }
        
}

```

***
### 18. 4Sum

这题如果用排序加左右夹逼，时间是O(n^3)。有人说会超时，但是我的好像没问题。如果超时的话，可以尝试采用一个hash来做缓冲，每两个数字组成一个pair，这样的预处理就时间是O(n^2), 然后就是和2 sum的做法差不多。我这题没这样写，把别人的代码贴上学习一下：

####code[C++]

```

class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        if (nums.size() < 4) return result;
        sort(nums.begin(), nums.end());

        unordered_multimap<int, pair<int, int>> cache;
        for (int i = 0; i + 1 < nums.size(); ++i)
            for (int j = i + 1; j < nums.size(); ++j)
                cache.insert(make_pair(nums[i] + nums[j], make_pair(i, j)));

        for (auto i = cache.begin(); i != cache.end(); ++i) {
            int x = target - i->first;
            auto range = cache.equal_range(x);
            for (auto j = range.first; j != range.second; ++j) {
                auto a = i->second.first;
                auto b = i->second.second;
                auto c = j->second.first;
                auto d = j->second.second;
                if (a != c && a != d && b != c && b != d) {
                    vector<int> vec = { nums[a], nums[b], nums[c], nums[d] };
                    sort(vec.begin(), vec.end());
                    result.push_back(vec);
                }
            }
        }
        sort(result.begin(), result.end());
        result.erase(unique(result.begin(), result.end()), result.end());
        return result;
    }
};

```

***

####code[java] -- O(n^3)的版本

这个还是用我们之前的理论来做。效果还行。

```

public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        if (nums.length < 4) {
            return ans;
        }
        for (int i = 0; i < nums.length - 3; i++) {
            int j = i + 1; 
            if (i > 0 && nums[i] == nums[i-1])
                continue;
            while (j < nums.length - 2) {
                int k = j + 1, h = nums.length - 1;
                while (k < h) {
                    if (nums[i] + nums[j] + nums[k] + nums[h] == target) {
                        List<Integer> li = new ArrayList<>(Arrays.asList(nums[i], nums[j], nums[k], nums[h]));
                        ans.add(li);
                        while (k < nums.length - 1 && nums[k] == nums[k+1]) k++;
                        while (h > k && nums[h] == nums[h-1]) h--;
                        h--; k++;
                    } else if (nums[i] + nums[j] + nums[k] + nums[h] > target) {
                        h--;
                    } else {
                        k++;
                    }
                }
                while (j < nums.length -2 && nums[j] == nums[j+1]) j++;
                j++;
            }
         }
        return ans;
    }
}
```





