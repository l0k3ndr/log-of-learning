# 19-jan-2021

### 4 - locate smallest window to be sorted

```python
def window(array):
  left, right = None, None
  
  s = sorted(array)
  
  for i in range(len(array)):
    if array[i] != s[i] and left is None:
      left = i
    elif array[i] != s[i]:
      right = i
  return left, right
```

### 3 - number of smaller elements to the right

nlogn

```python
import bisect

def smaller_counts(lst):

  result = []
  seen = []
  
  for num in reversed(lst):
    i = bisect.bisect_left(seen, num)
    result.append(i)
    bisect.insort(seen, num)
    
  return list(reversed(result))
```

### 2 - max circular subarray with wraparound

the max wraparound sum total minus min subarray sum and max of kadane

```python
def maximum_circular_subarray(arr):
  max_subarray_sum_wraparound = sum(arr) - min_subarray_sum(arr)  
  return max(max_subarray_sum(arr), max_subarray_sum_wraparound)

def max_subarray_arr(arr):

  max_ending_here = max_so_far = 0
  
  for x in arr:
    max_ending_here = max(x, max_ending_here + x)
    max_so_far = max(max_so_far, max_ending_here)
    
  return max_so_far
  
def min_subarray_sum(arr):
  min_ending_here = min_so_far = 0
  
  for x in arr:
    min_ending_here = min(x, min_ending_here + x)
    min_so_far = min(min_so_far, min_ending_here)
    
  return min_so_far
```
  

### 1 - datetime combine

```python
>>> datetime.now()
datetime.datetime(2021, 1, 19, 15, 35, 0, 358483)
>>> datetime.combine(datetime.now(), datetime.max.time())
datetime.datetime(2021, 1, 19, 23, 59, 59, 999999)
>>>
```
