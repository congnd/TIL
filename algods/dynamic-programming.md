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
