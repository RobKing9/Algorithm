# Day3 | 203.移除链表元素 | 707.设计链表   |.反转链表 



## [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* cur = dummy;
        while(cur->next != NULL) {
            if (cur->next->val == val) {
                ListNode* temp = cur->next;
                cur->next = cur->next->next;	// 最后指向空
                delete temp;
            }else {
                cur = cur->next;
            }
        }
        return dummy->next;
    }
};
```

理解：虚拟头结点的使用

## [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

- get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

```cpp
class MyLinkedList {

public:
    struct ListNode {
        int val;
        ListNode * next;
        ListNode(int val):val(val), next(nullptr){}
    };

    MyLinkedList() {
        // 虚拟 头结点
        dummyHead = new ListNode(-1);
        m_size = 0;
    }
    
    int get(int index) {
        if(index >= m_size || index < 0) return -1;
        ListNode * cur = dummyHead->next;
        while(index --) {
            cur = cur->next;
        }
        return cur->val;
    }
    
    void addAtHead(int val) {
        addAtIndex(0, val);
        // ListNode * ndoe = new ListNode(val);
        // ndoe->next = dummyHead->next;
        // dummyHead->next = ndoe;
        // m_size ++;
    }
    
    void addAtTail(int val) {
        addAtIndex(m_size, val);
        // ListNode * node = new ListNode(val);
        // ListNode * cur = dummyHead;
        // while(cur->next) cur = cur->next;
        // cur->next = node;
        // m_size ++;
    }
    // 插入 在index之前插入
    void addAtIndex(int index, int val) {
        if(index > m_size) return;
        if(index < 0) index = 0;

        ListNode * cur = dummyHead;
        while(index -- ) {
            cur = cur->next;
        }
        ListNode * node = new ListNode(val);
        node->next = cur->next;
        cur->next = node;
        m_size ++;
    }
    
    void deleteAtIndex(int index) {
        if(index >= m_size || index < 0) return;
        ListNode * cur = dummyHead;
        while(index --) {
            cur = cur->next;
        }
        // 删除当前结点
        ListNode * tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        m_size --;
    }
private:
    ListNode * dummyHead;
    int m_size;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

理解：注意几个问题

- 变量的定义在私有成员中
- `addAtIndex, index`的条件是可以等于`m_size`的，表示在最后插入

## [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // 之所以为空，是因为第一个指向的是空
        ListNode * pre = nullptr;
        ListNode * next = nullptr;
        while(head) {
            // 下一个结点改变了，需要提前保存下来
            next = head->next;
            // 将当前结点的next指针 指向上一个节点
            head->next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
};
```

理解：循环结束的条件是头结点不为空。

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        // 链表长度
        int len = 0;
        ListNode * cur = head;
        while(cur) {
            cur = cur->next;
            len ++;
        }
        ListNode * dummy = new ListNode(0);
        dummy->next = head;
        cur = dummy;
        int idx = len - n;
        while(idx --) {
            cur = cur->next;
        }
        cur->next = cur->next->next;

        return dummy->next;

    }
};
```

快慢指针方法

简单来说就是两个指针共同走完整条链表	

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * dummy = new ListNode(0);
        dummy->next = head;
        // 快慢指针
        ListNode * slow = dummy;
        ListNode * fast = dummy;

        // 先移动 n + 1 步，这样快指针移动结尾的时候，慢指针刚好移动到需要删除的结点的前面
        int step = n + 1;
        while(step --) fast = fast->next;

        while(fast) {
            fast = fast->next;
            slow = slow->next;
        }

        slow->next = slow->next->next;

        return dummy->next;

    }
};
```

## [面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(headB == nullptr || headA == nullptr) return nullptr;
        ListNode * curA = headA;
        ListNode * curB = headB;

        while(curA != curB) {
            if(curA) curA = curA->next;
            else curA = headB;

            if(curB) curB = curB->next;
            else curB = headA;
        }

        return curA;
    }
};
```

理解：注意循环结束条件即可，如果不想交，那么也会同时走向空节点会退出循环返回null