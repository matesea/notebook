# problem
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

**Exmaple:**  
Input: [0,1,0,2,1,0,1,3,2,1,2,1]  
Output: 6

[problem description on leetcode](https://leetcode.com/problems/trapping-rain-water/description/)

# analysis
1. My personal solution:  
   for every index i find the height at the right hand side that can trap rain.  
   its height should be >= height[i] or it's the local max if all heights at the right hand side are less than height[i].  
   time complexity: **O(n)**  
   space: **O(n)**
   drawback: too complex and prone to make mistake
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int> s;
        vector<int> idx;
        for (int i = height.size()-1; i >= 0; --i) {
            while (!s.empty() && height[i] > height[s.top()]) s.pop();
            if (s.empty()) idx.push_back(-1);
            else idx.push_back(s.top());
            s.push(i);
        }
        reverse(idx.begin(), idx.end());
        int localMax = height.size()-1;
        int res = 0;
        for (int i = height.size()-1; i >= 0; --i) {
            if (idx[i] == -1) idx[i] = localMax;
            if (height[i] > height[localMax]) localMax = i;
        }
        for (int i = 1; i < height.size(); ++i) {
           if (height[i-1] > height[i]) {
               int h = min(height[i-1], height[idx[i-1]]);
               for (int j = i-1; j <= idx[i-1]; ++j)
                   if (h > height[j]) res += h - height[j];
               i = idx[i-1];
           }
        }
        return res;
    }
};
```
2. solution from geeksforgeeks
   Much easier to understanad and still **O(n)**  
   drawback: space complexity is **O(2n)** and height array needs to be traversed twice
```C++
int findWater(int arr[], int n)
{
    // left[i] contains height of tallest bar to the
    // left of i'th bar including itself
    int left[n];
 
    // Right [i] contains height of tallest bar to
    // the right of ith bar including itself
    int right[n];
 
    // Initialize result
    int water = 0;
 
    // Fill left array
    left[0] = arr[0];
    for (int i = 1; i < n; i++)
       left[i] = max(left[i-1], arr[i]);
 
    // Fill right array
    right[n-1] = arr[n-1];
    for (int i = n-2; i >= 0; i--)
       right[i] = max(right[i+1], arr[i]);
 
    // Calculate the accumulated water element by element
    // consider the amount of water on i'th bar, the
    // amount of water accumulated on this particular
    // bar will be equal to min(left[i], right[i]) - arr[i] .
    for (int i = 0; i < n; i++)
       water += min(left[i],right[i]) - arr[i];
 
    return water;
}
```

3. Search from left to right and maintain a max height of left and right separately, which is like a one-side wall of partial container. Fix the higher one and flow water from the lower part. For example, if current height of left is lower, we fill water in the left bin. Until left meets right, we filled the whole container.  
   time complexity: **O(n) and one time traversal**  
   space: **O(1)**
```C++
class Solution {
public:
    int trap(int A[], int n) {
        int left=0; int right=n-1;
        int res=0;
        int maxleft=0, maxright=0;
        while(left<=right){
            if(A[left]<=A[right]){
                if(A[left]>=maxleft) maxleft=A[left];
                else res+=maxleft-A[left];
                left++;
            }
            else{
                if(A[right]>=maxright) maxright= A[right];
                else res+=maxright-A[right];
                right--;
            }
        }
        return res;
    }
};
```

4. stack approach
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int> st;
        int i = 0, res = 0, n = height.size();
        while (i < n) {
            if (st.empty() || height[i] <= height[st.top()]) {
                st.push(i++);
            } else {
                int t = st.top(); st.pop();
                if (st.empty()) continue;
                res += (min(height[i], height[st.top()]) - height[t]) * (i - st.top() - 1);
            }
        }
        return res;
    }
};
```
# reference
https://leetcode.com/problems/trapping-rain-water/discuss/17357/Sharing-my-simple-c++-code:-O(n)-time-O(1)-space  
http://www.cnblogs.com/grandyang/p/4402392.html  
https://www.geeksforgeeks.org/trapping-rain-water/
