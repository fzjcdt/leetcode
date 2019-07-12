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
| 11    | <a href="#11">Container With Most Water</a>      | 双指针，贪心 |
| 12    | <a href="#12">Integer to Roman</a>      | 模拟 |
| 13    | <a href="#13">Roman to Integer </a>      | 模拟 |
| 14    | <a href="#14">Longest Common Prefix    	</a>      | 模拟，字符串 |
| 15    | <a href="#15">3Sum    	</a>      | 模拟 |
| 16    | <a href="#16">3Sum Closest    	</a>      | 双指针 |
| 17    | <a href="#17">Letter Combinations of a Phone Number    	</a>      | 搜索 |
| 18    | <a href="#18">4Sum    	</a>      | 双指针，搜索 |
| 19    | <a href="#19">Remove Nth Node From End of List    	</a>      | 双指针 |
| 20    | <a href="#20">Valid Parentheses    	</a>      | 栈 |
| 21    | <a href="#21">Merge Two Sorted Lists    	</a>      | 链表 |
| 22    | <a href="#22">Generate Parentheses    	</a>      | 搜索 |
| 23    | <a href="#23">Merge k Sorted Lists    	</a>      | 链表，堆 |
| 24    | <a href="#24">Swap Nodes in Pairs    	</a>      | 链表，递归 |
| 25    | <a href="#25">Reverse Nodes in k-Group    	</a>      | 链表 |
| 26    | <a href="#26">Remove Duplicates from Sorted Array    	</a>      | 双指针 |
| 27    | <a href="#27">Remove Element    	</a>      | 双指针 |
| 28    | <a href="#28">Implement strStr()    	</a>      | KMP |
| 29    | <a href="#29">Divide Two Integers    	</a>      | 递归 |
| 30    | <a href="#30">Substring with Concatenation of All Words    	</a>      | 字符串 |






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

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

Example 1:
nums1 = [1, 3]
nums2 = [2]
The median is 2.0


Example 2:
nums1 = [1, 2]
nums2 = [3, 4]
The median is (2 + 3)/2 = 2.5

```

public class P4 {
    //可以寻找两个数组中任意第k大的数
    private int getMedian(int[] nums1, int nums1Left, int nums1Right,
                          int[] nums2, int nums2Left, int nums2Right, int k) {

        if (nums1Left > nums1Right) {
            return nums2[nums2Left + k - 1];
        } else if (nums2Left > nums2Right) {
            return nums1[nums1Left + k - 1];
        } else if (k == 1) {
            return Math.min(nums1[nums1Left], nums2[nums2Left]);
        }

        int nums1Mid = (nums1Left + nums1Right) / 2;
        int nums2Mid = (nums2Left + nums2Right) / 2;

        if (nums1[nums1Mid] <= nums2[nums2Mid]) {
            //把mid算成左边的元素
            if ((nums1Mid - nums1Left + 1) + (nums2Mid - nums2Left + 1) > k) {
                //左边已经大于k个了，则去掉更大的右边的。
                //nums2Mid不可能是结果，因为大于k个，nums2Mid左边最大，一定是大于k，所以nums2Mid - 1
                return getMedian(nums1, nums1Left, nums1Right, nums2, nums2Left, nums2Mid - 1, k);
            } else {
                //左边还不够或刚好k个，则去掉更小的左边的。
                //nums1Mid不可能是结果，刚好是k个的时候，nums2Mid大于nums1Mid，答案不会是nums1Mid，所以nums1Mid + 1
                return getMedian(nums1, nums1Mid + 1, nums1Right, nums2, nums2Left, nums2Right, k - (nums1Mid - nums1Left + 1));
            }
        } else {
            if ((nums1Mid - nums1Left + 1) + (nums2Mid - nums2Left + 1) > k) {
                return getMedian(nums1, nums1Left, nums1Mid - 1, nums2, nums2Left, nums2Right, k);
            } else {
                return getMedian(nums1, nums1Left, nums1Right, nums2, nums2Mid + 1, nums2Right, k - (nums2Mid - nums2Left + 1));
            }
        }
    }

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;

        if ((m + n) % 2 == 0) {
            return (getMedian(nums1, 0, m - 1, nums2, 0, n - 1, (m + n) / 2)
                    + getMedian(nums1, 0, m - 1, nums2, 0, n - 1, (m + n) / 2 + 1)) / 2.0;
        } else {
            return getMedian(nums1, 0, m - 1, nums2, 0, n - 1, (m + n) / 2 + 1);
        }
    }

    public static void main(String[] args) {
        P4 p = new P4();
        int[] nums1 = {3, 4};
        int[] nums2 = {1, 2};
        System.out.println(p.findMedianSortedArrays(nums1, nums2));
    }
}
```


## <a name="5">5.Longest Palindromic Substring</a>
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.


Example:
Input: "cbbd"
Output: "bb"

```
public class P5 {
    public String longestPalindrome(String s) {
        int left = 0, right = 0;
        int len = s.length();
        boolean[][] isPalindrome = new boolean[len][len];
        /*
         * 状态转移方程:
         * f[i, j] = true  if i == j
         * f[i, j] = s[i] == s[j]  if i + 1 == j
         * f[i, j] = s[i] == s[j] && f[i + 1, j - 1]  otherwise
         *
         * 一开始写的是
         * i = 0 to len
         *   j = i to len
         *     ...
         * 然后比如0到4，要用1到3，但1到3还没有计算，所以改用下面的间隔，间隔0,1,2。。
         * 实际只要改成P5_new那样
         * i = 0 to len
         *   j = 0 to i
         * 就好
         */

        for (int interval = 0; interval < len; interval++) {
            for (int i = 0; i < len - interval; i++) {
                if (interval == 0) {
                    isPalindrome[i][i] = true;
                } else if (interval == 1) {
                    isPalindrome[i][i + 1] = s.charAt(i) == s.charAt(i + 1);
                } else {
                    isPalindrome[i][i + interval] = s.charAt(i) == s.charAt(i + interval) && isPalindrome[i + 1][i + interval - 1];
                }

                if (isPalindrome[i][i + interval]) {
                    left = i;
                    right = i + interval;
                }
            }
        }

        return s.substring(left, right + 1);
    }

    public static void main(String[] args) {
        P5 p = new P5();
        System.out.println(p.longestPalindrome("babad"));
    }
}
```

```

public class P5_new {
    public String longestPalindrome(String s) {
        int left = 0, right = 0, maxLen = -1;
        int len = s.length();
        boolean[][] isPalindrome = new boolean[len][len];

        for (int i = 0; i < len; i++) {
            for (int j = 0; j <= i; j++) {
                isPalindrome[j][i] = s.charAt(j) == s.charAt(i) && (i - j < 2 || isPalindrome[j + 1][i - 1]);

                if (isPalindrome[j][i] && i - j > maxLen) {
                    left = j;
                    right = i;
                    maxLen = i - j;
                }
            }
        }

        return s.substring(left, right + 1);
    }

    public static void main(String[] args) {
        P5_new p = new P5_new();
        System.out.println(p.longestPalindrome("babad"));
    }
}
```

## <a name="6">6.ZigZag Conversion</a>
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"


Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

```

public class P6 {
    /*
     * zigzag pattern
     * 例如: abcdefghijklmnop, numRows = 4
     * a     g     m
     * b   f h   l n
     * c e   i k   o
     * d     j     p
     * 答案是agmbfglnceikodjp
     *
     * 规律是每次间隔2 * numRows - 2
     * 如果不是第0行和第numRows - 1行，每次还要加上(numRows - i - 1) * 2的字符
     */

    public String convert(String s, int numRows) {
        if (numRows == 1) {
            return s;
        }

        StringBuilder sb = new StringBuilder();
        int len = s.length();
        int interval = 2 * numRows - 2;
        int smallInterval = 0;

        for (int i = 0; i < numRows; i++) {
            for (int pos = i; pos < len; pos += interval) {
                sb.append(s.charAt(pos));
                if (i != 0 && i != numRows - 1) {
                    smallInterval = (numRows - i - 1) * 2;
                    if (pos + smallInterval < len) {
                        sb.append(s.charAt(pos + smallInterval));
                    }
                }
            }
        }

        return sb.toString();
    }

    public static void main(String[] args) {
        P6 p = new P6();
        System.out.println(p.convert("abcdefghi", 3));
    }
}
```

## <a name="7">7.Reverse Integer</a>
Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

Input: 123
Output:  321


Example 2:

Input: -123
Output: -321


Example 3:

Input: 120
Output: 21


Note:
Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

```

import java.util.Stack;

public class P7 {
    public int reverse(int x) {
        boolean isNegative = false;
        if (x < 0) {
            isNegative = true;
            x *= -1;
        }

        //检测是否溢出
        //实际把rst设为long，再判断是否大于Integer.MAX_VALUE和小于Integer.MIN_VALUE就好
        Stack<Integer> stack = new Stack<Integer>();
        int rst = 0;
        while (x != 0) {
            rst = rst * 10 + x % 10;
            if (rst != 0) {
                stack.push(x % 10);
            }
            x /= 10;
        }

        int t = rst;
        while (t != 0) {
            if (t % 10 != stack.pop()) {
                return 0;
            }

            t /= 10;
        }

        if (isNegative) {
            rst *= -1;
        }

        return rst;
    }

    public static void main(String[] args) {
        P7 p = new P7();
        System.out.println(p.reverse(-100));
        System.out.println(p.reverse(1000000004));
    }
}
```



## <a name="8">8.String to Integer (atoi)</a>
Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

Update (2015-02-10):
The signature of the C++ function had been updated. If you still see your function signature accepts a const char * argument, please click the reload button  to reset your code definition.

spoilers alert... click to show requirements for atoi.

Requirements for atoi:
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.


```

public class P8 {
    public int myAtoi(String str) {
        // 空字符串
        if (str.length() == 0) {
            return 0;
        }

        int sign = 1, pos = 0, len = str.length();
        // 去除行首空格
        for (pos = 0; pos < len; pos++) {
            if (str.charAt(pos) != ' ') {
                break;
            }
        }

        // 判断正负号，可能没有+号
        if (str.charAt(pos) == '-') {
            sign = -1;
            pos++;
        } else if (str.charAt(pos) == '+') {
            pos++;
        }

        // 转换
        int rst = 0;
        for (; pos < len; pos++) {
            int digit = str.charAt(pos) - '0';
            // 是否是数字
            if (digit >= 0 && digit <= 9) {
                // 有溢出的话，除以10和之前的不一样
                int temp = rst * 10 + digit;
                if (temp / 10 != rst) {
                    return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
                } else {
                    rst = temp;
                }
            } else {
                break;
            }
        }

        return rst * sign;
    }

    public static void main(String[] args) {
        P8 p = new P8();
        System.out.println(p.myAtoi("+12344123123123123123123123"));
    }
}
```


## <a name="9">9.Palindrome Number</a>

Determine whether an integer is a palindrome. Do this without extra space.

click to show spoilers.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

```
public class P9 {
    public boolean isPalindrome(int x) {
        if (x < 0) {
            return false;
        }

        // 这里额外用了空间，不好
        String s = Integer.toString(x);
        int l = 0, r = s.length() - 1;

        while (l < r) {
            if (s.charAt(l) == s.charAt(r)) {
                l++;
                r--;
            } else {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {
        P9 p = new P9();
        System.out.println(p.isPalindrome(-2147447412));
    }
}
```

```

public class P9_new {
    public boolean isPalindrome(int x) {
        if (x < 0 || (x != 0 && x % 10 == 0)) {
            return false;
        }

        int rev = 0;
        // 这里只转一半，就不会有全转后溢出的问题
        while (x > rev) {
            rev = rev * 10 + x % 10;
            x /= 10;
        }

        // 这里需要两个判断，因为x可能是奇数位数，这样rev会多一位
        return rev == x || rev / 10 == x;
    }

    public static void main(String[] args) {
        P9 p = new P9();
        System.out.println(p.isPalindrome(14744741));
    }
}
```



## <a name="10">10.Regular Expression Matching</a>
Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element. 匹配0个或多个前面的元素

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true


搜索，时间复杂度O(2^n)
```
public class P10 {
    public boolean dfs(String s, int sPos, int sLen, String p, int pPos, int pLen) {
        if (pPos >= pLen) {
            // p已经空了，如果sPos也是空，则匹配，否则不匹配
            if (sPos >= sLen) {
                return true;
            } else {
                return false;
            }
        }

        // 处理*的情况
        if (pPos < pLen - 1 && p.charAt(pPos + 1) == '*') {
            // 如果s和p相应字符一样，或者p为.，则s后移一位(*匹配一位s)，或p后移两位(*不匹配)
            if (sPos < sLen && (s.charAt(sPos) == p.charAt(pPos) || p.charAt(pPos) == '.')) {
                return dfs(s, sPos + 1, sLen, p, pPos, pLen) || dfs(s, sPos, sLen, p, pPos + 2, pLen);
            } else {
                return dfs(s, sPos, sLen, p, pPos + 2, pLen);
            }
        } else if (sPos < sLen && (s.charAt(sPos) == p.charAt(pPos) || p.charAt(pPos) == '.')) {
            // 处理字符相等，或p为.的情况，各自后移一位。注意判断sPos < sLen，否则会越界，且a和a.会误判为true
            return dfs(s, sPos + 1, sLen, p, pPos + 1, pLen);
        }

        return false;
    }

    public boolean isMatch(String s, String p) {
        return dfs(s, 0, s.length(), p, 0, p.length());
    }

    public static void main(String[] args) {
        P10 p = new P10();
        System.out.println(p.isMatch("aab", ".*"));
    }
}
```

动态规划, 时间复杂度O(n+m)

```

public class P10_new {
    /*
     * 1, If p.charAt(j) == s.charAt(i) :  dp[i][j] = dp[i-1][j-1];
     * 2, If p.charAt(j) == '.' : dp[i][j] = dp[i-1][j-1];
     * 3, If p.charAt(j) == '*':
     *   here are two sub conditions:
     *   1   if p.charAt(j-1) != s.charAt(i) : dp[i][j] = dp[i][j-2]  //in this case, a* only counts as empty
     *   2   if p.charAt(j-1) == s.charAt(i) or p.charAt(i-1) == '.':
     *                dp[i][j] = dp[i-1][j]   // in this case, a* counts as single or multiple a
     *             or dp[i][j] = dp[i][j-2]   // in this case, a* counts as empty
     */

    public boolean isMatch(String s, String p) {
        boolean[][] f = new boolean[s.length() + 1][p.length() + 1];
        f[0][0] = true;

        for (int i = 1, len = p.length(); i <= len; i++) {
            if (p.charAt(i - 1) == '*' && f[0][i - 2]) {
                f[0][i] = true;
            }
        }

        for (int i = 1, sLen = s.length(); i <= sLen; i++) {
            for (int j = 1, pLen = p.length(); j <= pLen; j++) {
                if (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.') {
                    f[i][j] = f[i - 1][j - 1];
                } else if (p.charAt(j - 1) == '*') {
                    if (s.charAt(i - 1) == p.charAt(j - 2) || p.charAt(j - 2) == '.') {
                        f[i][j] = f[i - 1][j] || f[i][j - 2];
                    } else {
                        f[i][j] = f[i][j - 2];
                    }
                }
            }
        }

        return f[s.length()][p.length()];
    }

    public static void main(String[] args) {
        P10_new p = new P10_new();
        System.out.println(p.isMatch("aa", "a*"));
    }
}
```

## <a name="11">11.Container With Most Water</a>
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
Note: You may not slant the container and n is at least 2.

```
public class P11 {
    /*
     * 双指针
     * 装水多少取决于较小的那个，所以每次移动较小的
     */

    public int maxArea(int[] height) {
        int maxWater = 0;
        int left = 0, right = height.length - 1;

        while (left < right) {
            maxWater = Math.max(maxWater, (right - left) * Math.min(height[left], height[right]));
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxWater;
    }

    public static void main(String[] args) {
        P11 p = new P11();
        int[] h = {1, 2, 3};
        System.out.println(p.maxArea(h));
    }
}
```


## <a name="12">12.Integer to Roman</a>
Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

```

public class P12 {
    /*
     * 数字和字符对应就好
     */
    public String intToRoman(int num) {
        int[] value = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] symbol = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        StringBuilder sb = new StringBuilder();
        int pos = 0;
        while (num > 0) {
            while (num >= value[pos]) {
                num -= value[pos];
                sb.append(symbol[pos]);
            }
            pos++;
        }

        return sb.toString();
    }

    public static void main(String[] args) {
        P12 p = new P12();
        System.out.println(p.intToRoman(7));
    }
}
```



## <a name="13">13.Roman to Integer</a>
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.
```
import java.util.HashMap;
import java.util.Map;

public class P13 {
    /*
     * 同样对应就好
     * 左边的小于右边则减，比如IV，第一个减1，第二个加5
     */

    public int romanToInt(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        map.put('M', 1000);
        map.put('D', 500);
        map.put('C', 100);
        map.put('L', 50);
        map.put('X', 10);
        map.put('V', 5);
        map.put('I', 1);

        int totalValue = 0;
        for (int i = 0, len = s.length(); i < len; i++) {
            if (i < len - 1 && map.get(s.charAt(i)) < map.get(s.charAt(i + 1))) {
                totalValue -= map.get(s.charAt(i));
            } else {
                totalValue += map.get(s.charAt(i));
            }
        }

        return totalValue;
    }

    public static void main(String[] args) {
        P13 p = new P13();
        System.out.println(p.romanToInt("VII"));
    }
}

```

## <a name="14">14.Longest Common Prefix</a>
Write a function to find the longest common prefix string amongst an array of strings.

```
public class P14 {
    /*
     * 逐一比较, 确认一下边界即可
     */

    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) {
            return "";
        }

        int pos = 0, minLen = Integer.MAX_VALUE;
        char temp;

        for (int i = 0, size = strs.length; i < size; i++) {
            int len = strs[i].length();
            if (len == 0) {
                return "";
            } else {
                minLen = Math.min(minLen, len);
            }
        }

        boolean isSame = true;
        for (int i = 0; i < minLen; i++) {
            temp = strs[0].charAt(i);
            for (int j = 1, size = strs.length; j < size; j++) {
                if (temp != strs[j].charAt(i)) {
                    isSame = false;
                    break;
                }
            }

            if (isSame) {
                pos++;
            } else {
                break;
            }
        }

        return strs[0].substring(0, pos);
    }

    public static void main(String[] args) {
        P14 p = new P14();
        String[] s = {"aa", "a"};
        System.out.println(p.longestCommonPrefix(s));
    }
}

```
## <a name="15">15.3Sum</a>
Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.

For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class P15 {
    /*
     * a + b + c = 0 -> a + b = -c
     * 本题主要是对重复数据的处理
     */

    public List<List<Integer>> threeSum(int[] nums) {
        // 先排序
        Arrays.sort(nums);
        List<List<Integer>> rst = new ArrayList<List<Integer>>();
        int target, left, right;

        for (int i = 0, len = nums.length; i < len - 2; i++) {
            // 和上一个数一样，跳过
            if (i != 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            // 从当前位置之后到最后找
            target = -1 * nums[i];
            left = i + 1;
            right = len - 1;
            while (left < right) {
                if (nums[left] + nums[right] < target) {
                    left++;
                } else if (nums[left] + nums[right] > target) {
                    right--;
                } else {
                    // 找到后添加进去
                    List<Integer> temp = new ArrayList<Integer>();
                    temp.add(nums[i]);
                    temp.add(nums[left]);
                    temp.add(nums[right]);
                    rst.add(temp);
                    left++;
                    right--;

                    // 和前一个数一样跳过，否则比如-3, 1, 1, 2, 2， 就会有两个-3, 1, 2
                    while (left < len && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (right > 0 && nums[right] == nums[right + 1]) {
                        right--;
                    }
                }
            }
        }

        return rst;
    }

    public static void main(String[] args) {
        P15 p = new P15();
        int[] n = {-1, -1, 0, 1, 2};
        List<List<Integer>> l = p.threeSum(n);
        for (List<Integer> t : l) {
            System.out.println(t);
        }
    }
}

```


## <a name="16">16.3Sum Closest</a>
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

```
import java.util.Arrays;

public class P16 {
    /*
     * O(n^2)
     * a + b + c 最接近 target
     * a + b 最接近 target - c
     * 双指针，记录minDis
     * 和比target - c小，一定是left++，因为right--会更小，一定不是结果
     * 和比target - c大同理
     */

    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int left, right, rst = 0, minDis = Integer.MAX_VALUE, newTarget;

        for (int i = 0, len = nums.length; i < len - 2; i++) {
            newTarget = -1 * nums[i] + target;
            left = i + 1;
            right = len - 1;

            while (left < right) {
                if (nums[left] + nums[right] < newTarget) {
                    if (newTarget - nums[left] - nums[right] < minDis) {
                        minDis = newTarget - nums[left] - nums[right];
                        rst = nums[left] + nums[right] + nums[i];
                    }
                    left++;
                } else if (nums[left] + nums[right] > newTarget) {
                    if (nums[left] + nums[right] - newTarget < minDis) {
                        minDis = nums[left] + nums[right] - newTarget;
                        rst = nums[left] + nums[right] + nums[i];
                    }
                    right--;
                } else {
                    return target;
                }
            }
        }

        return rst;
    }

    public static void main(String[] args) {
        P16 p = new P16();
        int[] nums = {-1, 2, 1, -4};
        System.out.println(p.threeSumClosest(nums, 1));
    }
}

```


## <a name="17">17.Letter Combinations of a Phone Number</a>

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.


Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.

```
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class P17 {
    /*
     * 搜索，或者用栈
     * 注意digits为""的情况，rst不能直接加str.toString，这样会有一个""元素
     */
    public void dfs(String digits, int pos, int len, Map<Character, String> map, List<String> rst, StringBuilder str) {
        if (pos == len) {
            if (len != 0) {
                rst.add(str.toString());
            }
            return;
        } else {
            String s = map.get(digits.charAt(pos));
            for (int i = 0, l = s.length(); i < l; i++) {
                str.append(s.charAt(i));
                dfs(digits, pos + 1, len, map, rst, str);
                str.deleteCharAt(pos);
            }
        }
    }

    public List<String> letterCombinations(String digits) {
        Map<Character, String> map = new HashMap<Character, String>();
        map.put('1', "1");
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");
        map.put('0', "0");

        StringBuilder str = new StringBuilder();
        List<String> rst = new ArrayList<String>();
        dfs(digits, 0, digits.length(), map, rst, str);

        return rst;
    }

    public static void main(String[] args) {
        P17 p = new P17();
        List<String> l = p.letterCombinations("");
        for (String s : l) {
            System.out.print(s + " ");
        }
    }
}

```


## <a name="18">18.4Sum</a>
Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.

For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]


```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class P18 {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        return kSum(nums, target, 4);
    }

    // 推广到了k个数之和为target的情况
    // 不断递归，边界是k = 2
    public List<List<Integer>> kSum(int[] nums, int target, int k) {
        Arrays.sort(nums);
        List<List<Integer>> rst = new ArrayList<List<Integer>>();
        // 注意nums为空的情况
        if (nums.length == 0) {
            return rst;
        } else {
            kSumImp(nums, 0, target, k, rst, new ArrayList<Integer>());
            return rst;
        }
    }

    public void kSumImp(int[] nums, int startPos, int target, int k,
                        List<List<Integer>> rst, List<Integer> pastValue) {
        // 后面的数太大，或者数不够多
        if (nums[startPos] * k > target || startPos + k > nums.length) {
            return;
        }

        if (k == 2) {
            int left = startPos;
            int right = nums.length - 1;
            while (left < right) {
                if (nums[left] + nums[right] < target) {
                    left++;
                } else if (nums[left] + nums[right] > target) {
                    right--;
                } else {
                    List<Integer> tempList = new ArrayList<Integer>();
                    // 加上之前的数
                    tempList.addAll(pastValue);
                    tempList.add(nums[left]);
                    tempList.add(nums[right]);
                    rst.add(tempList);
                    left++;
                    right--;
                    // 去重
                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (right > left && nums[right] == nums[right + 1]) {
                        right--;
                    }
                }
            }
        } else {
            for (int i = startPos, len = nums.length; i < len - k + 1; i++) {
                // 去重，有重复数字，最左边的能用最多的相同的数字，之后的就是多余了的
                if (i != startPos && nums[i] == nums[i - 1]) {
                    continue;
                }

                int newTarget = target - nums[i];
                pastValue.add(nums[i]);
                kSumImp(nums, i + 1, newTarget, k - 1, rst, pastValue);
                pastValue.remove(pastValue.size() - 1);
            }
        }
    }

    public static void main(String[] args) {
        P18 p = new P18();
        int[] nums = {1, 0, -1, 0, -2, 2};
        List<List<Integer>> list = p.fourSum(nums, 0);
        for (List<Integer> l : list) {
            for (int t : l) {
                System.out.print(t + " ");
            }
            System.out.println();
        }
    }
}
```

## <a name="19">19.Remove Nth Node From End of List</a>
Given a linked list, remove the nth node from the end of list and return its head.

For example,

   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
Note:
Given n will always be valid.
Try to do this in one pass.
实际上双指针就相当于是two pass了。。

```
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x;
    }
}

public class P19 {
    /*
     * 前后指针，前指针到null，后指针为要删除的元素
     */
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode left = head;
        ListNode right = head;
        for (int i = 0; i < n; i++) {
            right = right.next;
            // 题目已经确认了n有效，否则这里需要判断一下right是否为null
        }

        // 删除第一个
        if (right == null) {
            return head.next;
        }

        while (right.next != null) {
            left = left.next;
            right = right.next;
        }

        left.next = left.next.next;

        return head;
    }

    public static void main(String[] args) {
        P19 p = new P19();
        ListNode head = new ListNode(1);
        ListNode node2 = new ListNode(2);
        ListNode node3 = new ListNode(3);
        ListNode node4 = new ListNode(4);
        ListNode node5 = new ListNode(5);
        head.next = node2;
        node2.next = node3;
        node3.next = node4;
        node4.next = node5;

        p.removeNthFromEnd(head, 2);
        while (head != null) {
            System.out.println(head.val);
            head = head.next;
        }
    }
}

```

## <a name="20">20.Valid Parentheses</a>
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

```
import java.util.Stack;

public class P20 {
    /*
     * 栈的简单应用，括号匹配
     */
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0, len = s.length(); i < len; i++) {
            if (s.charAt(i) == '[' || s.charAt(i) == '(' || s.charAt(i) == '{') {
                stack.push(s.charAt(i));
            } else {
                if (stack.isEmpty()) {
                    return false;
                } else if ((s.charAt(i) == ']' && stack.pop() != '[')
                        || (s.charAt(i) == ')' && stack.pop() != '(')
                        || (s.charAt(i) == '}' && stack.pop() != '{')) {
                    return false;
                }
            }
        }

        return stack.isEmpty();
    }

    public static void main(String[] args) {
        P20 p = new P20();
        System.out.println(p.isValid("[]()"));
    }
}
```


## <a name="21">21.Merge Two Sorted Lists</a>
## <a name="22">22.Generate Parentheses</a>
## <a name="23">23.Merge k Sorted Lists</a>
## <a name="24">24.Swap Nodes in Pairs</a>
## <a name="25">25.Reverse Nodes in k-Group</a>
## <a name="26">26.Remove Duplicates from Sorted Array</a>
## <a name="27">27.Remove Element </a>
## <a name="28">28.Implement strStr()</a>
## <a name="29">29.Divide Two Integers</a>
## <a name="30">30.Substring with Concatenation of All Words </a>


