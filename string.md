# string sorting
- key-indexed counting, aka counting sort
- LSD radix sort
- MSD radix sort
- [3-way radix quicksort](https://en.wikipedia.org/wiki/Multi-key_quicksort)
- suffix array
  - [introduction](https://www.geeksforgeeks.org/suffix-array-set-1-introduction/)
  - Manber-Myers algorithm
  - [NlogN algorithm](https://www.geeksforgeeks.org/suffix-array-set-2-a-nlognlogn-algorithm/)
  - [another implementation](http://apps.topcoder.com/forums/?module=RevisionHistory&messageID=1171511)

# tries
- R-way tries
- ternary search tries

# substring search
- Knuth-Morris-Pratt
- Boyer-Moore
  - scan characters in pattern from right to left
  - can skip as many as M text chars when finding one not in the pattern
  - M = pattern length
  - best case: N/M
  - worst case: N\*M, mismatch at 1st character of pattern
    - avoid by adding KMP like rule to guard against repetitive patterns
- Rabin-Karp
  - compute hash of pattern characters 0 to M-1
  - for each i, compute a hash of text characters i to M+i-1
  - if pattern hash = text substring hash, check for a match

# regular expression
- NFA simulation: in worst case time complexity = N\*M
- NFA construction: O(M)
