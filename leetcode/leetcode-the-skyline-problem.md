# Analysis
1. Divide and conquer O(nlogn)
2. Track turning points O(nlogn)  
for each left & right boundary with height different from previous height  
add a turning point  

# Code
## C++
```C++
 class Solution {
 public:
    vector<pair<int, int>> getSkyline(vector<vector<int>>& buildings) {
        vector<pair<int, int>> height;
        for (auto &b : buildings) {
            height.push_back({b[0], -b[2]});
            height.push_back({b[1], b[2]});
        }
        sort(height.begin(), height.end());
         multiset<int> heap;
         heap.insert(0);
         vector<pair<int, int>> res;
         int pre = 0, cur = 0;
         for (auto &h : height) {
             if (h.second < 0) {
                 heap.insert(-h.second);
             } else {
                 heap.erase(heap.find(h.second));
             }   
             cur = *heap.rbegin();
             if (cur != pre) {
                 res.push_back({h.first, cur});
                 pre = cur;
             }
         }
         return res;
     }
 };
```
# Reference
https://www.geeksforgeeks.org/divide-and-conquer-set-7-the-skyline-problem/    
http://www.cnblogs.com/grandyang/p/4534586.html    
http://www.cnblogs.com/easonliu/p/4531020.html
