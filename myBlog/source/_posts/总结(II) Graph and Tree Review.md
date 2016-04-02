---
title: 总结(II) Graph and Tree Review --- 图和树的总结
date: 2015-12-29 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---
***
## 图
图的话我现在也就基本掌握了***BFS*** 和 ***DFS***，在难的像什么有向图，流的搜索，拓扑排序，最小生成树什么的，大二考完试后除了记得大体思路外，具体编码已经忘记怎么做了。以后再慢慢进行补充好了。

首先还是总结基本的算法。
***
### DFS
DFS其实就是把每次搜索的点加入到***栈***当中，一直搜索知道栈为空。这个方法有时候在树的问题也会用的到。下面是代码的基本思路。

这个代码是找判断给出的两个节点是否联通，如果联通就返回ture，不连通返回false。

####code[java]
```

public class Solution {
	enum State {Unvisit, Visited, Visting};

	public static boolean search(Graph g,Node start,Node end) {
		if(start == end)
			return true;

		for(Node u : g.getNodes())
			u.state = State.Unvisit;

		Stack<Node> s = new Stack<Node>();

		start.state = State.Visting;
		s.push(start);

		while(!s.isEmpty()) {
			Node current = s.pop();
			if(current != null) {
				for(Node v : u.getAdjacent()) {
					if (v.state == State.Unvisit) {
						if(v == end)
							return true;
						else {
							v.state = State.Visting;
							s.push(v);
						}
					}
				}
			}
			current.state = State.Visited;
		}
		return false;
	}

}

```

***
### BFS
BFS思想和DFS类似，把每次搜索的点都放进***队列***当中，一直搜索知道队列为空为止。代码和例子和上述差不多，就是把stack改成Queue即可。具体的以后再补充。

***
## 树
树一定要掌握的还是***前序，中序，后续***， 递归和非递归的方法都应该掌握。

***
### 前序

访问顺序是: 访问，左子树，右子树

####code[java],递归版本
```
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        tral(result, root);
        return result;
    }
    
    public void tral(ArrayList<Integer> result, TreeNode root) {
        if(root == null)
            return;
        result.add(root.val); // 中序则把这行放下一行，后序就把这行放到最后一行。
        tral(result, root.left);
        tral(result, root.right);
    }
}

```

####code[python],非递归版本

```
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        stac = []
        result = []
        p = root
        while len(stac) != 0 or p != None:
            if p != None:
                stac.append(p)
                result.append(p.val)
                p = p.left
            else:
                top =stac.pop()
                p = top.right
        return result
```

***
### 中序和后序
基本上看了前序以后，中序和后序就差不多了。递归版本的话，就是读取和访问的顺序不同而已。上面已经在代码上标注了，下面再次把三种方式总结：

* 前序访问顺序是: 访问，左子树，右子树
* 中序访问顺序是: 左子树，访问，右子树
* 后序访问顺序是: 左子树，右子树，访问

***
### 树的构建
比较基本的还有树的构建方面的题目，比如给出中序和后序访问的数组，构建一个树，或者给出前序和中序，构建一个树。

***
####Construct Binary Tree from Inorder and Postorder Traversal
####code[java] 

```
public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int rootindex = 0;
        if(inorder==null || postorder==null || inorder.length==0 || postorder.length==0) 
            return null;
        TreeNode root = new TreeNode(postorder[postorder.length - 1]);
        if(postorder.length==1) 
            return root;
            
        for(int i = 0; i < inorder.length; i++) {
            if(inorder[i] == postorder[postorder.length - 1]) {
                rootindex = i;
                break;
            }
        }
        
        int[] inleft = Arrays.copyOfRange(inorder, 0, rootindex);
        int[] inright = Arrays.copyOfRange(inorder, rootindex + 1, inorder.length);
        int[] postleft = Arrays.copyOfRange(postorder, 0, rootindex);
        int[] postright = Arrays.copyOfRange(postorder, rootindex, postorder.length - 1);
        root.left = buildTree(inleft, postleft);
        root.right = buildTree(inright, postright);
        return root;
        
    }
}
```

***
####Construct Binary Tree from Preorder and Inorder Traversal
####code[java] 
```
public class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int rootindex = 0;
        if(inorder==null || preorder==null || inorder.length==0 || preorder.length==0) 
            return null;
        TreeNode root = new TreeNode(preorder[0]);
        if(preorder.length==1) 
            return root;
            
        for(int i = 0; i < inorder.length; i++) {
            if(inorder[i] == preorder[0]) {
                rootindex = i;
                break;
            }
        }
        
        int[] inleft = Arrays.copyOfRange(inorder, 0, rootindex);
        int[] inright = Arrays.copyOfRange(inorder, rootindex + 1, inorder.length);
        int[] preleft = Arrays.copyOfRange(preorder, 1, rootindex + 1);
        int[] preright = Arrays.copyOfRange(preorder, rootindex + 1, preorder.length);
        root.left = buildTree(preleft, inleft);
        root.right = buildTree(preright, inright);
        return root;
    }
}
```

