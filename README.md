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
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

```
class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }
}

// 要用rel保存temp链表的表头
public class P_2 {
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int a, b, sum = l1.val + l2.val;
        int carry = sum / 10;
        ListNode temp = new ListNode(sum % 10);
        ListNode rel = temp;
        l1 = l1.next;
        l2 = l2.next;
        while (l1 != null || l2 != null) {
            a = l1 == null ? 0 : l1.val;
            b = l2 == null ? 0 : l2.val;
            sum = a + b + carry;
            if (sum >= 10) {
                carry = 1;
                sum -= 10;
            } else {
                carry = 0;
            }
            temp.next = new ListNode(sum);
            temp = temp.next;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }

        if (carry == 1) {
            temp.next = new ListNode(1);
        }
        return rel;
    }

    public static void main(String[] args) {
        ListNode l1 = new ListNode(2);
        ListNode temp = l1;
        temp.next = new ListNode(4);
        temp = temp.next;
        temp.next = new ListNode(3);

        ListNode l2 = new ListNode(5);
        temp = l2;
        temp.next = new ListNode(6);

        ListNode l = addTwoNumbers(l1, l2);
        while (l != null) {
            System.out.println(l.val);
            l = l.next;
        }
    }
}
```

基本没变, 只是最后return  head.next, 不用特殊处理第一个位置. 另外while里加上carry != 0, 不用特殊处理最高位的进位
```

class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }
}

public class P_2_new {
    public static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(0);
        ListNode temp = head;
        int carry = 0, a, b, sum;

        while (l1 != null || l2 != null || carry != 0) {
            a = (l1 == null) ? 0 : l1.val;
            b = (l2 == null) ? 0 : l2.val;
            sum = a + b + carry;
            if (sum >= 10) {
                temp.next = new ListNode(sum - 10);
                carry = 1;
            } else {
                temp.next = new ListNode(sum);
                carry = 0;
            }

            temp = temp.next;

            l1 = (l1 == null) ? l1 : l1.next;
            l2 = (l2 == null) ? l2 : l2.next;
        }

        return head.next;
    }

    public static void main(String[] args) {
        ListNode l1 = new ListNode(2);
        ListNode temp = l1;
        temp.next = new ListNode(4);
        temp = temp.next;
        temp.next = new ListNode(3);

        ListNode l2 = new ListNode(5);
        temp = l2;
        temp.next = new ListNode(6);

        ListNode l = addTwoNumbers(l1, l2);
        while (l != null) {
            System.out.println(l.val);
            l = l.next;
        }
    }
}
```

## <a name="3">3.Longest Substring Without Repeating Characters</a>
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

```

import java.util.HashMap;
import java.util.Map;

public class P_3 {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        int maxLen = 0, curLen = 0, begin = 0;
        char temp;
        for (int i = 0, len = s.length(); i < len; i++) {
            temp = s.charAt(i);
            if (map.containsKey(temp)) {
                // 这里map的value直接映射到char的位置能更快, 就不用从头找了
                // 因为字母数不多, 可以用数组代替map
                while (begin < i && temp != s.charAt(begin)) {
                    // 注意remove的是s.charAt(begin), 不是temp
                    map.remove(s.charAt(begin));
                    curLen--;
                    begin++;
                }

                begin++;
                map.put(temp, 1);
            } else {
                map.put(temp, 1);
                curLen++;
                if (curLen > maxLen) {
                    maxLen = curLen;
                }
            }
        }

        return maxLen;
    }

    public static void main(String[] args) {
        P_3 p = new P_3();
        System.out.println(p.lengthOfLongestSubstring("tmmzuxt"));
    }
}
```

tmmsdft   
在第二个m的时候, 发现包含了m, curPos位置更新, 这个时候t也应该失效, 如果从map里删去t的话比较麻烦, 解决的办法是不删, 让curPos = max(curPos, map.get(s.charAt(i)), 这样在curPos前的重复字符就不会影响了. 到最后的t, map.get(s.char(i))不会影响curPos. 后面都会执行map.put()更新重复字符最后面的位置

```
import java.util.HashMap;
import java.util.Map;

public class P_3_new {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        int maxLen = 0, curPos = 0;

        for (int i = 0, len = s.length(); i < len; i++) {
            if (map.containsKey(s.charAt(i))) {
                curPos = Math.max(map.get(s.charAt(i)) + 1, curPos);
            }

            maxLen = Math.max(maxLen, i - curPos + 1);
            map.put(s.charAt(i), i);
        }

        return maxLen;
    }

    public static void main(String[] args) {
        P_3_new p = new P_3_new();
        System.out.println(p.lengthOfLongestSubstring("tmmsdft"));
    }
}

```


## <a name="4">4.Median of Two Sorted Arrays</a>
## <a name="5">5.Longest Palindromic Substring</a>
## <a name="6">6.ZigZag Conversion</a>
## <a name="7">7.Reverse Integer</a>
## <a name="8">8.String to Integer (atoi)</a>
## <a name="9">9.Palindrome Number</a>
## <a name="10">10.Regular Expression Matching</a>

