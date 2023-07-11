### Leetcode 669 Trim a Binary Search Tree

递归解法

- 删除当前节点的操作，保证二叉树的所有值在[low, high]的范围内

```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if(root == null) return null;
        if(root.val < low){
            return trimBST(root.right, low, high);
        }else if(root.val > high){
            return trimBST(root.left, low, high);
        }

        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```

### Leetcode 108 Convert Sorted Array to Binary Search Tree

递归解法

- 二分法
- 前序遍历

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        int left = 0;
        int right = nums.length;
        return buildTree(left, right, nums);
    }

    private TreeNode buildTree(int left, int right, int[] nums){
        if(left >= right) return null;

        int mid = left + ((right - left) >> 1);
        TreeNode root = new TreeNode(nums[mid]);

        root.left = buildTree(left, mid, nums);
        root.right = buildTree(mid + 1, right, nums);
        return root;
    }
}
```

### Leetcode 538 Convert BST to Greater Tree

递归解法

- 中序遍历二叉树获得单调递增的数组，相反则获得单调递减数组即右中左的遍历方式

```java
class Solution {
    int pre = 0;
    public TreeNode convertBST(TreeNode root) {
        buildTree(root);
        return root;
    }
    private void buildTree(TreeNode cur){
        if(cur == null) return;

        buildTree(cur.right);
        cur.val += pre;
        pre = cur.val;
        buildTree(cur.left);
    }
}
```
