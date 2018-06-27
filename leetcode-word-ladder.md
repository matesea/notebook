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

## Reference
http://www.cnblogs.com/grandyang/p/4539768.html
