# Algorithm-Training

### Leetcode 704 Binary Search

左闭右开区间 ( while (left < right) )

- 在定义 right 区间的时候, 注意 right = nums.length
- 定义 mid 的值时候可以使用位运算, 防止数组越界（int mid = left + ((right - left) >> 1);）
- 当 mid 值大于 target 时，说明 target 在左区间，由于右开原则，下次循环的 right = mid

左闭右闭区间 ( while (left <= right) )

- right 和 left 的值必须是数据的有效区间，right = 0 & left = nums.length - 1
- 当 mid 值大于 target 时，说明 target 在左区间， 右闭原则 right = middle - 1
- 当 mid 值小于 target 时，说明 target 在右区间， 左闭原则 left = middle + 1

```java
class Solution {
    public int search(int[] nums, int target) {
        if( target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

### Leetcode 27 Remove Element

暴力解法

- 数组中删除元素用后边一个的值覆盖当前值，且遍历的 i 需要往前移动一位，数组长度自动减一
- 由于双循环时间复杂度是 O(n^2), 没有开拓新的空间空间复杂度 O(1)

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int size = nums.length;
        for (int i = 0; i < size; i++){
            if (nums[i] == val) {
                for (int j = i + 1; j < nums.length; j++){
                    nums[j-1] = nums[j];
                }
                i--;
                size--;
            }
        }
        return size;
    }
}
```

快慢指针

- 快指针负责选择元素，慢指针负责新数组的下标值.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.length; fastIndex++){
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
}
```
