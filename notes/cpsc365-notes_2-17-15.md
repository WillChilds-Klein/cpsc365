2/17/15: Divide & Conquer
=========================
  1. divide into sub-problems (barely overlapping)
  2. combine together  

MergeSort
---------
\begin{algorithm}[caption={Merge}, label={alg1}, mathescape={true}]
  input: $c_{1}, ..., c_{m}, d_{1}, .., d_{n}$ 
  init: $i=1, j=1$
  begin
    while $i \leq m$ and $j \leq n$:  
      if $c_{i} \leq d_{j}$:  
        append $c_{i}$ to $b$  
        $i = i + 1$
      else  
        append $d_{j}$ to $b$  
        $j = j + 1$
    end while

    if $i = m + 1$:
      append $d_{j},...,d_{n}$ to b  
    else  
      append $c_{i},...,c_{m}$ to $b$
  end
\end{algorithm}

\begin{algorithm}[caption={MergeSort}, label={alg2}, mathescape={true}]
  input: $a_{1},...,a_{n}$
  begin
    if $n-2$:
      return $\text{sort}(a_{1}, a_{2})$
    else:
      $c_{1},...,c_{\frac{n}{2}} = MergeSort(a_{1},...,a_{\frac{n}{2}})$
      $d_{1},...,d_{\frac{n}{2}} = MergeSort(a_{\frac{n}{2}},...,a_{n})$
      return $Merge(c,d)$
  end
\end{algorithm}


**Time:** $O(n + m)$  


### Prove by induction that $T(n) \leq cnlog_{2}(n)$  
  - Base Case: $n=2$
    + we know $T(2) \leq c \leq c2log_{2}(n) = c$
    + $\checkmark$  
  - Induction Step: assume True for $\frac{n}{2}$  
     $$ T(n) \leq 2T(\frac{n}{2}) + cn $$
    1. $\leq 2c(\frac{n}{2})log_{2}(\frac{n}{2}) + cn$ (by induction)
    2. $= cn(log_{2}(n) - 1) + cn$
    3. $= cnlog_{2}(n) - cn + cn = cnlog_{2}(n)$
    4. $\checkmark$


Example: Inversions
-------------------
  - __def__: an *inversion* is a pair $i < j$ s.t. $a_i > a_j$
  - __def:__ a *cross-inversion* is an $i \leq \frac{n}{2}, j > \frac{n}{2}$ with $a_{i} > a_{j}$


|---------|---|---|---|---|---|
|My rank  | d | a | c | b | e |
|James    | c | a | e | d | b |


  - count # of inversions - pairs out of order: _5 inversions_
  - rename s.t. my order is $1,2,3,...$ and other order is $a_{1},a_{2
  - yeilds:


|---------|---|---|---|---|---|
|My rank  | 1 | 2 | 3 | 4 | 5 |
|James    | 3 | 2 | 5 | 1 | 4 |


  - $w = \text{CountInversions}(a_{1},...,a_{n})$
  - $x = \text{CountInversions}(a_{1},...,a_{\frac{n}{2}})$
  - $y = \text{CountInversions}(a_{\frac{n}{2}+1},...,a_{n})$
  - $z = \text{CountCrossInversions}(a_{1},...,a_{\frac{n}{2}},a_{\frac{n}{2}+1},...,a_{n})$
  $$ w = x + y + z $$
  - here, we are going to compute more than we need to make our problem easier -->*Modified Merge Sort*
  - sort and count number of inversions


|-----------------|-----------------------------------------|
|$(b_1,...,b_n)$  | $w = \text{CountInversions}(a_1,a_n)$   |
|$(c_1,...,c_{\frac{n}{2}})$  | $x = \text{CountInversions}(a_1,...,a_{\frac{n}{2}})$   |
|$(d_1,...,d_{\frac{n}{2}})$  | $y = \text{CountInversions}(a_{\frac{n}{2}},...,a_n)$   |
|$(b_1,...,b_n)$  | $y = \text{CountInversions}(c_1,...,c_{\frac{n}{2}},d_1,...,d_{\frac{n}{2}})$   |


$$ (b_1,...,b_n),2 $$
\begin{algorithm}[caption={CountCrossInversions}, label={alg3}, mathescape={true}]
  input: $c_1,...,c_m,d_1,...,d_n$
  init: $i = j = 1, z = 0$
  begin
    while $i \leq m \wedge j \leq n$:
      if $c_i \leq d_j$
        append $c_i$ to $b$
        $i = i + 1$
      else
        append $d_j$ to $b$
        $j = j + 1$
        $z = z + m - i + 1$
    endwhile

    if $i = m + 1$
      append $d_j,...,d_n$ to $b$
    else
      append $c_i,...,c_m$ to $b$
    
    return $b$,$z$
  end
\end{algorithm}



Closest Pair
------------
  - __input:__ $(x_{1},y_{1}),...,(x_{n},y_{n})$
    + assume $x_{i}$'s are distinct
  - __init:__ $i=1, j=1$
  - __strategy__
    + find $i$ and $j$ s.t. $dist((x_{i},y_{i}),(x_{j},y_{j}))$ is as small as possible
    + can compare all pairs, pick smallest in $O(n^2)$
    + we will do this in $O(n\text{log}n)$


### Divide plane into right $(R)$ and left $(L)$
  1. sort so $x_1 < x_2 < ... < x_n$
  2. set $L = \{1,...,\frac{n}{2}\}$, $R = \{\frac{n}{2}+1,...,n\}$
  3. Find closest pair in $L$. Let $\delta_L$ be its distance.
  4. Find closest pair in $R$. Let $\delta_R$ be its distance.
  5. $\delta = min(\delta_L,\delta_R)$
    + only care about closest pair bridging $L$ and $R$
  6. will find closes $L$/$R$ pair if its distance is $< \delta$
    + only need to look at points within $\delta$ of center line
    + only need to compare $\frac{L}{R}$ pairs within $\delta$ of $y$-coordinate
    + sort points on $y$-coordinates
    + for each point in $L$, only need to compare to the $10$ points in $R$ with closest $y$ components
    + linear time to compare $L$,$R$ 
      * b/c each point compared to only $10$ others


### Idea
  - we only need to look at points withing $\delta$ of the center line.
  - we only need to compare $L/R$ pairs within $\delta$ of y-coordinate.
  - Sort points on y-coordinates.
  - For each point in $L$, only need to compare to the 10 points in $R$ with closest y-coordinates
  - So, takes linear time to compare $L$ and $R$.


### Explanation
  - at most 1 point per box.
  - For every point on left, are at most 10 boxes on right with points at distance $\leq \delta$
  - after sort, linear time to scan 10 proximal boxes, but sort take $log(n)$
  - $T(n) \leq 2T(\frac{n}{2}) + cn\text{log}_2(n)$
  - $T(n) \leq cn(\text{log}_2(n))^2)$
  - but wait! can sort on y-coordinates up front (pay $n\text{log}n$ once), and then keep track of order when split.
  - $T(n) \leq 2T(\frac{n}{2}) + c \space n \leq cn\text{log}_2 n$


Test on 2/24/15
---------------
  - test will be split into 2 rooms
  - content
    + memorization-heavy
    + 3 problems, pretty much straight out of class or PSETS
    + memorize problems from PSETS and class
    + follow directions carefully, test is one thing where no name is on every page. only first.



 
