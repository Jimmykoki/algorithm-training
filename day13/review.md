## 二叉树的递归遍历

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
2. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

### Leetcode 144 Binary Tree Preorder Traversal

递归写法

- 中左右

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        preorder(root, res);
        return res;
    }

    public void preorder(TreeNode root, List<Integer> res){
        if(root == null){
            return;
        }
        res.add(root.val);
        preorder(root.left, res);
        preorder(root.right, res);
    }
}
```

迭代写法

- 前序遍历是中左右，每次先处理中间节点，那么先将根节点放入栈中，然后将右孩子加入栈，再加入左孩子

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) return res;
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode curNode = stack.peek();
            res.add(stack.pop().val);
            if(curNode.right != null) stack.push(curNode.right);
            if(curNode.left != null) stack.push(curNode.left);
        }
        return res;
    }
}
```

### Leetcode 145 Binary Tree Postorder Traversal

递归写法

- 左右中

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        postorder(root, res);
        return res;
    }

    public void postorder(TreeNode root, List<Integer> res){
        if(root == null){
            return;
        }
        postorder(root.left, res);
        postorder(root.right, res);
        res.add(root.val);
    }
}
```

迭代写法

- 修改前序遍历，中左右 ---> 中右左，调整入栈顺序
- 再反转整个链表 Collections.reverse() 中右左 ---> 左右中 ;

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) return res;
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode cur = stack.peek();
            res.add(stack.pop().val);
            if(cur.left != null ) stack.push(cur.left);
            if(cur.right != null ) stack.push(cur.right);
        }
        Collections.reverse(res);
        return res;
    }
}
```

### Leetcode 94 Binary Tree Inorder Traversal

递归写法

- 左中右

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode root, List<Integer> res){
        if(root == null){
            return;
        }
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right, res);
    }
}
```

迭代写法

- 循环直到某一个节点没有左节点再往回处理节点，弹出栈顶的第一个，开始处理节点，然后处理当前节点的右节点

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) return res;
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty() ){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            } else{
                cur = stack.peek();
                stack.pop();
                res.add(cur.val);
                cur = cur.right;
            }
        }
        return res;
    }
}
```
