### 19. Remove Nth Node From End of List

Given a linked list, remove the *n*th node from the end of list and return its head.

For example,

```
   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.

```

**Note:**
Given *n* will always be valid.
Try to do this in one pass.

双指针找到要删除的节点前一个位置，注意用一个 dummynode 来防止边界条件

### 23. Merge k Sorted Lists

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

用 priority_queue 存储当前所有节点，每次选取 top 的节点，如果 top 的 next 不为空，再把top 的next 压进来

```c++
struct cmp{
    bool operator()(ListNode* a, ListNode* b) {
        return a->val > b->val;
    }
};
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        
        priority_queue<ListNode*, vector<ListNode*>, cmp> pq;
        
        ListNode *dummy = new ListNode(0), *current = dummy;
        
        for(int i = 0 ; i < lists.size() ; i++)
            if(lists[i])
                pq.push(lists[i]);
         
        while(!pq.empty()) {
            
            current->next = pq.top();
            
            pq.pop();
            
            if(current->next->next) 
                pq.push(current->next->next);
            
            // 这句话要不要都行 current->next->next = nullptr;
            
            current = current->next;
            
        }
        return dummy->next;
    }
};
```

