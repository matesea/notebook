Leetcode 315 count of smaller numbers after self
sample code of segment tree & binary indexed tree

# problem
You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

**Example:**
```
Input: [5,2,6,1]
Output: [2,1,1,0] 
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

# code
```C++
class Solution {
    int bit_query(vector<int>& bits, int i) {
        int res = 0;
        ++i;
        while (i > 0) {
            res += bits[i];
            i -= i & -i;
        }
        return res;
    }
    void bit_update(vector<int>& bits, int i) {
        ++i;
        while (i < bits.size()) {
            bits[i]++;
            i += i & -i;
        }
    }
    void st_update(vector<int>& st, int i) {
        int n = st.size()/2;
        i += n;
        while (i > 0) {
            st[i]++;
            i >>= 1;
        }
        // for (const auto& s: st)
        //     cout << s << " ";
        // cout << endl;
    }
    int st_query(vector<int>& st, int i) {
        int sum = 0, n = st.size()/2; 
        i += n;
        int l = n, r = i;
        while (l <= r) {
            if (l & 1) {
                sum += st[l];
                l++;
            }
            if ((r ^ 1) & 1) {
                sum += st[r];
                r--;
            }
            l >>= 1;
            r >>= 1;
        }
        return sum;
    }
public:
    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        // vector<int> bits(n+1, 0);
        vector<int> st(2*n, 0);
        vector<int> copy(nums);
        sort(copy.begin(), copy.end());
        unordered_map<int,int> idx;
        for (int i = 0; i < copy.size(); ++i)
            idx[copy[i]] = i;
        for (int i = nums.size()-1; i >= 0; --i) {
            copy[i] = st_query(st, idx[nums[i]] - 1);
            st_update(st, idx[nums[i]]);
        }
        return copy;
    }
};
```
