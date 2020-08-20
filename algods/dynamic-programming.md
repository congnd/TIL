Dynamic programming is mainly an optimization over plain recursion. 
Wherever we see a recursive solution that has repeated call for same inputs, 
we can optimize it using Dynamic Programming.
The idea is to simply store the results of subproblems, 
so we don't have to recalculate them when needed latter.

This simple optimization reduces the time complexities from exponential to polinominal.
For example, if we write simple recursive solution for Fibonacci Numbers, 
we get exponential time complexity and if we optimize it by storing results of subproblems, 
time complexity reduces to linear.

___Recursive Solution___
```python
def fib(n):
  if n <= 1:
    return n
  return fib(n-2) + fib(n-1)
```

___Dynamic Programming Sulution___
```python
r = [0, 1]
for i in range(2, num):
  r.append(r[i-1] + r[i-2])
return r[num-1]
```
## Overlapping Subproblems

### Memorizaion (Top-Down)
Similar to the recursive solution with a small modification that it looks into a lockup table before computing solutions.

```python
import sys
num = int(sys.argv[1])
r = [None] * num

def fib(n):
  if n <= 1:
    return n
  if r[n-1] is None:
    r[n-1] = fib(n-1)
  if r[n-2] is None:
    r[n-2] = fib(n-2)
  return r[n-2] + r[n-1]

print(fib(num-1))
```

Since it's still a recursive solution, so the high possibility that the program will exceed maximum recursion depth.

### Tabulation (Bottom-Up)
Tabulated program builds a table in bottom-up fashion and return the last entry from the table.

```python
import sys

r = [0, 1]

num = int(sys.argv[1])

for i in range(2, num):
  r.append(r[i-1] + r[i-2])

print(r[num-1])
```
