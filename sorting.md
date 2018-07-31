# selection sort
- steps
  - for each iteration i, pick j which is the smallest among i+1 to end
  - swap i-th & j-th element
- characteristics
  - time complexity = O(n<sup>2</sup>) comparisons, O(n) exchanges in maximum

```Java
public static void sort(Comparable[] a) {
  int N = a.length;
  for (int i = 0; i < N; ++i) {
    int min = i;
    for (int j = i + 1; j < N; ++j) {
      if (less(a[j], a[min]))
        min = j;
      exch(a, i, min);
    }
  }
}
```

# insertion sort
- steps
  - for each iteration i, keep swap i-th element with previous one until the first i elements is ordered.
- characteristics
  - time complexity: O(n<sup>2</sup>) comparisons & exchanges in worst case
    - O(n) in best case, which is array is already sorted.
    - space complexity O(1), in-place, stable

```Java
public static void sort(Comparable[] a) {
  int N = a.length;
  for (int i = 0; i < N; ++i) {
    for (int j = i; j >0; --j) {
      if (less(a[j], a[j-1]))
        exch(a, j, j-1);
      else break;
    }
  }
}
```

# shell sort
- steps
  - h-sort array for elements with stride length h
  - reduce h to 1
- characteristics
  - time complexity relys on the h sequence picked
  - one example: 3x+1 = 1, 4, 13, 40, 121, 364... => O(n<sup>3/2</sup>) in worst case
  - useful in practice
    - fast unless array size is huge(used for small subarrays)
    - tiny fixed footprint for code(used for embedded system)
    - hardware sort prototype

```Java
public static void sort(Comparable[] a)
{
  int N = a.length;
  int h = 1;
  while (h < N/3) h = 3*h + 1; // 1, 4, 13, 40, 121, 364, ...
  while (h >= 1)
  { // h-sort the array.
    for (int i = h; i < N; i++)
    {
      for (int j = i; j >= h && less(a[j], a[j-h]); j -= h)
        exch(a, j, j-h);
    }
    h = h/3;
  }
}
```
