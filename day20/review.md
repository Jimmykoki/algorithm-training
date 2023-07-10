### Leetcode 235 Lowest Common Ancestor of a Binary Search Tree

递归解法

- 根据 BST 的特性，如果 p, q 分别在当前节点的两侧，则该节点为他们的最近公共祖先(因为 p, q 都是 unique 的)在往左或者往右遍历都只能找到他们一个节点

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root.val > p.val && root.val > q.val){
            return lowestCommonAncestor(root.left, p, q);
        } else if(root.val < p.val && root.val < q.val){
            return lowestCommonAncestor(root.right, p, q);
        } else {
            return root;
        }
    }
}
```

### Leetcode 701 Insert into a Binary Search Tree

递归解法

- 假如不用调整二叉树的结构， 添加进去的新节点都会是叶子节点

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        return insert(root, val);
    }

    private TreeNode insert(TreeNode root, int val){
        if(root == null){
            TreeNode node = new TreeNode(val);
            return node;
        }

        if(root.val > val){
            root.left = insert(root.left, val);
        } else {
            root.right = insert(root.right, val);
        }
        return root;
    }
}
```

### Leetcode 450 Delete Node in a BST

递归解法

- 考虑多种出现的情况, 找到当前节点为目标节点时候 需要判断以下情况

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null) return null;
        if(root.val == key){
            // current node is a leaf node
            if(root.left == null && root.right == null){
                return null;
            } else if(root.left == null && root.right != null){ // left child is null
                return root.right;
            } else if(root.left != null && root.right == null){ // right child is null
                return root.left;
            } else{ // both of that is not null, transit left child of current node to the left node of the smallest node in right child of current node
                TreeNode cur = root.right;
                while(cur.left != null) cur = cur.left;
                cur.left = root.left;
                return root.right;
            }
        }
        if(root.val > key) root.left = deleteNode(root.left, key);
        if(root.val < key) root.right = deleteNode(root.right, key);
        return root;
    }
}
```
