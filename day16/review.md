### Leetcode 110 Balanced Binary Tree

递归法

- 因为求深度可以从上到下去查 所以需要前序遍历（中左右），而高度只能从下到上去查，所以只能后序遍历（左右中)
- 发现左子树返回-1 或者右子树返回-1，根节点返回-1， 否则返回当前节点的高度

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getDepth(root) == -1 ? false : true;
    }

    private int getDepth(TreeNode root){
        if(root == null) return 0;
        int leftDepth = getDepth(root.left);
        if(leftDepth == -1) return -1;
        int rightDepth = getDepth(root.right);
        if(rightDepth == -1) return -1;

        if(Math.abs(leftDepth - rightDepth) > 1){
            return -1;
        }
        return Math.max(leftDepth, rightDepth) +1;

    }
}
```

### Leetcode 257 Binary Tree Path

递归解法

- （前序遍历）递归函数参数以及返回值: 递归函数的返回值为 void, 用链表 List<Integer> paths 记录每个中点, 结果集用来记录完整的 path, List<String> res
- 确定终止条件： 当前节点的左右节点都为 null 的时候，开始记录
- 单层循环条件： 每次递归后，必须回朔到父节点

![257.二叉树的所有路径](https://code-thinking-1253855093.file.myqcloud.com/pics/20210204151702443.png)

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        if(root == null) return res;
        List<Integer> paths = new ArrayList<>();
        preorder(root, paths, res);
        return res;
    }

    private void preorder(TreeNode root, List<Integer> paths, List<String> res){
        paths.add(root.val);// deal with root node with preorder
        if(root.left == null && root.right == null){
            StringBuilder sb = new StringBuilder(); // connect string
            for(int i = 0; i < paths.size() -1; i++){ // travering the path
                sb.append(paths.get(i)).append("->");
            }
            sb.append(paths.get(paths.size() - 1)); // the last node without ->
            res.add(sb.toString());// collect a path
            return;
        }

        if(root.left != null){
            preorder(root.left, paths, res);
            paths.remove(paths.size() -1);
        }
        if(root.right != null){
            preorder(root.right, paths, res);
            paths.remove(paths.size() - 1);
        }
    }
}
```

### Leetcode 404 Sum of Left Leaves

层序遍历

- BFS 每次 node 进入队列都对左节点进行处理

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) return 0;
        int sum = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            while(len > 0){
                TreeNode cur = queue.poll();
                if(cur.left != null){
                    queue.offer(cur.left);
                    sum += cur.left.val;
                }
                if(cur.right != null){
                    queue.offer(cur.right);
                }
                len--;
            }
        }
        return sum;
    }
}
```

递归解法

- 无法判断当前节点是否有父节点的左节点，所以返回上一层父节点的时候再进行判断

```java
 class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) return 0;
        int sumOfLeft = sumOfLeftLeaves(root.left);
        int sumOfRight = sumOfLeftLeaves(root.right);

        int midValue = 0;
        if(root.left != null && root.left.left == null && root.left.right == null){
            midValue = root.left.val + sumOfLeft + sumOfRight;
        }
        return midValue;
    }
}
```
