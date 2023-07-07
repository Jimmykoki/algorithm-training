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

### Leetcode 654 Maximum Binary Tree

递归解法

- 确定函数参数类型和返回类型

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        // define the terminal situation
        if(nums.length == 1) return new TreeNode(nums[0]);
        int maxValue = 0;
        int index = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] > maxValue){
                 maxValue = nums[i];
                 index = i;
            }
        }
        TreeNode root = new TreeNode(maxValue);
        if(index > 0){
            int[] leftSub = Arrays.copyOfRange(nums, 0, index);
            root.left = constructMaximumBinaryTree(leftSub);
        }
        if(index < nums.length - 1){
            int[] rightSub = Arrays.copyOfRange(nums, index + 1, nums.length);
            root.right = constructMaximumBinaryTree(rightSub);
        }
        return root;
    }
}
```

### Leetcode 617 Merge Two Binary Trees

同时操作两个二叉树

-

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        if(root1 == null && root2 == null) return null;

        root1.val += root2.val;
        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);
        return root1;
    }
}
```

### Leetcode 98 Validate Binary Search Tree

递归解法

- 中序遍历二叉树，得到的数组是一个单调递增的

```java
class Solution {
    private long prev = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if(!isValidBST(root.left)){
            return false;
        }
        if(root.val <= prev){
            return false;
        }
        prev = root.val;
        return isValidBST(root.right);
    }
}
```
