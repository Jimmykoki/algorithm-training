### Leetcode 104 Maximum Depth of Binary Tree

- 二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从 0 开始还是从 1 开始）
- 二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从 0 开始还是从 1 开始）

迭代解法

- 层序遍历

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

递归解法

- 通过后序遍历求根节点高度来求的二叉树最大深度

```java
class Solution{
    public int maxDepth(TreeNode root){
        return getDepth(root);
    }

    public int getDepth(TreeNode root){
        if(root == null) return 0;

        int leftdepth = getDepth(root.left);
        int rightdepth = getDepth(root.right);
        int depth = 1 + Math.max(leftdepth, rightdepth);
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

### Leetcode 222 Count Complete Tree Nodes

递归解法

- 确定递归函数的参数和返回值：参数就是传入树的根节点，返回就返回以该节点为根节点二叉树的节点数量，所以返回值为 int 类型
- 确定终止条件：如果为空节点的话，就返回 0，表示节点数为 0。
- 确定单层递归的逻辑：先求它的左子树的节点数量，再求右子树的节点数量，最后取总和再加一 （加 1 是因为算上当前中间节点）就是目前节点为根节点的节点数量。
- time complexity O(n), space complexity O(logn)

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null) return 0;

        int leftcount = countNodes(root.left); // left
        int rightcount = countNodes(root.right); // right
        int counts = leftcount + rightcount + 1; // mid
        return counts;
    }
}
```

完全二叉树特性写法

- 如果子节点是满二叉树可以直接用公式 2^ depth - 1
- 分别递归左孩子，和右孩子，递归到某一深度一定会有左孩子或者右孩子为满二叉树
- 在完全二叉树中，如果递归向左遍历的深度等于递归向右遍历的深度，那说明就是满二叉树

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null) return 0;
        TreeNode left = root.left;
        TreeNode right = root.right;
        int leftdepth = 0, rightdepth = 0;
        while(left != null){
            left = left.left;
            leftdepth++;
        }
        while(right != null){
            right = right.right;
            rightdepth++;
        }
        if(leftdepth == rightdepth){
            return ( 2 << leftdepth) - 1;
        }
        int leftCounts = countNodes(root.left);
        int rightCounts = countNodes(root.right);
        int res = leftCounts + rightCounts + 1;
        return res;
    }
}
```

### Leetcode

- 先构建一个无向图
- 从一点出发求这个点能到的所有点， 再求和

```java
class Solution{
    public int reachableNodes(int n, int[][] edges, int[] restricted){
        boolean[] isVisited = new boolean[n];
        Set<Integer> set = new HashSet<>();
        for(int node : restricted) set.add(node);
        List<ArrayList<Integer>> graph = buildGraph(edges, n);
        return doDFS(0, graph, set, isVisited);
    }

    private int doDFS(int source, List<ArrayList<Integer>> graph, Set<Integer> set, boolean[] isVisited){
        isVisited[source] = true;
        int reachableNodes = 1;
        for(Integer adjacentNode : graph.get(source)){
            if(!isVisited[adjacentNode] && !set.contains(adjacentNode)){
                reachableNodes += doDFS(adjacentNode, graph, set, isVisited);
            }
        }
        return reachableNodes;
    }

    private List<ArrayList<Integer>> buildGraph(int[][] edges, int n){
        List<ArrayList<Integer>> graph = new ArrayList<>();
        for(int i = 0; i < n; i++){
            graph.add(new ArrayList<Integer>());
        }
        for(int[] edge : edges){
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        return graph;
    }
}

```
