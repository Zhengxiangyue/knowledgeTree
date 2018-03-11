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

