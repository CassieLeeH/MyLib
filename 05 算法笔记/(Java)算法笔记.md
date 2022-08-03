# 数据结构

## 数组/链表

### 前缀和

\303. 区域和检索 - 数组不可变（中等） 

\304. ⼆维区域和检索 - 矩阵不可变（中等） 

\560. 和为K的⼦数组（中等） 

**前缀和主要适⽤的场景是原始数组不会被修改的情况下，频繁查询某个区间的累加和的情况。**

要计算一个数组（i，j）之间的总和，例如nums := []int{3,5,2,-2,4,1}, 常规思路是，从第i个数加到第j个数，计算总和。但是如果需要反复调用计算，那每次查询的时间复杂度为O(N). 前缀和的核⼼思路是我们 new ⼀个新的数组 preSum 出来，preSum[i] 记录 nums[0..i-1] 的累加和：如求（2，5）则preSum[6]-preSum[2], 时间复杂度O(1).

![img](assets/1649818863260-883b0f9f-e306-4a08-9f8e-d43fc7b59516.png)

**和为 k 的⼦数组**

![img](assets/1649833201778-c079ba36-e03b-470b-be4d-9deef5177c4b.png)

**方法一：先计算数组前缀和，穷举所有子数组，找出符合条件个数；**时间复杂度 O(N^2) 空间复杂度 O(N)，并不是最优的解法。

**方法二：直接记录下有⼏个** **preSum[j]** **和** **preSum[i] - k** **相等，直接更新结果，就避免了内层** 

**的 for 循环。**我们可以⽤**哈希表**，在记录前缀和的同时记录该前缀和出现的次数。



### 差分数组

\370. 区间加法（中等） 

\1109. 航班预订统计（中等） 

\1094. 拼⻋（中等） 

**差分数组的主要适⽤场景是频繁对原始数组的某个区间的元素进⾏增减。** 

![img](assets/1649989072520-b7492be2-84c9-47ab-818f-cfc8a452792d.png)

```go
//构造差分数组
diff := make([]int,len(nums))
diff[0] = nums[0]
for i:=1;i<len(nums);i++{
    diff[i] = nums[i] - nums[i-1]
}
//一顿操作，对区间内加加减减
//对nums[i...j]之间加3，只要在差分数组diff[i]+=3,diff[j+1]-=3 注意j+1超出界限问题，j+1超界限，不减
//操作完了把diff数组还原

resnum := make([]int,len(diff))
resnum[0] = diff[0]
for i:=0;i<len(diff);i++{
    resnum[i] = resnum[i-1] +diff[i]
}
```

### 双指针

**数组双指针**

**26. 删除有序数组中的重复项** 

**83. 删除排序链表中的重复元素**

**27. 移除元素**

**283. 移动零**

**167. 两数之和 II - 输⼊有序数组** 

**344. 反转字符串**

**5. 最长回文子串**

**链表双指针**

1. **合并两个有序链表** 

双指针分别指向两个链表表头，比较P1,P2,把较小的接到新链表，移动指针，继续比较；这个算法的逻辑类似于「拉拉链」，l1, l2 类似于拉链两侧的锯⻮，指针 p 就好像拉链的拉索，将两个有序链表合并。 

1. **合并K个升序链表**

合并 k 个有序链表的逻辑类似合并两个有序链表，难点在于，如何快速得到 k 个节点中的最⼩节点，接到结 

果链表上？ 用优先级队列（⼆叉堆），把链表节点放⼊⼀个最⼩堆，就可以每次获得 k 个节点中的最⼩节点。

1. **环形链表**
2. **环形链表 II**
3. **链表的中间结点**
4. **删除链表的倒数第 N 个结点**
5. **相交链表**

### 二分查找

二分查找的前提是数组有序，难点在于定四个区间点。

```java
public int BinarySearch(int[] nums,int target){
   	 int high = nums.length - 1;
   	 int low = 0;
     while(low <= high){
       int mid = (high - low)/2 + low;
       if(nums[mid] == target){
         //找到了
         return mid;
       }else if(nums[mid] > target ){
         //大于目标值，找左区间
         high = mid - 1;
       }else{
         //小于目标值，找右区间
         low = mid + 1;
       }
     }
    //没找到
    return -1;
}
```

### 滑动窗口

![img](assets/1649593147122-04fd825b-579d-42d1-8c49-d4f7b34ddbba.png)

## 队列/栈

### 括号题

### LRU

## 树

### 二叉树

⼆叉树题⽬的难点在于如何通过题⽬的要求思考出每⼀个节点需要做什么。

![img](assets/1651993271660-d54a0390-0136-4bfe-80ba-172282a06021.jpeg)

**二叉树的遍历方式：** 

**深度优先遍历**：想往深走，遇到叶节点再往回走。前中后指的是中间节点的位置。

1. 1. 前序遍历（递归法，迭代法）中左右    计算二叉树的深度
   2. 中序遍历（递归法，迭代法）左中右
   3. 后序遍历（递归法，迭代法）左右中    计算二叉树的高度

**广度优先遍历**：从左到右一层一层的去遍历二叉树。

- - 层次遍历（迭代法）

```go
type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

//前序遍历二叉树
func preorderTraversal(root *TreeNode) []int {
	var res []int
	var preorder func(node *TreeNode)
	preorder = func(node *TreeNode) {
		if node == nil {
			return
		}
		res = append(res, node.Val)
		preorder(node.Left)
		preorder(node.Right)
	}
	preorder(root)
	return res
}

//中序遍历
func inorderTraversal(root *TreeNode) []int {
	var res []int
	var inorder func(node *TreeNode)
	inorder = func(node *TreeNode) {
		if node == nil {
			return
		}
		inorder(node.Left)
		res = append(res, node.Val)
		inorder(node.Right)
	}
	inorder(root)
	return res
}

//后序遍历
func postorderTraversal(root *TreeNode) []int {
	var res []int
	var postorder func(node *TreeNode)
	postorder = func(node *TreeNode) {
		if node == nil {
			return
		}
		postorder(node.Left)
		postorder(node.Right)
		res = append(res, node.Val)
	}
	postorder(root)
	return res
}

//层序遍历
func levelOrder(root *TreeNode) [][]int {
	var res [][]int
	if root == nil {
		return res
	}
	//树类型切片,q[0] = root
	q := []*TreeNode{root}
	//从上向下遍历
	for i := 0; len(q) > 0; i++ {
		res = append(res, []int{})
		var p []*TreeNode
		//从左到右遍历
		for j := 0; j < len(q); j++ {
			node := q[j]
			res[i] = append(res[i], node.Val)
			if node.Left != nil {
				p = append(p, node.Left)
			}
			if node.Right != nil {
				p = append(p, node.Right)
			}
		}
		q = p
	}
	return res
}
```



**二叉树的序列化：**

\297. 二叉树的序列化与反序列化





### 二叉搜索树

**二叉搜索树是⼀个有序树**：（中序遍历结果是有序的）

- 若它的左⼦树不空，则左⼦树上所有结点的值均⼩于它的根结点的值； 
- 若它的右⼦树不空，则右⼦树上所有结点的值均⼤于它的根结点的值； 
- 它的左、右⼦树也分别为⼆叉排序树。

**平衡二叉搜索树** 

平衡⼆叉搜索树：⼜被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是⼀棵空树或 

它的左右两个⼦树的⾼度差的绝对值不超过1，并且左右两个⼦树都是⼀棵平衡⼆叉树。

![img](assets/1652191188377-3555c8d6-8fd0-4482-9f96-cf7ad3981eeb.jpeg)

## 图

### 图的基础

图的本质是高级多叉树，适用于树的DFS/BFS遍历算法，常用邻接表和邻接矩阵来实现。

<img src="assets/image-20220710215627513.png" alt="image-20220710215627513" style="zoom: 33%;" />

```java
//图节点的逻辑结构
class Vertex{
  int id；
  Vertrx[] neighbors;
}
//⽤邻接表和邻接矩阵的存储⽅式如下：
// 邻接表: 空间占用少；无法快速判断两个节点是否相邻；
// graph[x] 存储 x 的所有邻居节点 
List<Integer>[] graph;
//邻接矩阵: 更多的存储空间；可以快速判断两个节点是否相邻；
//matrix[x][y] 记录 x 是否有⼀条指向 y 的边
//如果是有向加权图则用int型
boolean[][] matrix;
```

**遍历有向无环图：**

```java
class Solution {
    //记录所有路径
    List<List<Integer>> res = new LinkedList<>();

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        //维护递归过程中经过的路径
        LinkedList<Integer> path = new LinkedList<>();
        traverse(graph, 0, path);
        return res;
    }

    //图的遍历框架
    void traverse(int[][] graph, int s, LinkedList<Integer> path){
        //添加节点s到路径
        path.addLast(s);

        int n = graph.length;
        if(s == n-1 ){
            res.add(new LinkedList<>(path));
            path.removeLast();
            return;
        }
        //递归每个相邻节点
        for(int v : graph[s]){
            traverse(graph, v, path);
        }
        //从路径移出节点s
        path.removeLast();

    }
}
```



### 拓扑排列


### 二分图判定

### Union-Find(并查集) 

### 最小生成树

# 暴力搜索

## DFS/回溯算法

回溯算法其实就是我们常说的 DFS 算法，本质上就是⼀种暴⼒穷举算法。 解决⼀个回溯问题，实际上就是⼀个决策树的遍历过程，需要思考 3 个问题： 

1、路径：也就是已经做出的选择。 

2、选择列表：也就是你当前可以做的选择。 

3、结束条件：也就是到达决策树底层，⽆法再做选择的条件。

```go
func backtrack(nums []int, start int, target int) {
    //结束条件
    if trackSum == target {
        res = append(res, append([]int{}, track...))
        return
    }
    if trackSum > target {
        return
    }
    for i := start; i < len(nums); i++ {
        //剪枝
        if i > start && nums[i] == nums[i-1] {
            continue
        }
        //做选择
        track = append(track, nums[i])
        trackSum += nums[i]
        //递归
        backtrack(nums, i+1, target)
        //撤销选择
        track = track[:len(track)-1]
        trackSum -= nums[i]
    }
}
```

### 

![img](assets/1650865926029-febdb61e-c31c-49a4-a17c-1c6c386a0092.jpeg)

## BFS



# 动态规划

⾸先，**动态规划问题的⼀般形式就是求最值**。**求解动态规划的核⼼问题是穷举**。因为要求最值，肯定要把所有可⾏的答案穷举出来，然后在其中找最值呗。 

⾸先，动态规划的穷举有点特别，因为这类问题存在「**重叠⼦问题**」，如果暴⼒穷举的话效率会极其低下， 所以需要「备忘录」或者「DP table」来优化穷举过程，避免不必要的计算。 

⽽且，动态规划问题⼀定会具备「**最优⼦结构**」，才能通过⼦问题的最值得到原问题的最值。

另外，虽然动态规划的核⼼思想就是穷举求最值，但是问题可以千变万化，穷举所有可⾏解其实并不是⼀件 容易的事，只有列出正确的「**状态转移⽅程**」，才能正确地穷举。 

**明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义**

![img](assets/1649555679467-c3a46ec1-6bab-4f89-9b86-001db983844b.png)

![img](assets/1650247498541-e98d8786-5004-45c4-9789-6b1db8e507cb.jpeg)

# 数学相关

## 凸包问题

常见的凸包算法有多种： Jarvis 算法、 Graham 算法、 Andrew 算法