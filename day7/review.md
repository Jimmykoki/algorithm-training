### Leetcode 454 4Sum II

- 哈希法，类似于 two sum, 遍历所有 a+b 出现的值作 Key, 值出现的次数为 value（多种组合可能会出现一样的 sum）
- 查询目标值(0 - (num3+ num4)是否在集合中, res 用来记录次数
- map.getOrDefault(), 第一个参数值为 key, 如果不存在返回 default 值(第二个参数 0)
- 时间复杂度为 O(n^2), 空间复杂度: O(n^2)，最坏情况下 A 和 B 的值各不相同，相加产生的数字个数为 n^2

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;
        HashMap<Integer, Integer> record = new HashMap<>();
       // caculate the sum of two elements in arraies, and record the number of times
        for(int num1: nums1){
            for (int num2: nums2){
                int sum = num1+ num2;
                record.put(sum, record.getOrDefault(sum, 0)+ 1);
            }
        }
        for (int num3 : nums3){
            for(int num4 : nums4){
                res += record.getOrDefault(0 - num3 - num4, 0);
            }
        }
        return res;
    }
}
```

### Leetcode 383. Ransom Note

- 因为题目所只有小写字母，那可以采用空间换取时间的哈希策略， 用一个长度为 26 的数组还记录 magazine 里字母出现的次数，然后再用 ransomNote 去验证这个数组是否包含了 ransomNote 所需要的所有字母
- 使用 map 的空间消耗要比数组大一些的，因为 map 要维护红黑树或者哈希表，而且还要做哈希函数，是费时的！数据量大的话就能体现出来差别了。 所以数组更加简单直接有效!

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        char[] ransomChar = ransomNote.toCharArray();
        char[] magazineChar = magazine.toCharArray();
        for(char i : magazineChar){
            map.put(i, map.getOrDefault(i, 0)+1);
        }
        for(char j : ransomChar){
            if(!map.containsKey(j)){
                return false;
            } else {
                map.put(j, map.get(j) - 1);
            }
        }
        for(Integer k : map.values()){
            if(k < 0){
                return false;
            }
        }
        return true;
    }
}
```

### Leetcode15 3Sum

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        for(int i = 0; i< nums.length; i++){
            if(nums[i] > 0){
                break;
            }
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            int left = i + 1;
            int right = nums.length - 1;
            while(left < right){
                int sum = nums[i] + nums[left] + nums[right];
                if(sum > 0){
                    right--;
                } else if(sum < 0) {
                    left++;
                } else {
                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while(left < right && nums[left] == nums[left+1]) left++;
                    while(left < right && nums[right] == nums[right-1]) right--;
                    left++;
                    right--;
                }
            }
        }
        return res;
    }
}
```

### Leetcode18 4Sum

-

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        for(int k = 0; k < nums.length; k++){
            if(nums[k] > target && nums[k] >= 0) return res;
            if(k > 0 && nums[k] == nums[k-1]) continue;
            for(int i = k+1; i < nums.length; i++){

                if (i > k + 1 && nums[i] == nums[i-1]) continue;
                int left = i + 1;
                int right = nums.length - 1;
                while(left < right){
                    int sum = nums[k] + nums[i] + nums[left] + nums[right];
                    if(sum > target){
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        res.add(Arrays.asList(nums[k], nums[i], nums[left], nums[right]));
                        while(left < right && nums[left] == nums[left+1]) left++;
                        while(left < right && nums[right] == nums[right-1]) right--;
                        left++;
                        right--;
                    }
                }
            }
        }
        return res;
    }
}
```
