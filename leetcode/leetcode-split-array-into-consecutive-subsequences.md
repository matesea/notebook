# Problem
You are given an integer array sorted in ascending order (may contain duplicates), you need to split them into several subsequences, where each subsequences consist of at least 3 consecutive integers. Return whether you can make such a split.

**Example 1:**
```
Input: [1,2,3,3,4,5]
Output: True
Explanation:
You can split them into two consecutive subsequences : 
1, 2, 3
3, 4, 5
```

**Example 2:**
```
Input: [1,2,3,3,4,4,5,5]
Output: True
Explanation:
You can split them into two consecutive subsequences : 
1, 2, 3, 4, 5
3, 4, 5
```

**Example 3:**
```
Input: [1,2,3,4,4,5]
Output: False
```

**Note:**
1. The length of the input is in range of [1, 10000]

# Analysis
- Greedy algorithm
1. We iterate through the array once to get the frequency of all the elements in the array
1. We iterate through the array once more and for each element we either see if it can be appended to a previously constructed consecutive sequence or if it can be the start of a new consecutive sequence. If neither are true, then we return false.

```Java
public boolean isPossible(int[] nums) {
    Map<Integer, Integer> freq = new HashMap<>(), appendfreq = new HashMap<>();
    for (int i : nums) freq.put(i, freq.getOrDefault(i,0) + 1);
    for (int i : nums) {
        if (freq.get(i) == 0) continue;
        else if (appendfreq.getOrDefault(i,0) > 0) {
            appendfreq.put(i, appendfreq.get(i) - 1);
            appendfreq.put(i+1, appendfreq.getOrDefault(i+1,0) + 1);
        }   
        else if (freq.getOrDefault(i+1,0) > 0 && freq.getOrDefault(i+2,0) > 0) {
            freq.put(i+1, freq.get(i+1) - 1);
            freq.put(i+2, freq.get(i+2) - 1);
            appendfreq.put(i+3, appendfreq.getOrDefault(i+3,0) + 1);
        }
        else return false;
        freq.put(i, freq.get(i) - 1);
    }
    return true;
}
```
- Opening and closing events
1. let c = count[x+1]-count[x], if c > 0, some sequence starts at x+1
1. if c < 0, some sequence ends at x

```Java
class Solution {
    public boolean isPossible(int[] nums) {
        Integer prev = null;
        int prevCount = 0;
        Queue<Integer> starts = new LinkedList();
        int anchor = 0;
        for (int i = 0; i < nums.length; ++i) {
            int t = nums[i];
            if (i == nums.length - 1 || nums[i+1] != t) {
                int count = i - anchor + 1;
                if (prev != null && t - prev != 1) {
                    while (prevCount-- > 0)
                        if (prev < starts.poll() + 2) return false;
                    prev = null;
                }

                if (prev == null || t - prev == 1) {
                    while (prevCount > count) {
                        prevCount--;
                        if (t-1 < starts.poll() + 2)
                            return false;
                    }
                    while (prevCount++ < count)
                        starts.add(t);
                }
                prev = t;
                prevCount = count;
                anchor = i+1;
            }
        }

        while (prevCount-- > 0)
            if (nums[nums.length - 1] < starts.poll() + 2)
                return false;
        return true;
    }
}
```

# Reference
- https://leetcode.com/problems/split-array-into-consecutive-subsequences/solution/
- https://leetcode.com/problems/split-array-into-consecutive-subsequences/discuss/106496/Java-O(n)-Time-O(n)-Space
