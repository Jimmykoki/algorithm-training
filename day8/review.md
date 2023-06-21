### LeetCode 344 Reverse string

双指针解法

- 避免直接用库函数直接解决问题
- 交换左右指针的值

```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        while(left < right) {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
```

### 剑指 offer 05 Replace space

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

StringBuilder 解法

- create 一个新对象，赋值 str, 复制过程中判断，如是空格则替换，否则直接复制

```java
public static String replaceSpace(String s) {
        if (s == null) {
            return null;
        }
        //选用 StringBuilder 单线程使用，比较快，选不选都行
        StringBuilder sb = new StringBuilder();
        //使用 sb 逐个复制 s ，碰到空格则替换，否则直接复制
        for (int i = 0; i < s.length(); i++) {
            //s.charAt(i) 为 char 类型，为了比较需要将其转为和 " " 相同的字符串类型
            //if (" ".equals(String.valueOf(s.charAt(i)))){}
            if (s.charAt(i) == ' ') {
                sb.append("%20");
            } else {
                sb.append(s.charAt(i));
            }
        }
        return sb.toString();
    }
```

双指针解法

- 其实很多数组填充类的问题，都可以先预先给数组扩容带填充后的大小，然后在从后向前进行操作

```java
public String replaceSpace(String s) {
  if(s == null || s.length() == 0 ) return s;
  char[] oldChars = s.toCharArray();
  int count = 0;
  for(int i = 0; i < oldChars.length; i++){
    if(oldChars[i] == ' '){
      count++;
    }
  }
  if(count == 0) {
    return s;
  }
  char[] newChars = Arrays.copyOf(oldChars, oldChars.length + count * 2);
  int left = oldChars.length - 1;
  int right = newChars.length - 1;
  while(left >= 0){
    if(newChars[left] == ' '){
      newChars[right--] = '0';
      newChars[right--] = '2';
      newChars[right] = '%';
    } else{
      newChars[right] = newChars[left];
    }
    left--;
    right--;
  }
  return new String(newChars);
}
```

### Leetcode 151 Reverse Words in a String

双指针，不用库函数

-

```java
class Solution {
    public String reverseWords(String s) {
        char[] chars = s.toCharArray();
        // remove extra space in string
        chars = removeExtraSpaces(chars);
        // reverse the whole string
        reverse(chars, 0, chars.length - 1);
        // reverse the single word in string
        reverseEachWord(chars);
        return new String(chars);
    }

    public char[] removeExtraSpaces(char[] chars){
        int slow = 0;
        for(int fast = 0; fast < chars.length; fast++){
            if(chars[fast] != ' '){
                if(slow != 0){
                    chars[slow++] = ' ';
                }
                while(fast < chars.length && chars[fast] != ' '){
                    chars[slow++] = chars[fast++];
                }
            }
        }
        char[] newChars = new char[slow];
        System.arraycopy(chars, 0, newChars, 0, slow);
        return newChars;
    }

    public void reverse(char[] chars, int left, int right){
        while(left < right){
            chars[left] ^= chars[right];
            chars[right] ^= chars[left];
            chars[left] ^= chars[right];
            left++;
            right--;
        }
    }

    public void reverseEachWord(char[] chars){
        int start = 0;
        for(int end = 0; end <= chars.length; end++){
            if(end == chars.length || chars[end] == ' '){
                reverse(chars, start, end - 1);
                start = end + 1;
            }
        }
    }
}
```
