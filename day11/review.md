### Leetcode 20 Valid Parentheses

- 找出不符合的三种情况，多余的左括号，多余的右括号，左右括号不匹配

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        char[] charStr = s.toCharArray();
        for(int i = 0; i < charStr.length; i++){
            if(charStr[i] == '('){
                stack.push(')');
            } else if (charStr[i] == '{'){
                stack.push('}');
            } else if (charStr[i] == '['){
                stack.push(']');
            } else if (stack.isEmpty() || charStr[i] != stack.peek()){
                return false;
            } else {
                stack.pop();
            }
        }
        return stack.isEmpty();
    }
}
```

### Leetcode 1047 Remove All Adjacent Duplicates In String

利用栈

- 使用 ArrayDeque 堆栈，`ArrayDeque`在某些操作上比基于链表的实现（如`LinkedList`）更高效，特别是在队列的两端添加或删除元素
- 弹出的元素与目标字符串是相反的

```java
class Solution {
    public String removeDuplicates(String s) {
        Deque<Character> deque = new ArrayDeque<>();
        char[] charStr = s.toCharArray();
        for(char c : charStr){
            if(deque.isEmpty() || deque.peek() != c){
                deque.push(c);
            } else {
                deque.pop();
            }
        }
        String str = "";
        while(!deque.isEmpty()){
            // reverse the position of str
            str = deque.pop() + str;
        }
        return str;
    }
}
```

### Leetcode 150 Evaluate Reverse Polish Notation

双端队列

- 利用栈来处理后缀表达式
- 逆波兰表达式相当于是二叉树中的后序遍历
- 遇到数字则入栈；遇到运算符则取出栈顶两个数字进行计算，并将结果压入栈中
- time complexity O(n) space complexityO(n)

```java
class Solution {
    public int evalRPN(String[] tokens) {
       Deque<Integer> deque = new LinkedList<>();
        for(String str : tokens){
            switch(str) {
                case "+":
                    deque.push(deque.pop() + deque.pop());
                    break;
                case "-":
                    int temp1 = deque.pop();
                    int temp2 = deque.pop();
                    deque.push(temp2 - temp1);
                    break;
                case "*":
                    deque.push(deque.pop() * deque.pop());
                    break;
                case "/":
                    temp1 = deque.pop();
                    temp2 = deque.pop();
                    deque.push(temp2 / temp1);
                    break;
                default:
                    // If str is not an operator, then it must be a number
                    deque.push(Integer.valueOf(str));
                    break;
            }
        }
        return deque.peek();
    }
}
```
