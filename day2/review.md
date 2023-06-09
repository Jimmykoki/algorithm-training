# Algorithm-Training

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

### Leetcode 59. Spiral Matrix II

- 循环不变量，
- 明确循环次数，n \* n 的矩阵中需要循环的次数是 n/2, n 为奇数时，需要为内环中的最后一个值手动赋值
- 在对“边”的赋值时，明确赋值的区间范围，比如：左闭右开或左闭右闭，整个循环中必须遵循当前设定

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int startX = 0;
        int startY = 0;
        int offSet = 1;
        int count = 1;
        int i = 0, j = 0;
        while(startX * 2 < n) {
            i = startX;
            j = startY;
            for (; j < n - offSet; j++){
                res[i][j] = count++;
            }
            for(; i < n - offSet; i++) {
                res[i][j] = count++;
            }
            for (; j > startX ; j--){
                res[i][j] = count++;
            }
            for (; i > startY; i--){
                res[i][j] = count++;
            }
            startX++;
            startY++;
            offSet++;
        }
         if (n %2 ==1){
             res[startX -1][startY -1] = count;
         }
         return res;
    }
}
```
