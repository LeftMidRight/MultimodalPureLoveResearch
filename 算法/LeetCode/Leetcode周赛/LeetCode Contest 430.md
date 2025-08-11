# LeetCode Contest 430

## T1 [3402.使每一列严格递增的最少操作次数](https://leetcode.cn/problems/minimum-operations-to-make-columns-strictly-increasing/description/)[¶](https://leftmidright.cn/blog/leetcode-contest-430/#t1-3402)

每次固定一列，按行从上到下进行遍历，如果 $grid[i][j]>grid[i−1][j]$ 就让 $grid[i][j]$ 自增一次，且记录的答案自增一次

```java
class Solution {
    public int minimumOperations(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;

        int ans = 0;

        for(int j = 0; j < m; j ++) {
            for(int i = 0; i < n; i ++) {
                while(i > 0 && grid[i][j] <= grid[i - 1][j]) {
                    ans ++;
                    grid[i][j] ++;
                }
            }
        }

        return ans;
    }
}
```

## T2 [3403. 从盒子中找出字典序最大的字符串 I](https://leetcode.cn/problems/find-the-lexicographically-largest-string-from-the-box-i/description/)

这道题目多了很多不必要的描述，使得题目的意思并不是很好把握。

题目的意思是：**从字符串 word 中 分出 numFriends 个子字符串，每个字符串不能为空，判断可以分出的字典序最大的子字符串是哪个？**

由字典序的定义可以知道，在 $word$ 中同一起始位置，字符串长度越长，字典序越大。所以我们很自然地想到，我们只需要为 $word$ 中每个字母，找一个以该字母开头的可以划分的最长的子字符串。 $word$ 中题目要求从 $word$ 中划分出 $numFriends$ 个子字符串，且每个子字符串不能为空字符串，所以最长的子字符串不能超过 $word.length−numFriends+1$ 。

注意要特判，当 $numFriends=1$ 的时候只能划分出原字符串一个字符串。

```java
class Solution {
    public String answerString(String word, int numFriends) {
        if(numFriends == 1) {
            return word;
        }
        int len = word.length() - numFriends + 1;
        int n = word.length();
        String ans = "";
        
        for (int i = 0; i < n; i++) {
            String temp;
            if(i < n - len + 1) {
                temp = word.substring(i, i + len);
            } else {
                temp = word.substring(i, word.length());
            }

            if(temp.compareTo(ans) > 0) {
                ans = temp;
            }
        }
        return ans;
    }
}
```

## T3 [3404. 统计特殊子序列的数目](https://leetcode.cn/problems/count-special-subsequences/description/)

题目大意：从数组 $nums$ 中选取 $4$ 下标 $p,q,r,s$，满足 $p<q<r<s$ 且这 $4$ 个数每两个下标之间，至少相隔一个位置，且这 $4$ 个下标满足 $nums[p]∗nums[r]==nums[q]∗nums[s]$

数组长度为 $1000$ ，需要 $O(n^2)$ 以下的时间复杂度才能够不超时解决问题。