2/10/15: 2-Dimensional Dynamic Programming
==========================================

Edit Distance
-------------  
Given two strings $x = [x_{1},...,x_{n}]$ and $y = [y_{1},...,y_{n}]$  
There are changes to the arrays by insertions and deletions.  

**Penalty:** $n_{indels} + \alpha * n_{changes}$  

should set $\alpha < 2$, otherwise use an insert & deletion.  
An alighment is a sequence of pairs $(a_{1},b_{1}),...,(a_{k},b_{k})$ meaning  $x_{a_{i}}$ is aligned with $y_{b_{i}}$  
such that: $$ 1 \leq a_{1} < a_{2} < ... < a_{k} < m $$ $$ 1 \leq b_{1} < b_{2} < ... < b_{k} < n $$ 

**Goal:** find an alignment that minimizes the penalty. number of $indels = m + n - 2k$, number of $changes = |\{i: x_{a_{i}} \ne y_{b_{i}}\}|$  


### Subproblem
min penalty alignment on $(x_{1},...,x_{i})$ and $(y_{1},...,y_{j})$  
$opt(i,j).align$ is the alignment  
$opt(i,j).val$ is the penalty

#### Case Analysis & Recurrence
Is $(i,j)\in opt(i,j).align?$  
If **YES**: $$ opt(i,j).align = {(i,j)} \cup opt(i-1,j-1).align $$ $$ opt(i,j).val = opt(i-1,j-1).val \space [if \space x_{i} = y_{j}] $$ $$ opt(i,j).val = opt(i-1,j-1).val + \alpha \space [if \space x_{i} \ne y_{j}] $$  
If **NO**: $$ opt(i,j).val = 1 + min(opt(i-1,j).val, opt(i,j-1).val) $$  

#### Algorithm
$$ \alpha_{ij} = 0 \space [if \space x_{i} = y_{j}] $$
$$ \alpha_{ij} = \alpha \space [if \space x_{i} \ne y_{j}] $$  

```
For i = 1 to m:
    For j = 1 to n:
        opt(i,j).val = min (
            (a_{i,j} + opt(i-1,j-1).val), 
            (1 + opt(i-1, j).val (x_{i} del)),
            (1 + opt(i, j-1).val) (y_{j} ins))
        )
Return opt(m,n).val
```
Runtime $O(mn)$  
Space $O(mn)$  
Only need previous column, can reduce to $O(m+n)$  

---

Knapsack Problem
----------------
knapsack can hold weight $T$  
have objects with *integer* weights $w_{1}, w_{2}, ... , w_{n}$  
**Goal:** pick $S\subseteq \{1,...,n\}$ to maximize $\sum\limits_{i \in S} w_{i}$ such that $\sum\limits_{i \in S} w_{i} \leq T$  
And real number values $v_{1}, ... , v_{n}$ $$ max \sum\limits_{i \in S} v_{i} \space\space s.t. \space\space \sum\limits_{i \in S} w_{i} \leq T $$

### Subproblem
with objects $1,..,j$ and threshold $t\in \mathbb{Z}$  
$$ opt(j,t) \space\space max_{S \subseteq \{1,...,j\}} \sum\limits_{i \in S} v_{i} \space\space s.t. \space\space \sum\limits_{i \in S} w_{i} \leq t $$
$$ opt(j,t).\{val,set\} $$

```
Is j \in opt(j,t).set?
If yes, then: 
    opt(j,t).set = {j} \cup opt(j-1, t-w_{j}).set
    opt(j,t).val = v_{j} + opt(j-1, t-w_{j}).val
If no, then:
    opt(j,t) = opt(j-1,t)
```

#### Algorithm:
```
Init: 
    opt(0,t).val = 0 for all t
    opt(j,t).val = -infiniti for t < 0

For j = 1 to n:
    For t = 1 to T:
        if opt(j-1,t).val \geq v_{j} + opt(j-1, t-w_{j}).val, then:
            opt(j,t) = opt(j-1,t)
        else:
            opt(j-1,t).val = v_{j} + opt(j-1,t-w_{j}).val
            opt(j-1,t).set = \{j\} \cup opt(j-1,t-w_{j}).set
Return opt(n,T)
```

<br>

### Note:
To avoid sorting sets, just keep `branch.use = True` if $j \in opt(j,t)$  
$S = \emptyset, t = T$

#### Revised Algorithm:
```
[same init]
bb
For j = 1 to n:
    For t = 1 to T:
        if opt(j-1,t).val \geq v_{j} + opt(j-1, t-w_{j}).val, then:
[-]         ~~opt(j,t) = opt(j-1,t)~~
[+]         opt(j,t).val = opt(j-1,t).val
[+]         opt(j,t).use = False
        else:
            opt(j-1,t).val = v_{j} + opt(j-1,t-w_{j}).val
[-]         ~~opt(j-1,t).set = \{j\} \cup opt(j-1,t-w_{j}).set~~
[+]         opt(j-1,t).use = True
Return opt(n,T)
```

#### Then,  
```
for j = n down to 1:
    if opt(j,t).use = True, then:
        S = S \cup {j}
        t = t - w_{j}
```

Time $O(nT)$  
Space $O(nT) \rightarrow O(n^2)$  
Is *not* polynomial time because # of bits is $\sum log_{2} \left\lceil w_{i} \right\rceil + log_{2} T$  
Call it pseudo-polynomial time. Polynomial time in magnitudes of numbers in input.

