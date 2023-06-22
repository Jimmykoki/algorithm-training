### Leetcode 28 Find the index of the first occurrence in a string

- prefix tabel: 前缀表是用来回退的，它记录了模式串与主串(文本串)不匹配的时候，模式串应该从哪里开始重新匹配
- initiate next[] used to store the prefix table
- KMP 算法的匹配过程中，当发现不匹配的字符时，能快速找到一个位置，跳过一些肯定不会匹配的位置，从而提高字符串匹配的效率
- time complexity O(n+m)

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        //initiate next array used to store prefix table
        int[] next = new int[needle.length()];
        getNext(next, needle);

        int j = 0;
        for(int i = 0; i < haystack.length(); i++){
            while(j > 0 && needle.charAt(j) != haystack.charAt(i)){
                j = next[j-1];
            }
            if(needle.charAt(j) == haystack.charAt(i)){
                j++;
            }
            if(j == needle.length()){
                return i - needle.length() + 1;
            }
        }
        return -1;
    }

    public void getNext(int[] next, String str){
        int j = 0;
        // lenght of string is 1 without prefix and suffix
        next[0] = 0;
        // i is used to record the last char of suffix, starting from index of 1
        for(int i = 1; i < str.length(); i++){
            while(j > 0 && str.charAt(j) != str.charAt(i)){
                j = next[j-1];
            }
            if(str.charAt(j) == str.charAt(i)){
                j++;
            }
            next[i] = j;
        }
    }
}
```

### Leetcode 459 Repeated Substring Pattern

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

KMP 解法

- 计算目标 str 的前缀表
- 数组长度减去最长相同前后缀的长度相当于是第一个周期的长度，也就是一个周期的长度，如果这个周期可以被整除，就说明整个数组就是这个周期的循环，存在重复子串的同时

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        if(s == null || s.length() == 0) return false;
        int len = s.length();
        char[] charStr = s.toCharArray();
        int[] next = new int[len];
        int j = 0;
        next[0] = 0;
        for(int i = 1; i < len; i++){
            while(j > 0 && charStr[j] != charStr[i]) j = next[j-1];
            if(charStr[j] == charStr[i]) j++;
            next[i] = j;
        }
        int n = len - next[len - 1];
        if(len % n == 0 && next[len - 1] != 0){
            return true;
        }
        return false;
    }
}
```
