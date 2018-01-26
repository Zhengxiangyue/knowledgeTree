### I.Introduction

-  图搜索（遍历）技术用一种系统化的方式访问每一个节点一次
-  两种标准的搜索方法被广泛使用：
   -  1.深度优先搜索（DFS）
   -  2.广度优先搜索（BFS）


-  在二叉树中，三中递归遍历的技术被广泛使用：
   -  中序遍历
   -  先序遍历
   -  后序遍历

### II.Tree traversal techniques

中序遍历：

```c++
Procedure inorder(input: T)
begin
	if T = nil then
		return;
	endif
	
	inorder(T->left);  // traverse the left subtree recursively
	visit(T);		   // visit/process the root
	inorder(T->right); // traverse the right subtree recursively
end
```

先序遍历

```c++
Procedure perorder(input: T)
begin
	if T = nil then
		return;
	endif
	
	visit(T)			// visit/process the root
	preorder(T->left);  // traverse the left subtree recursively
	preorder(T->right); // traverse the right subtree recursively
end
```

后序遍历

```c++
Procedure perorder(input: T)
begin
	if T = nil then
		return;
	endif
	
	preorder(T->left);  // traverse the left subtree recursively
	preorder(T->right); // traverse the right subtree recursively
	visit(T)			// visit/process the root
end
```

### III. Depth-First Search

DFS遵循以下规则：

1.选择一个没有访问过的节点，把这个借点当做 current node

2.找一个没有访问过的 current node 的相邻节点，访问它，并且把这个节点当做新的 current node

3.如果 current node 的所有相邻节点都被访问过，回退到它的父节点，并把这个父节点当做 current node.

重复上面两步直到所有节点都被访问

4.如果还有未访问的节点，重复步骤1（不联通图？）



Implementation

-  观察：最后一个访问的节点是第一个处理的节点。并且回退过程也遵循“最后访问，最先回退”
-  这说明 stack 是比较合适的数据结构，用来记录 current node 以及如何回退

```c++
Procedure DFS(input: graph G)
begin
   Stack S;
   Integer s,x;
   while (G has an unvisited node) do
      s := an unvisited node;
      visit(v);
      push(v,S);
      While (S is not empty) do
         x := top(S);
         if (x has an unvisited neighbor y) then
            visit(y);
            push(y,S);
         else
            pop(S);
         endif
      endwhile
   endwhile
end
```

时间复杂度：

-  每一个节点被访问1次，每一个边(x,y)被访问了两次，第一次是当 从 x 点检查 y 是否北方问过（如果没有访问过，y 将被（从 x）访问），第二次是从 y 退回到 x 的时候
-  因此，DFS 的时间是 O（n+|E|）
-  如果图是联通的，时间复杂度是 O(|E|)因为图至少有 n-1个节点



DFS 的应用：连通性、无权图的生成树、双联通度





### V. Biconnectivity: A Major Application of DFS

-  定义：一个联通图中的**关节点**（articulation point）指的是如果删除这个节点，剩下的图不在联通
-  定义：如果一个联通图不存在关节点，他被称作 biconnected.也就是说删除任何一个节点，剩下的图都保持联通
-  *In the case of networks, an articulation point is referred to as a single point of failure.*
-  双连通问题：
   -  输入：一个联通图 G
   -  问题：判断图是否是双联通的，如果不是，找到所有 articulation points


-  对联通图使用 DFS，产生一个深度优先搜索树，它的所有边都来自图中的边。把这些边用直线画出来，把图中剩下的边用虚线画出来。
-  结论：每一条虚线都联通了一对祖先节点和孩子节点，从孩子节点到祖先节点


观察到的结论

-  如果根节点有多个子节点，那么根节点是一个 articulation point
-  每个节点都有两个新的量：DFN[i]和 L[i],DFN是节点在 dfs 时被访问的顺序，如根节点的 dfn 为1，L[w]是以 w 为根节点的树（加上 dash edge）的所有节点可以连接的最高的节点

为什么要定义这两个变量，因为很容易观察到，如果一个点（非 root）有子树并且任意一个子树可以利用dash edge 回到比这个点更高的点的话，删除这个点对这颗子树的连通性没有影响，一旦有子树回不到更高的点，删除那个点就会造成图不在联通






### 重连通图及重连通分量

在无向图中，如果任意两个顶点之间含有不止一条通路，这个图就被称为**重连通图**。在重连通图中，在删除某个顶点及该顶点相关的边后，图中各顶点之间的连通性也不会被破坏。

在一个无向图中，如果删除某个顶点及其相关联的边后，原来的图被分割为两个及以上的连通分量，则称该顶点为无向图中的一个关节点（或者“割点”）。

![2-1F9111F216335](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/2-1F9111F216335.png)
图 1 连通图

图 1 是连通图但不是重连通图，图中有4个关节点，分别是：A、B、D 和 G。比如删除顶点 B 及相关联的边后，原图就变为：

![2-1F9111F253129](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/2-1F9111F253129.png)

图 2 连通分量

可以看到，图被分割为各自独立的 3 部分，顶点集合分别为：{A、C、F、L、M、J}、{G、H、I、K} 和 {D、E}。

了解了什么是关节点后，重连通图其实就是没有关节点的连通图。

在重连通图中，只删除一个顶点及其相关联的边，肯定不会破坏其连通性。

如果一味地做删除顶点的操作，直到删除 K 个顶点及其关联的边后，图的连通性才遭到破坏，则称此重连通图的连通度为 K。

### 重连通图的实际应用

如今的通信网络对人们的生活有着重要的影响，如果将通信网络比做一个巨大的连通图的话，它的连通度 K 值越高，证明其稳定性越好，即使某一个站点发生故障无法工作也不会影响整个系统的正常工作。

同样，小到城市之间，大到国家之间的航空网也可以看作是一个连通图，但如果此图建设成为重连通图，当某条航线因为天气等因素关闭时，飞机仍可以从别的航线到达目的地。

在战争中，有“兵马未动，粮草先行”的说法，可见后勤补给对军队的重要性。如果补给线是一个重连通图，就不用过于担心补给线被破坏的问题，因为即使破坏一条，还有其它的，只要连通度足够大。

### 判断重连通图的方法

了解了什么是重连通图之后，如何编写程序直接判断一个图是否是重连通图呢？

对于任意一个连通图来说，都可以通过深度优先搜索算法获得一棵深度优先生成树，例如，图 1 通过深度优先搜索获得的深度优先生成树为：

![2-1F9111F601611](/Users/Cancel/Course/gitbooks/knowledgeTree/assets/2-1F9111F601611.png)
图 3 深度优先生成树

虚线表示遍历生成树时未用到的边，简称“回边”。也就是图中有，但是遍历时没有用到，生成树中用虚线表示出来。

在深度优先生成树中，图中的关节点有两种特性：

1. 首先判断整棵树的树根结点，如果树根有两条或者两条以上的子树，则该顶点肯定是关节点。因为一旦树根丢失，生成树就会变成森林。
2. 然后判断生成树中的每个非叶子结点，以该结点为根结点的每棵子树中如果有结点的回边与此非叶子结点的祖宗结点相关联，那么此非叶子结点就不是关节点；反之，就是关节点。
3. *a non-root node x is an articulation point if and only if x has a subtree from which no backward edge originates and ends at a proper ancestor of x.*

>  注意：必须是和该非叶子结点的祖宗结点（**不包括结点本身**）相关联，才说明此结点不是关节点。

所以，判断一个图是否是重连通图，也可以转变为：判断图中是否有关节点，如果没有关节点，证明此图为重连通图；反之则不是。

拿图 3 的生成树来说，利用两个特性判断每个顶点是否为关节点：

-  首先，判断树根结点 A ，由于有两个孩子，也就是有两棵子树，所以 A 是关节点。然后判断树中所有的非叶子结点，也就是： L 、 M 、 B 、 D 、 H 、 K 、 G ；
-  L 结点为根结点的子树中 B 结点有回边直接关联 A ，所以， L 不是关节点；
-  在以 M 结点为树根的子树中，J 结点和 B 结点都有回边关联 M 结点的祖宗结点，所以，M 不是关节点；
-  以 B 结点为根结点的 3 棵子树中，只有一棵子树（只包含结点 C ）与 B 结点的祖宗结点 A 有关联，其他两棵子树没有，所以结点 B 是关节点；
-  以 D 结点为根结点的子树中只有结点 E，且没有回边与祖宗结点关联，所以，D 是关节点；
-  以 H 结点为根结点的子树中， G 结点与 B 结点关联，所以， H 结点不是关节点；
-  K 结点和 H 结点相同，由于 G 结点与祖宗结点 B 关联，所以 K 结点不是关节点；
-  以 G 结点为根结点的子树中只有一个结点 I，没有回边，所以结点 G 是关节点；

综上所述，图 3 中的关节点有 4 个，分别是： A 、 B 、 D 、 G 。