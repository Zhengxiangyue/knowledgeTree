总结二叉树非递归的遍历方法，前序和中序一样，前序遍历压栈前访问，中序遍历压栈后访问。

从代码理解:

```c++
    void un_recurse(TreeNode* root) {
        stack<TreeNode*> stk;    
        while(root || !stk.empty()) {
            while(root) {
                /*
                	In-order visit root here
                */
                stk.push(root);
                root = root->left;
            }            
            /*
            	Pre-order visit stack top here	
            */
            root = stk.top()->right;
            stk.pop();   
        }
        
    }
```

总结：一路往左压，来到 top.right(精髓就是这个，无论是否nullptr)，然后弹出 top

后续遍历：

```c++
void un_recurse(TreeNode* root) {
    stack<TreeNode*> stk;
    TreeNode* pre = nullptr;
    stk.push(root);
    
    while(!stk.empty()) {
        root = stk.top();
        if(!root->left && !root->right || pre 
           && (pre == root->left || pre == root->right)) {
            /*
            	Visit root here
            */
            stk.pop();
            pre = root;
        }else {
            if(root->right) stk.push(root->right);
            if(root->left) stk.push(root->left);
        }
    }
}
```
看栈顶是不是没有孩子，是不是上一个访问元素的父亲节点，若是弹出访问；若不是先压右孩子，再压左孩子