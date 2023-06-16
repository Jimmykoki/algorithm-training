### Leetcode 242 Valid Anagram

暴力解法

- 判断两字符串长度是否相等，否则返回 null
- 将字符串存入 char[] 数组，排序后，字符数组相等，则为 anagram
- 时间复杂度 O(nlogN)

```java
public class Solution {
    public boolean isAnagram(String s, String t) {
        // 如果两个字符串长度不等，它们不可能是字母异位词
        if (s.length() != t.length()) {
            return false;
        }

        // 将两个字符串转换为字符数组
        char[] sChars = s.toCharArray();
        char[] tChars = t.toCharArray();

        // 对字符数组进行排序
        Arrays.sort(sChars);
        Arrays.sort(tChars);

        // 如果排序后的字符数组相等，那么它们就是字母异位词
        return Arrays.equals(sChars, tChars);
    }
}
```

数组解法

-

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] result = new int[26];
        for (int i =0; i< s.length(); i++){
            result[s.charAt(i) - 'a']++;
        }
        for (int i=0; i < t.length(); i++){
            result[t.charAt(i) - 'a']--;
        }
        for (int count: result){
            if (count != 0){
                return false;
            }
        }
        return true;
    }
}
```

### LeetCode 349 Intersection of Two Arrays

- 给你一个数判断是否在集合出现过, 考虑哈希法（set, array, map）
- 数组用于处理数据量比较小的集合。使用 set 对于空间占用比较大，且速度比数组慢

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> res = new HashSet<>();
        int[] hash = new int[1005];
        for (int num: nums1){
            hash[num] = 1;
        }
        for (int num: nums2){
            if(hash[num] == 1){
                res.add(num);
            }
        }
        return res.stream().mapToInt(x -> x).toArray();
    }
}
```

### Leetcode 202. Happy Number

- it **loops endlessly in a cycle** which does not include 1, 在不等于 1 的时候会进入无限循环
- sum 有可能会重复出现， => 已经进入无限循环中
- so 结束循环的条件 当 n=1 或者 sum 已经重复出现了

```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> record = new HashSet<Integer>();
        while(n != 1 && !record.contains(n)){
            record.add(n);
            int res = 0;
          //取到每一位数
            while(n > 0){
                int temp  = n % 10;
                res += temp * temp;
                n = n / 10;
            }
            n = res;
        }
        return n == 1 ? true : false;
    }
}
```

### Leetcode 1. Two Sum

- 遍历的同时，同时需要查找集合中有没有对应的数
- 为什么需要用 map，是因为我们不仅需要找到相对应的值，而且还需要知道它的下标值
- map 的 Key 存目标值，value 来存下标值， 通过 containsKey(Key)方法找到 value

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        if(nums == null || nums.length == 0) return res;
        HashMap<Integer, Integer> record = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            int temp = target - nums[i];
            if(record.containsKey(temp)){
                res[1] = i;
                res[0] = record.get(temp);
                break;
            }
            record.put(nums[i], i);
        }
        return res;
    }
}
```
