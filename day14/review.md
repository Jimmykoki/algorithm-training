### Leetcode 102 Binary Tree Level Order Traversal

BFS- Iteration - queue

- the size of queue demonstrate the numbe of elements in each level

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            // store elements in each level
            List<Integer> levelList = new ArrayList<Integer>();
            // record the size of each level
            int len = queue.size();
            while(len > 0){
                TreeNode cur = queue.poll();
                levelList.add(cur.val);
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                len--;
            }
            res.add(levelList);
        }
        return res;
    }
}
```

### Leetcode 107 Binary Tree Level Order Traversal II

- 同样利用 BFS 的解法
- 将结果集合反转即可，或者利用 LinkedList 每层 list 都加在结果集表头

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        //List<List<Integer>> res = new LinkedList<List<Integer>>();
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            List<Integer> levelList = new ArrayList<>();
            int len = queue.size();
            while(len > 0){
                TreeNode cur = queue.poll();
                levelList.add(cur.val);
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                len--;
            }
            //res.addFirst(levelList); add elements in the head of list;
            res.add(levelList);
        }

        List<List<Integer>> reverseRes = new ArrayList<List<Integer>>();
        for(int i = res.size() -1; i >= 0; i--){
            reverseRes.add(res.get(i));
        }
        return reverseRes;
    }
}
```

### Leetcode 199 Binary Tree Right Side View

- BFS, 获取每一层的最后一个 element

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            // record the size of each level
            int len = queue.size();
            while(len > 0){
                TreeNode cur = queue.poll();
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                if(len == 1){
                    res.add(cur.val);
                }
                len--;
            }
        }
        return res;
    }
}
```

### Leetcoed 637 Average of Levels in Binary Tree

层序遍历 -BFS

```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        if(res == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            double sum = 0;
            for(int i = 0; i < len; i++){
                TreeNode cur = queue.poll();
                sum += cur.val;
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
            res.add( sum / len);
        }
        return res;
    }
}
```

### Leetcode 429 N-ary Tree Level Order Traversal

BFS

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null) return res;
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            List<Integer> levelList = new ArrayList<>();
            int len = queue.size();
            while(len > 0){
                Node cur = queue.poll();
                levelList.add(cur.val);
                if(cur.children != null){
                    for(Node n : cur.children){
                        queue.offer(n);
                    }
                }
                len--;
            }
            res.add(levelList);
        }
        return res;
    }
}
```

### Leetcode 515 Find Largest Value in Each Tree Row

BFS

- 找到当前层的最大值

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            int max = Integer.MIN_VALUE;
            while(len > 0){
                TreeNode cur = queue.poll();
                max = Math.max(cur.val, max);
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                len--;
            }
            res.add(max);
        }
        return res;
    }
}
```

### Leetcode 116 Populating Next Right Pointers in Each Node

```java
class Solution {
    public Node connect(Node root) {
        if(root == null) return root;
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            while(len > 0){
                Node cur = queue.poll();
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                if(len == 1){
                    cur.next = null;
                } else{
                    cur.next = queue.peek();
                }
                len--;
            }
        }
        return root;
    }
}
```

### Leetcode 104 Maximum Depth of Binary Tree

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        int depth = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            depth++;
            while(len > 0){
                TreeNode cur = queue.poll();
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                len--;
            }
        }
        return depth;
    }
}
```

### Leetcode 111 Minimum Depth of Binary Tree

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        int depth = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            depth++;
            while(len > 0){
                TreeNode cur = queue.poll();
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
                if(cur.left == null && cur.right == null) return depth;
                len--;
            }
        }
        return depth;
    }
}
```

### Leetcode 226 Invert Binary Tree

- BFS 层序遍历，DFS 递归遍历

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return root;

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

### Leetcode 101 Symmetric Tree

递归解法

- 保证二叉树的是对称的，左子树的左节点等于右子树的右节点(外侧), 左子树的右节点等于右子树的左节点
- 递归三部曲：1. 确定递归函数的参数和返回值 2. 确定终止条件 3. 确定单层循环逻辑

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        return compare(root.left, root.right);
    }

    public boolean compare(TreeNode left, TreeNode right){
        if(left == null && right != null) return false;
        else if(left != null && right == null) return false;
        else if(left == null && right == null) return true;
        else if(left.val != right.val) return false;

        boolean outside = compare(left.left, right.right);
        boolean inside = compare(left.right, right.left);
        boolean isSame = outside && inside;
        return isSame;
    }
}
```
