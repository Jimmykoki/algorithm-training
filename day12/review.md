### Leetcode 239 Sliding Window Maximum

- 创建单调队列对象
- 确保队列里者单调递减， refactor push()函数 和 pop()函数， 保持栈顶是最大值
- time complexity O(n) space complexity O(k)

```java
class MyQueue {
    Deque<Integer> deque = new ArrayDeque<>();

     void poll(int val){
         if(!deque.isEmpty() && val == deque.peek()){
             deque.poll();
         }
     }

     void push(int val){
         while(!deque.isEmpty() && val > deque.getLast()){
             deque.removeLast();
         }
         deque.offer(val);
     }
    //  make sure the top of the queue is max
     int peek(){
         return deque.peek();
     }
}

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 1){
            return nums;
        }
        int[] res = new int[nums.length - k + 1];
        int num = 0;
        MyQueue queue = new MyQueue();
        // set the length of slid windwos is k
        for(int i = 0; i < k; i++){
            queue.push(nums[i]);
        }
        res[num++] = queue.peek();
        for(int i = k; i < nums.length; i++){
            // pop a element in queue
            queue.poll(nums[i-k]);
            // sliding windows moves right by one position
            queue.push(nums[i]);
            // get the max
            res[num++] = queue.peek();
        }
        return res;
    }
}
```

### Leetcode 347 Top K Frequent Elements

优先队列

- 为什么不用快排呢， 使用快排要将 map 转换为数组的结构，然后对整个数组进行排序， 而这种场景下，我们其实只需要维护 k 个有序的序列就可以了，所以使用优先级队列是最优的
- 统计元素出现的频率，这一类的问题可以使用 map 来进行统计（key: element, value: times）
- time complexity O(nlogk) space complexity O(n)

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // store the times of appearances of elements and elements
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+ 1);
        }
        //lambda 表达式设置优先级队列从小到大存储 o1 - o2 为从小到大，o1 - o2 反之
        PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);
        int[] res = new int[k];
        for(var s : map.entrySet()){
            int[] temp = new int[2];
            temp[0] = s.getKey();
            temp[1] = s.getValue();
            pq.offer(temp);
            if(pq.size() > k){
                pq.poll(); //poll the min of elements in number of k
            }
        }
        for(int i= k-1; i >=0; i--){
            res[i] = pq.poll()[0];
        }
        return res;
    }
}
```
