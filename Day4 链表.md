# 两两交换链表中的节点  | 删除链表的倒数第N个节点  |

# 链表相交  | 环形链表II  



## [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode * dummy = new ListNode(0);
        dummy->next = head;
        ListNode * cur = dummy;

        while(cur->next && cur->next->next) {
            // 下一个结点改变了，需要保存位置
            ListNode * ne = cur->next;
            // 保存下三个节点，给第三步使用
            ListNode * neNe = cur->next->next->next;

            // 第一步：虚拟头结点指向第二个节点
            cur->next = cur->next->next;
            // 第二步：第二个节点指向第一个节点，反转
            cur->next->next = ne;
            // 第三步：第一个节点指向第三个节点，进入下一组
            ne->next = neNe;

            // 移动两步
            cur = cur->next->next;
        }

        return dummy->next;
    }
};
```

理解：最重要的就是循环的条件，之后必须要有两个节点才循环。画图理解三个步骤，保存临时节点。

## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr) return nullptr;

        ListNode * slow = head;
        ListNode * fast = head;

        while(1) {
            if(fast == nullptr || fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow) {
                break;
            }
        }

        fast = head;
        while(fast != slow) {
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

理解

没有环直接返回nil；有环的话，如果进入环之前有a个，内有b个；分别走f，s步

相遇的时候有：

1. f=2s （快指针每次2步，路程刚好2倍）
2. f = s + nb (相遇时，刚好多走了n圈）

推出：s = nb

从head结点走到入环点需要走 ： a + nb， 而slow已经走了nb，那么slow再走a步就是入环点了。

如何知道slow刚好走了a步？ 从head开始，和slow指针一起走，相遇时刚好就是a步

## 链表总结

链表最重要的就是虚拟头结点的使用了，然后就是快慢双指针