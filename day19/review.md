### Leetcode 530 Minimum Absolute Difference in BST

递归解法

- 使用双指针 pre, cur,在中序遍历的过程一直比较他们的差值，如果有更小的差值则更新 res 结果
- 在 BST 中，通过中序遍历 可以获得一个单调递增的数组

```java
class Solution {
        int res = Integer.MAX_VALUE;
        TreeNode pre = null;
    public int getMinimumDifference(TreeNode root) {
        inorder(root);
        return res;
    }
    private void inorder(TreeNode cur){
        if(cur == null) return;
        inorder(cur.left);
        if(pre != null){
            res = Math.min(res, cur.val - pre.val);
        }
        pre = cur;
        inorder(cur.right);
    }
}
```

### Leetcode 501 Find Mode in Binary Search Tree

暴力解法

- 递归遍历将 node 的值和出现次数存进 map 中
- 排序，获取频率最高的 node

```java
class Solution {
	public int[] findMode(TreeNode root) {
		Map<Integer, Integer> map = new HashMap<>();
		List<Integer> list = new ArrayList<>();
		if (root == null) return list.stream().mapToInt(Integer::intValue).toArray();
		// 获得频率 Map
		searchBST(root, map);
		List<Map.Entry<Integer, Integer>> mapList = map.entrySet().stream()
				.sorted((c1, c2) -> c2.getValue().compareTo(c1.getValue()))
				.collect(Collectors.toList());
		list.add(mapList.get(0).getKey());
		// 把频率最高的加入 list
		for (int i = 1; i < mapList.size(); i++) {
			if (mapList.get(i).getValue() == mapList.get(i - 1).getValue()) {
				list.add(mapList.get(i).getKey());
			} else {
				break;
			}
		}
		return list.stream().mapToInt(Integer::intValue).toArray();
	}

	void searchBST(TreeNode curr, Map<Integer, Integer> map) {
		if (curr == null) return;
		map.put(curr.val, map.getOrDefault(curr.val, 0) + 1);
		searchBST(curr.left, map);
		searchBST(curr.right, map);
	}
}
```

双指针解法

```java
class Solution {
    TreeNode pre = null;
    int count = 0;
    int maxCount = 0;
    List<Integer> res = new ArrayList<>();
    public int[] findMode(TreeNode root) {
        inorder(root);
        int[] resArray = new int[res.size()];
        for(int i = 0; i < res.size(); i++){
            resArray[i] = res.get(i);
        }
        return resArray;
    }

    private void inorder(TreeNode cur){
        if(cur == null) return;
        inorder(cur.left);
        if(pre == null || pre.val != cur.val){
            count = 1;
        } else{
            count++;
        }
        if(count == maxCount){
            res.add(cur.val);
        } else if(count > maxCount){
            res.clear();
            res.add(cur.val);
            maxCount = count;
        }
        pre = cur;
        inorder(cur.right);
    }
}
```

### Leetcode 236 Lowest Common Ancestor of a Binary Tree

递归解法

- 确定递归函数返回值以及参数: 返回最近公共节点，可以利用上题目中返回值是 TreeNode ，那么如果遇到 p 或者 q，就把 q 或者 p 返回，返回值不为空，就说明找到了 q 或者 p
- 确定终止条件:遇到空的话，因为树都是空了，所以返回空

- 本题函数有返回值，是因为回溯的过程需要递归函数的返回值做判断，但本题我们依然要遍历树的所有节点。

**在递归函数有返回值的情况下：如果要搜索一条边，递归函数返回值不为空的时候，立刻返回，如果搜索整个树，直接用一个变量 left、right 接住返回值，这个 left、right 后序还有逻辑处理的需要，也就是后序遍历中处理中间节点的逻辑（也是回溯）**

我们在[二叉树：递归函数究竟什么时候需要返回值，什么时候不要返回值？ (opens new window)](https://programmercarl.com/0112.路径总和.html)中说了 递归函数有返回值就是要遍历某一条边，但有返回值也要看如何处理返回值！

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return null;
        if(root == p || root == q) return root;

        TreeNode left = lowestCommonAncestor(root.left, p, q); // leftNode
        TreeNode right = lowestCommonAncestor(root.right, p , q); // rightNode

        if(left != null && right != null) return root;
        if(left == null && right != null) return right;
        if(left != null && right == null) return left;
        else return null;
    }
}
```
