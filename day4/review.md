### Leetcode 24.Swap Nodes in Pairs

递归写法

- 相邻的两个 node 相互交换

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode firstNode = head;
        ListNode secondNode = head.next;

        // Swap
        firstNode.next = swapPairs(secondNode.next);
        secondNode.next = firstNode;

        // Return the new head node
        return secondNode;
    }
}
```

迭代写法

-

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null) {
            return null;
        }
        if ( head.next == null) {
            return head;
        }
        ListNode dummyHead = new ListNode(-1, head);
        ListNode currentNode = dummyHead;
        // if there are odd number and even number of elements
        while ( currentNode.next !=null && currentNode.next.next != null){
            ListNode temp1 = currentNode.next;
            ListNode temp2 = currentNode.next.next.next;
            currentNode.next = currentNode.next.next;
            currentNode.next.next = temp1;
            temp1.next = temp2;
            currentNode = currentNode.next.next;
        }
        return dummyHead.next;
    }
}
```

### LeetCode 19. Remove Nth Node From End of List

双指针写法

-

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if ( head.next == null) return null;

        ListNode dummyHead = new ListNode(-1, head);
        ListNode fastIndex = dummyHead;
        ListNode slowIndex = dummyHead;

        for (int i = 0; i <= n; i++){
            fastIndex = fastIndex.next;
        }
        while (fastIndex != null){
            fastIndex = fastIndex.next;
            slowIndex = slowIndex.next;
        }
        slowIndex.next = slowIndex.next.next;
        return dummyHead.next;
    }
}
```

### Leetcode 160.Intersection of Two Linked Lists

迭代解法

- 返回两条单链表相交的 node, 并不是两个值相同的 node
- 计算链表的长度, 先保存头节 循环 while (curNode != null)
- 规定长链表先走，相差的位数，使两个链表保持在同一个长度
- 开始遍历两个链表，当遇到 curA == curB, 则返回当前节点

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        int lengthA = 0, lengthB = 0;
        while(curA != null){
            lengthA++;
            curA = curA.next;
        }// curA = null;
        while (curB != null){
            lengthB++;
            curB = curB.next;
        }
        curA = headA;
        curB = headB;
        if (lengthB > lengthA) {
            int tempLenght = lengthA;
            lengthA = lengthB;
            lengthB = tempLenght;
            ListNode temp = curA;
            curA = curB;
            curB = temp;
        }
        int gap = lengthA - lengthB;
        for (int i = 0; i < gap; i++){
            curA = curA.next;
        }
        while (curA != null){
            if(curA == curB){
                return curA;
            }
            curA = curA.next;
            curB = curB.next;
        }
        return null;
    }
}
```

### Leetcode 142.Linked List Cycle II

双指针写法

- 规定快指针走两步，慢指针走一步，如果有环则两指针必定在环内相遇
- 在相遇点设定一个新指针 point1，在链表头节点设定指针 point2，两指针相遇的第一个节点为环形入口节点

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next ==null) return null;
        ListNode fastIndex = head;
        ListNode slowIndex = head;

        while(true) {
            fastIndex = fastIndex.next.next;
            slowIndex = slowIndex.next;
            if (fastIndex == null || fastIndex.next == null) return null;
            if (fastIndex == slowIndex) {
                break;
            }
        }
        ListNode point1 = head;
        ListNode point2 = fastIndex;
        while (point1 != point2){
            point1 = point1.next;
            point2 = point2.next;
        }
        return point1;
    }
}
```
