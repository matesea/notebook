# Problem

Given an unsorted array of integers, find the number of longest increasing subsequence.

**Example 1:**
```
Input: [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
```

**Example 2:**
```
Input: [2,2,2,2,2]
Output: 5
Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
```

**Note:** Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.

# Analysis
Although the difficulty of this problem is merely easy, it is still worth to note as I did it wrong for so many times.  
There are two conditions to consider: the disorder item is either too high or too low  

When you find nums[i-1] > nums[i] for some i, you will prefer to change nums[i-1]'s value, since a larger nums[i] will give you more risks that you get inversion errors after position i. But, if you also find nums[i-2] > nums[i], then you have to change nums[i]'s value instead, or else you need to change both of nums[i-2]'s and nums[i-1]'s values.

```C++
bool checkPossibility(vector<int>& nums) {
        int cnt = 0;                                                                    //the number of changes
        for(int i = 1; i < nums.size() && cnt<=1 ; i++){
            if(nums[i-1] > nums[i]){
                cnt++;
                if(i-2<0 || nums[i-2] <= nums[i])nums[i-1] = nums[i];                    //modify nums[i-1] of a priority
                else nums[i] = nums[i-1];                                                //have to modify nums[i]
            }
        }
        return cnt<=1;
    } 
```

Mine, the idea is same:
```C++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        if (!nums.size() || nums.size() == 1) return true;
        int misorder = -1;
        for (int i = 0; i < nums.size() - 1; ++i) {
            if (misorder == -1 && nums[i] > nums[i + 1]) {
                misorder = i;
            } else if (nums[i] > nums[i + 1])
                return false;
        }
        return misorder == -1 || misorder == 0 || misorder == nums.size() - 2 ||
            nums[misorder - 1] <= nums[misorder + 1] || nums[misorder] <= nums[misorder + 2];
    }
};
```

# Reference
- https://leetcode.com/problems/number-of-longest-increasing-subsequence/solution/
- https://leetcode.com/problems/number-of-longest-increasing-subsequence/discuss/107293/JavaC++-Simple-dp-solution-with-explanation
