### 115. Distinct Subsequences
Given a string S and a string T, count the number of distinct subsequences of S which equals T.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

Example 1:
```
Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:

As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```
Example 2:
```
Input: S = "babgbag", T = "bag"
Output: 5
Explanation:

As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)

babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```

```
public class P115 {
    /*
    dp[i][j]表示s前i个字符，t前j个字符匹配的个数
    如果字符相等，可以这个字符匹配，加上s的前一个匹配t的0到j个字符
    if (s.charAt(i - 1) == t.charAt(j - 1)) {
        dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
    } else {
        dp[i][j] = dp[i - 1][j];
    }
    */
    public static void main(String[] args) {
        P115 p = new P115();
        System.out.println(p.numDistinct("babgbag", "bag"));
    }

    public int numDistinct(String s, String t) {
        int[][] dp = new int[s.length() + 1][t.length() + 1];

        for (int i = 0; i <= s.length(); i++) {
            dp[i][0] = 1;
        }

        for (int i = 1; i <= s.length(); i++) {
            for (int j = 1; j <= t.length(); j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        return dp[s.length()][t.length()];
    }
}

```

### 120. Triangle
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

Note:

Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.
```
import java.util.List;

public class P120 {
    public int minimumTotal(List<List<Integer>> triangle) {
        List<Integer> bottomLine = triangle.get(triangle.size() - 1);
        int[] dp = new int[bottomLine.size()];
        for (int i = 0; i < bottomLine.size(); i++) {
            dp[i] = bottomLine.get(i);
        }

        for (int i = triangle.size() - 1; i >= 1; i--) {
            for (int j = 0; j < i; j++) {
                dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i - 1).get(j);
            }
        }

        return dp[0];
    }
}

```
