# LeetCode Contest 436

## T1.[3446. 按对角线进行矩阵排序](https://leetcode.cn/problems/sort-matrix-by-diagonals/)

对于每一个 $grid[i][j]$ 我们计算 $i - j$ 可以发现在同一条反对角线上的 $i - j$ 是相同的，为了避免负数的出现我们给 $i - j$ 加一个偏移量，设 $k = n + i - j$ 。这样我们就可以用不同的 $k$ 唯一标识一条反对角线了，我们现在要对在一条反对角线上的数进行排序，可以想到的就是把一条反对角线上的数收集起来，排完序之后再放回去。

可以知道 $k$ 的范围是 $[1, 2 * n - 1]$ ，我们可以依次枚举 $k$，对于每个 $k$ ，可以知道 $minj = n - k$，$maxj = 2 * n - 1 - k$，由于不是每个 $k$ 都有 $i = 0$ 和 $i = 2 * n - 1$ 的时候所以 $minj = max(n - k, 0)$，$maxj = min(2 * n - 1 - k, n - 1)$。

最后判断下是排正序还是倒序 $k < n$ 时是下三角不包含最中间的反三角线，$k \ge n$ 时是上三角包含最中间的反三角线。

```java
import java.util.*;

class Solution {
    public int[][] sortMatrix(int[][] grid) {
        int n = grid.length;    

        for(int k = 1; k < 2 * n; k ++) {
            int minj = Math.max(n - k, 0);
            int maxj = Math.min(2 * n - 1 - k, n - 1);

            List<Integer> temp = new ArrayList<>();

            for(int j = minj; j <= maxj; j ++) {
                temp.add(grid[k + j - n][j]);
            }

            if(k < n) {
                temp.sort((a, b) -> (a - b));
            } else {
                temp.sort((a, b) -> (b - a));
            }
            
            int idx = 0;
            for(int j = minj; j <= maxj; j ++) {
                grid[k + j - n][j] = temp.get(idx ++);
            }
        }

        return grid;
    }
}

```

## T2 [3447. 将元素分配给有约束条件的组](https://leetcode.cn/problems/assign-elements-to-groups-with-constraints/)

分解因数，时间复杂度 $O(n\sqrt{m})$  $n$ 为数组长度，$m$为数组中数的最大值

```java
import java.util.*;

class Solution {
    public int[] assignElements(int[] groups, int[] elements) {
        Map<Integer, Integer> cnt = new HashMap<>();

        for(int i = 0; i < elements.length; i ++) {
            if(!cnt.containsKey(elements[i])) {
                cnt.put(elements[i], i);
            }
        }


        int[] ans = new int[groups.length];

        Arrays.fill(ans, -1);
        
        for(int i = 0; i < groups.length; i ++) {
            List<Integer> temp = new ArrayList<>();

            int t = groups[i];

            for(int j = 1; j <= t / j; j ++) {
                if(t % j == 0) {
                    temp.add(j);
                    if(t / j != j) temp.add(t / j);
                }
            }

            int min_idx = Integer.MAX_VALUE;
            for(int x : temp) {
                if(cnt.containsKey(x)) {
                    min_idx = Math.min(min_idx, cnt.get(x));
                }
                if(min_idx == 0) {
                    break;
                }
            }
            if(min_idx != Integer.MAX_VALUE) ans[i] = min_idx;
        }
    
        return ans;

    }
}
```

