---
title: 总结(VIII) Topological sort --- 拓扑排序总结
date: 2016-01-13 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---
今天终于把拓扑排序好好的看了一遍，终于稍微搞懂了。其实还是比较简单的。我们只要明白一点：拓扑排序只能作用在有向无环图之中。明白了这个，我们的题目就比较好做了。
<!--more-->

首先是有向无环图，这里我就直接摘抄网上一个写的比较好的例子来说明。

拓扑排序其实就是有向无环图的所有顶点的线性序列。且该序列必须满足下面两个条件：

* 每个顶点出现且只出现一次。
* 若存在一条从顶点 A 到顶点 B 的路径，那么在序列中顶点 A 出现在顶点 B 的前面。

然后，我们就可以开始进行拓扑排序了，我们可以参照下面的步骤

* 从 DAG 图中选择一个 没有前驱（即入度为0）的顶点并输出。
* 从图中删除该顶点和所有以它为起点的有向边。
* 重复 1 和 2 直到当前的 DAG 图为空或当前图中不存在无前驱的顶点为止。后一种情况说明有向图中必然存在环。
"下面是图示：

一个普通的有向无环图:

<img src="http://img.blog.csdn.net/20150507001028284"/>

下面是拓扑排序的步骤。

<img src="http://img.blog.csdn.net/20150507001759702">

最后我们可以得到其中一个结果（结果不唯一）：
 { 1, 2, 4, 3, 5 }


### leetcode 例子

### 207. Course Schedule
There are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

For example:

```
2, [[1,0]]
```

There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

```
2, [[1,0],[0,1]]
```

There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

####code[java]
```
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // 用邻接矩阵来储存有向图
        int[][] matrix = new int[numCourses][numCourses];
        int[] inarray = new int[numCourses];
        
        // 建立矩阵，若pre点到current点有边，则设为1，同时current点的入度+1
        for(int i = 0; i < prerequisites.length; i++) {
            int pre = prerequisites[i][1];
            int current = prerequisites[i][0];
            if(matrix[pre][current] == 0)
                inarray[current]++;
            matrix[pre][current] = 1;
        }
        
        // 先把入度为0的点加入队列
        LinkedList<Integer> que = new LinkedList<Integer>();
        for(int i = 0; i < inarray.length; i++) {
            if(inarray[i] == 0)
                que.add(i);
        }
        int count = 0;
        while(que.size() != 0) {
            int current = que.poll();
            count++;
            for(int i = 0; i < numCourses; i++){
                if(matrix[current][i] != 0) { // 判断入度为0的课到该课程上是否有边
                    inarray[i]--;   // 删除该边
                    if(inarray[i] == 0) // 如果该点入度也为0了，加入队列
                        que.add(i);
                }
            }
        }
        // 如果该图是一个有向无环图，则最后拓扑排序一定能遍历所有点，否则不是。
        return count == numCourses;
    }
}

```

***