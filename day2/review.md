### LeetCode 977.Squares of a Sorted Array

暴力解法

- 新建一个新数组，把原数组遍历一边，将每个数组的平方后放进新数组
- 使用快排对新数组重新排序
- O(n+ nlogN)

双指针解法

- 最大值肯定在数组的两边，最小值在中间
- 利用双指针从两头向中间移动，把每一次比较的最大值放入新数组中，从小到大排列
- 时间复杂度 O(n)， 空间复杂度 O(n)

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int k = nums.length - 1;
        int[] res = new int[k+1];
        int leftIndex = 0, rightIndex = k;
        while (leftIndex <= rightIndex) {
            if (nums[leftIndex] * nums[leftIndex] > nums[rightIndex] * nums[rightIndex]) {
                res[k] = nums[leftIndex] * nums[leftIndex];
                leftIndex++;
                k--;
            } else {
                res[k] = nums[rightIndex] * nums[rightIndex];
                rightIndex--;
                k--;
            }
        }
        return res;
    }
}
```

### Leetcode 209.Minimum Size Subarray Sum

滑动窗口

- for 循环中的 endPointer 控制滑动窗口的终止位置，startPointer 控制滑动窗口的初始位置
- for 循环中的判断条件为 while(sum >= target), 在满足条件的情况下，持续迭代找出 miniLength, 用 if 的话只执行一次
- 时间复杂度为 O(n), 空间复杂度为 O(nums.length);

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int startPointer = 0;
        int miniLength = Integer.MAX_VALUE;
        int sum = 0;
        for (int endPointer = 0; endPointer < nums.length; endPointer++) {
            sum += nums[endPointer];
            while (sum >= target) {
                int subLength = endPointer - startPointer + 1;
                miniLength = Math.min(miniLength, subLength);
                sum = sum - nums[startPointer];
                startPointer++;
            }
        }
        return miniLength < Integer.MAX_VALUE ? miniLength : 0;
    }
}
```
