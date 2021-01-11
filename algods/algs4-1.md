# algs4 - Part 1

## Week 2

### Assignments
- Spec [here](https://coursera.cs.princeton.edu/algs4/assignments/queues/specification.php)
- My solution [here](https://github.com/congnd/algs4/tree/master/part1/week2/queues)

#### Permutation with fixed memory utilization
> For an extra challenge and a small amount of extra credit, use only one Deque or RandomizedQueue object of maximum size at most k.

We need to use the [Reservoir sampling algorithms](https://en.wikipedia.org/wiki/Reservoir_sampling) to solve this problem.

```Java
while (!StdIn.isEmpty()) {
    count++;
    String s = StdIn.readString();
    if (count > k && StdRandom.uniform(count) < k) {
        queue.dequeue();
    }
    else if (count > k) {
        continue;
    }
    queue.enqueue(s);
}
```
