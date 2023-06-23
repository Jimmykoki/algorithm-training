### Leetcode 232 Implement Queue using Stacks

Implement queue with stack

- 用两个栈维护，一个负责进栈，一个负责出栈，当调用 pop 函数，确保 stackOut 为空，转移所有 stackIn 的元素到 stackOut

```java
class MyQueue {

    Stack<Integer> stackIn;
    Stack<Integer> stackOut;

    public MyQueue() {
        // initiate two stack
        stackIn = new Stack<>();
        stackOut = new Stack<>();
    }

    public void push(int x) {
        stackIn.push(x);
    }

    public int pop() {
        dumpstackIn();
        return stackOut.pop();
    }

    public int peek() {
        dumpstackIn();
        return stackOut.peek();
    }
    // make sure two stack is empty
    public boolean empty() {
         return stackIn.isEmpty() && stackOut.isEmpty();
    }

    public void dumpstackIn(){
        if(!stackOut.isEmpty()) return;
        while(!stackIn.isEmpty()){
            stackOut.push(stackIn.pop());
        }
    }
}
/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

### Leetcode 225 Implement Stack using Queues

双队列解法

- time complexity push O(n) 其他为 O(1) space complexity O(n)
- 每次有元素进栈就必须重新排列一次（先出后进）

```java
class MyStack {
    Queue<Integer> queue1;
    Queue<Integer> queue2;

    public MyStack() {
        queue1 = new LinkedList<>();
        queue2 = new LinkedList<>();
    }

    public void push(int x) {
        queue2.offer(x);
        while(!queue1.isEmpty()){
            queue2.offer(queue1.poll());
        }
        Queue<Integer> temp;
        temp = queue1;
        queue1 = queue2;
        queue2 = temp;
    }

    public int pop() {
        return queue1.poll();
    }

    public int top() {
        return queue1.peek();
    }

    public boolean empty() {
        return queue1.isEmpty();
    }
}
```

单队列解法

```java
class MyStack {
    Queue<Integer> queue;

    public MyStack() {
        queue = new LinkedList<>();
    }

    public void push(int x) {
        queue.offer(x);
        int size = queue.size();
        size--;
        while(size-- > 0){
            queue.offer(queue.poll());
        }
    }

    public int pop() {
        return queue.poll();
    }

    public int top() {
        return queue.peek();
    }

    public boolean empty() {
        return queue.isEmpty();
    }
}
```
