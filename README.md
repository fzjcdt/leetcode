# leetcode

| #    | **Title**                     | Tags   |
| :--- | ------------------------------------------------------------ | ------ |
| 1    | <a href="#1">Two Sum</a>      | 双指针 |
| 2    | <a href="#2">Add Two Numbers</a>      | 模拟，链表 |
| 3    | <a href="#3">Longest Substring Without Repeating Characters</a>      | 模拟，字符串 |
| 4    | <a href="#4">Median of Two Sorted Arrays</a>      | 二分 |
| 5    | <a href="#5">Longest Palindromic Substring</a>      | 动态规划 |
| 6    | <a href="#6">ZigZag Conversion</a>      | 模拟，找规律 |
| 7    | <a href="#7">Reverse Integer</a>      | 模拟 |
| 8    | <a href="#8">String to Integer (atoi)</a>      | 字符串 |
| 9    | <a href="#9">Palindrome Number </a>      | 模拟 |
| 10    | <a href="#10">Regular Expression Matching</a>      | 搜索，动态规划 |







## <a name="1">1.Two Sum</a>
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

注意会有[3, 3]这种情况, 虽然每个元素只能用一次, 但这是不同的元素, 一次性全加到哈希表里会出错

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class P1 {
    class Node {
        int num;
        int index;

        public Node(int num, int index) {
            this.num = num;
            this.index = index;
        }
    }

    public int[] twoSum(int[] nums, int target) {
        List<Node> list = new ArrayList<>();
        for (int i = 0, len = nums.length; i < len; i++) {
            list.add(new Node(nums[i], i));
        }

        list.sort(Comparator.comparingInt(o -> o.num));

        int begin = 0, end = nums.length - 1;
        while (begin < end) {
            if (list.get(begin).num + list.get(end).num < target) {
                begin++;
            } else if (list.get(begin).num + list.get(end).num > target) {
                end--;
            } else {
                break;
            }
        }

        int[] rel = {list.get(begin).index, list.get(end).index};
        Arrays.sort(rel);

        return rel;
    }

    public static void main(String[] args) {
        int[] t = {3, 3};
        P1 s = new P1();
        int[] r = s.twoSum(t, 6);
        System.out.println(r[0] + "  " + r[1]);
    }
}
```
一边读取进来, 一边加入哈希表, [3, 3]这种情况就能正确解决
```

import java.util.HashMap;
import java.util.Map;

public class P_1_new {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        int[] rel = new int[2];
        int temp;
        for (int i = 0, len = nums.length; i < len; i++) {
            temp = target - nums[i];
            if (map.containsKey(temp)) {
                rel[0] = map.get(temp);
                rel[1] = i;
                break;
            } else {
                map.put(nums[i], i);
            }
        }
        return rel;
    }

    public static void main(String[] args) {
        P_1_new p = new P_1_new();
        int[] nums = {3, 3};
        int[] t = p.twoSum(nums, 6);
        System.out.println(t[0]);
        System.out.println(t[1]);
    }
}
```

## <a name="2">2.Add Two Numbers</a>


## <a name="3">3.Longest Substring Without Repeating Characters</a>
## <a name="4">4.Median of Two Sorted Arrays</a>
## <a name="5">5.Longest Palindromic Substring</a>
## <a name="6">6.ZigZag Conversion</a>
## <a name="7">7.Reverse Integer</a>
## <a name="8">8.String to Integer (atoi)</a>
## <a name="9">9.Palindrome Number</a>
## <a name="10">10.Regular Expression Matching</a>

