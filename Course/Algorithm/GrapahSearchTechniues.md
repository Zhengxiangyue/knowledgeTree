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
-  ​

