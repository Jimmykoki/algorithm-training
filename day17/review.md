### Leetcode 513 Find Bottom Left Tree Value

层序遍历

- BFS,每一层中最先弹出的 Node 就是这一层中最左边的一个

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        if(root == null) return 0;
        int res = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            for(int i = 0; i < len; i++){
                TreeNode cur = queue.poll();
                if(i == 0) res = cur.val;
                if(cur.left != null) queue.offer(cur.left);
                if(cur.right != null) queue.offer(cur.right);
            }
        }
        return res;
    }
}
```

### Leetcode 112 Path Sum

递归写法

- 确定递归函数的参数和返回类型：需要二叉树的根节点，还需要一个计数器，这个计数器用来计算二叉树的一条边之和是否正好是目标和，计数器为 int 型；搜索其中一条符合条件的路径，那么递归一定需要返回值，因为遇到符合条件的路径了就要及时返回。（本题的情况

- 确定终止条件：不要去累加然后判断是否等于目标和，那么代码比较麻烦，可以用递减，让计数器 count 初始为目标和，然后每次减去遍历路径节点上的数值

- 确定单层递归的逻辑：因为终止条件是判断叶子节点，所以递归的过程中就不要让空节点进入递归了。

  递归函数是有返回值的，如果递归函数返回 true，说明找到了合适的路径，应该立刻返回。

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        return traversal(root, targetSum - root.val);
    }

    private boolean traversal(TreeNode cur, int count){
        if(cur.left == null && cur.right == null && count == 0) return true;
        if(cur.left == null && cur.right == null) return false;

        if(cur.left != null){
            count -= cur.left.val;
            if(traversal(cur.left, count)) return true;
            count += cur.left.val;
        }
        if(cur.right != null){
            count -= cur.right.val;
            if(traversal(cur.right, count)) return true;
            count += cur.right.val;
        }
        return false;
    }
}
```

### Leetcode 113 Path Sum II

Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_

递归写法

- 确定递归函数的参数和返回类型: 需要搜索整棵二叉树且不用处理递归返回值，递归函数就不要返回值, 参数包括记录结果集和当前 path 的集合
- 确定终止条件: 与 112 相似，当前节点为叶子节点，且 count 的值为 0，则将当前的 path 集合记录在结果集中
- 确定单层递归的逻辑：

![113.路径总和ii](https://code-thinking-1253855093.file.myqcloud.com/pics/20210203160922745.png)

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null) return res;
        List<Integer> path = new ArrayList<>();
        path.add(root.val);
        preorder(root, targetSum - root.val, res, path);
        return res;
    }

    private void preorder(TreeNode root, int count, List<List<Integer>> res, List<Integer> path){
        if(root.left == null && root.right == null && count == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        if(root.left == null && root.right == null){
            return;
        }

        if(root.left != null){
            path.add(root.left.val);
            preorder(root.left, count - root.left.val, res, path);
            path.remove(path.size() - 1);
        }

        if(root.right != null){
            path.add(root.right.val);
            preorder(root.right, count - root.right.val, res, path);
            path.remove(path.size() - 1);
        }

        return;
    }
}
```

### Leetcode 106 Construct Binary Tree from Inorder and Postorder Traversal

递归解法

- 后序数组为 0 == 空节点， 后序数组最后一个元素为节点元素，寻找中序数组位置作为切割点， 切中序数组，切后序数组，递归处理左区间右区间
-

```java
class Solution {
    Map<Integer, Integer> map;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        map = new HashMap<>();
        for(int i = 0; i < inorder.length; i++){
            map.put(inorder[i], i);
        }
        return buildHelper(inorder, 0, inorder.length, postorder, 0, postorder.length);
    }

    private TreeNode buildHelper(int[] inorder, int inStart, int inEnd, int[] postorder, int poStart, int poEnd){
        if(poStart == poEnd) return null;
        int rootValue = postorder[poEnd - 1];
        TreeNode root = new TreeNode(rootValue);
        int midIndex = map.get(root.val);

        int lenOfLeft = midIndex - inStart;
        root.left = buildHelper(inorder, inStart, midIndex, postorder, poStart, poStart + lenOfLeft);
        root.right = buildHelper(inorder, midIndex + 1, inEnd, postorder, poStart + lenOfLeft, poEnd - 1);
        return root;
    }
}
```
