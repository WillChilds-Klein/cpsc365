2/5/15 - Dynamic Programming
============================
Weighted Interval Scheduling
----------------------------
input intervals $(s_{i}, f_{i})$ with weight (or value) $w_{i}$

__Goal:__ find a subset $A$ of non-overlapping intervals, maximizing:
    $$ \sum\limits_{i \in A} w_{i}$$

Order intervals so $f_{1} \leq f_{2} \leq...\leq f_{n}$
sub-problem: same problem, but only have intervals $1,2,...,j$

$opt(j).set = $ the set of intervals maximizing weight, out of first $j$  
$opt(j).val = $ weight of this set.  
**want:** opt(j)

For each $j$, will compute $opt(j)$ from $opt(1),...,opt(j-1)$

Case analysis: is $j \in opt(j).set$ ?  
if no: $opt(j)= opt(j-1)$  
if yes: then for all intervals $i$ ooverlapping $j$, $i \notin opt(j).set$   
    - $i \leq j \Rightarrow f_{i} \leq f_{j}$, so any overlap if $s_{j} \lt f_{i}$  
    - so, only keep those with $f_{i} \leq s_{j}$  
    - $p_{j} = max(i: f_{i} \leq s_{j})$  
    - $opt(j).set = {j} \cup opt(p_{j}).set$  
    - $opt(j).val = w_{j} \cup opt(p_{j}).val$

Algorithm 1
-----------
```
    0. *init:* $opt(0) = {empty set}$  
    1. sort input so f_{1} \leq f_{2} \leq...\leq f_{n}$  
    2. compute $p_{j}$ for all $j$  
    3. $opt(1).set = {1}$, $opt(1).val = w_{1}$  
    4. for $j = 2$ to $n$:  
        if $w_{j} + opt(p_{j}).val \gt opt(j-1).val$  
            $opt(j).set = {j} \cup opt(p_{j}).set$  
            $opt(j).val = w_{j} \cup opt(p_{j}).val$  
            $opt(j).use = True$**  
        else  
            $opt(j) = opt(j-1)$  
            $opt(j).use = False$** 
```

Implement  
    1. O(n log n)  
    2. each $p_{j}$ by binary search in time O(log n)  
        - so, all in O(n log n) time.  
        - **: replace *.set with *.use = True if $j \in opt(j).set$
    3. (and onwards) of time O(n)

Segmented Least Squares
-----------------------
given data points $(x_{1},y_{1}),...,(x_{n},y_{n})$$  
try to find a lind $l(x) = ax+b$ which minimizes $err$ with
    $$ err = \sum\limits_{i} (l(x_{i})-y_{i})^2$$
for $x_{i} = $ look time, $y_{i} = $ temp.  

To use $k$ lines, divide into $k$ segments $[s_{i},f_{i}]$
    $$ 1 = s_{1} \lt s_{2} \lt s_{3} \lt ... \lt s_{k}, f_{k} = n $$
    $$ s_{i+1} = 1 + f_{i} $$
    $$ err_{s,f} = \min\limits_{l} \sum\limits_{i=s}^f (l(x_{i})-y_{i})^2 $$

**Goal:** find $k$ and $s_{2},...,s_{k}$ to minimize
    $$ \sum\limits_{j=1}^k (P + err_{s_{j},f_{j}}) = kP + \sum\limits_{j=1}^k err_{s_{j},f_{j}}$$
where $P$ = penalty, and $err_{s_{j},f_{j}}$ = error of line

Solve by dynamic programming  
    - sub-problem = problem on points $1,2,...,h  
    - $opt(h).val =$ the value  
    - $opt(h).start =$ the start times used  
if last interval starts at  $s$:
    $$ opt(h) = P + err_{s,h} + opt(s-1).val $$

Algorithm  
    0. $opt(0).val = 0$  
    1. for $h = 1$ to $n$:  
        - $s = arg \min\limits_{1 \leq s \leq h} P + err_{s,h} + opt(s-1).val$  
        - $opt(h).val = P + err_{s,h} + opt(s-1).val$  
        - $opt(h).starts = {s} \cup opt(s-1).starts$


