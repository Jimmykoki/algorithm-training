### Leetcode 203. Remove Linked List Elements

直接使用原来的链表进行删除操作

- head 是有效的，head 包括值以及指向下一个 node 的所在的内存地址
- 必须先判断头节点的值是否为目标值，为 true 的话，直接将头结点指向下一个节点
- 时间复杂度为 O(n), 必须遍历整个链表才能找到所有的存在的目标值

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        while (head != null && head.val == val){
            head = head.next;
        }
        if (head == null) {
            return head;
        }
      // head的值不是目标值
        ListNode pre = head;
        ListNode cur = head.next;
        while (cur != null) {
            if (cur.val == val) {
                pre.next = cur.next;
            } else {
                pre = cur;
            }
            cur = cur.next;
        }
        return head;
    }
}
```

设置虚拟头结点进行删除操作

- 定义初始化虚拟节点 dummy, 并设置虚拟节点的 next 为头结点 head
- 如果头结点为目标值，可以统一操作

```java
public ListNode removeElements(ListNode head, int val) {
    if (head == null) {
        return head;
    }
    // 因为删除可能涉及到头节点，所以设置dummy节点，统一操作
    ListNode dummy = new ListNode(-1, head);
    ListNode pre = dummy;
    ListNode cur = head;
    while (cur != null) {
        if (cur.val == val) {
            pre.next = cur.next;
        } else {
            pre = cur;
        }
        cur = cur.next;
    }
    return dummy.next;
}
```

### Leetcode 707. Design Linked List

设计链表

- 初始化链表的同时已经创建 dummyNode, val =0, dummyNode.next 指向链表的第一个值
- 设置虚拟头结点 dummyNode 可以使原链表用统一的方式移除所有的节点
- 找到前节点的循环，与找到当天节点的循环是不一样的，for (int i; i <= index; i++) 为当前节点，i < index 为前置节点
- 头结点的插入，index = 0, 末节点的插入 index = size;
- 插入与删除操作都必须先对 size 进行自增或自减

```java
class LinkedNode {
    int val;
    LinkedNode next;
    LinkedNode () {}
    LinkedNode (int val) {
        this.val = val;
    }
    LinkedNode (int val, LinkedNode next){
        this.val = val;
        this.next = next;
    }
}

class MyLinkedList {

    int size;
    LinkedNode head;

    MyLinkedList () {
        size = 0;
        head = new LinkedNode(0);
    }
    public int get(int index) {
        if(index <= -1 || index >= size ){
            return -1;
        }
        LinkedNode currentNode = head;
        // include the dummy index, starting from index + 1
        for (int i = 0; i <= index; i++) {
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }
    public void addAtHead(int val) {
        // LinkedNode firstNode = new LinkedNode(val);
        // LinkedNode temNode = head.next;
        // firstNode.next = temNode;
        // head.next = firstNode;
        addAtIndex(0, val);
    }
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if ( index < 0) {
            index =0;
        }
        size++;
        LinkedNode pre = head;
        for (int i = 0; i < index; i++){
            pre = pre.next;
        }
        LinkedNode newNode = new LinkedNode(val);
        newNode.next = pre.next;
        pre.next = newNode;
    }
    public void deleteAtIndex(int index) {
        if(index <= -1 || index >= size ){
            return;
        }
        size--;
        if (index ==0) {
            head = head.next;
            return;
        }
        LinkedNode pre = head;
        for (int i=0; i < index; i++){
            pre = pre.next;
        }
        pre.next = pre.next.next;
    }
}
```

### Leetcode 206. Reverse Linked List

双指针写法

- 初始化 pre 为 null, 当前节点为头节点 head
- 循环结束的条件：当前节点为 null, pre 指向链表的最后一个值

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode currentNode = head;
        while ( currentNode != null){
            ListNode temp = currentNode.next;
            currentNode.next = pre;
            pre = currentNode;
            currentNode = temp;
        }
        return pre;
    }
}
```

迭代写法
