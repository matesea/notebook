# Word Ladder I & II
## Problem
### Word Ladder I
Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:
### Word Ladder II
Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

**Common rules**
1. Only one letter can be changed at a time
2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

**Note:**
- Return an empty list if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume beginWord and endWord are non-empty and are not the same.

**Example 1:**
```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```
**Example 2:**
```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: []

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## Analysis
BFS. Find all possible words that can be transformed from current word. Return when one of them matches endWord.

**Tricks**
- Assume *s1* is the word set evloved from beginWord, *s2* is the set evloved from endWord, in reverse direction.  
Always pick the smaller set to expand so we can reduce the words to transform. 
When anyone word is found in the other set, we are finished.
```  
                       +-> w1		w4 -+
beginWord+-> w2		w5 -+-> endWord
                       +-> w3		w6 -+
```

## Solution
### Word Ladder II
found in submit solutions.
very fast but not easy to think/implement of in short time.
```C++
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        
        vector<vector<string>> ladders;
        
        int len = beginWord.length();
        if (len != endWord.length()) {
            return ladders;
        }
        
        unordered_set<string> wordDict;
        wordDict.insert(wordList.begin(), wordList.end());
        if (wordDict.count(endWord) == 0) {
            return ladders;
        }
        
        unordered_set<string> startNodes({beginWord}), endNodes({endWord});
        unordered_map<string, vector<string>> links;
        wordDict.erase(beginWord);
        wordDict.erase(endWord);
        
        buildNextLinks(1, wordDict, startNodes, endNodes, links);    
//cout<<"\nlinks: \n";
//for(auto& l : links) if(l.second.size()>0) {cout<<"   "<<l.first<<": ["; for(auto& f:l.second) cout<<f<<" ";cout<<"]\n";}
        
        vector<string> path(1, beginWord);
        path.reserve(wordList.size() + 1);
        buildAllLadders(links, endWord, path, ladders);
        
        return ladders;
    }
    
private:
    
    void buildNextLinks(const int level,
                        unordered_set<string> & wordDict,
                        unordered_set<string>& startNodes,
                        unordered_set<string>& endNodes,
                        unordered_map<string, vector<string>>& links) {
        
        if ((startNodes.size() == 0) || (endNodes.size() == 0)) {
            return;
        }        

        // !!! performance gain: prefer to work on the smaller set for quick converging
        bool forward = (startNodes.size() <= endNodes.size());        
        unordered_set<string>& workNodeSet = forward ? startNodes : endNodes;
        const unordered_set<string>& refNodeSet = forward ? endNodes : startNodes;
        const unordered_set<string> curNodeSet(workNodeSet); 
        workNodeSet.clear();
        
        bool done = false;
        for (auto& w : curNodeSet) {            
            string nextw = w;
            for (int i = 0; i < nextw.size(); ++i) {
                for (char c = 'a'; c <= 'z'; ++c) {
                    if (c != w[i]) {
                        nextw[i] = c;
                        bool met = (refNodeSet.count(nextw) > 0);
                        if (met ||  (wordDict.count(nextw) > 0)) {
                            done = (done || met);
                            if (forward) {
                                links[w].push_back(nextw);
                            }
                            else {
                                links[nextw].push_back(w);
                            }
                            workNodeSet.insert(nextw);
                        }
                    }
                }
                nextw[i] = w[i];
            }
        }
//cout<<"level="<<level<<"  start:<"; for(auto& ss : startNodes) cout<<ss<<" ";cout<<">\n";
//cout<<"         end:  <"; for(auto& ee : endNodes) cout<<ee<<" ";cout<<">\n";        
        if (! done) {
            for (auto& s : workNodeSet) {
                wordDict.erase(s);
            }
            buildNextLinks(level+1, wordDict, startNodes, endNodes, links);
        }
    }

    void buildAllLadders( const unordered_map<string, vector<string>>& links,
                          const string& endWord,
                          vector<string>& path,
                          vector<vector<string>>& ladders  ) {
        if (path.size() == 0) {
            return;
        }
        
        string& node = path.back();        
        if (node == endWord) {
            ladders.push_back(path);
        }
        else if (links.count(node) > 0) {
            auto it = links.find(node);
            for (auto& next : it->second) {
                path.push_back(next);
                buildAllLadders(links, endWord, path, ladders);
                path.pop_back();
            }
        }
    }
};
```

## Reference
http://www.cnblogs.com/grandyang/p/4539768.html
