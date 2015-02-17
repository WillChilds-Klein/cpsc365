2/10/15: Divide & Conquer
=========================
  1. divide into sub-problems (barely overlapping)
  2. combine together  

MergeSort
---------
for even $n$,   
def $Merge(c_{1}, ..., c_{m}, d_{1}, .., d_{n})$:  
  $i=1, j=1$  
  while $(i \leq m$ and $j \leq n)$:  
    if $c_{i} \leq d_{j}$:  
      append $c_{i}$ to $b$  
      $i = i + 1$
    else  
      append $d_{j}$ to $b$  
      $j = j + 1$  
  if $i = m + 1$:
    append $d_{j},...,d_{n}$ to b  
  else  
    append $c_{i},...,c_{m}$ to $b$  

def $MergeSort(a_{1},...,a_{n})$:  
  if $n-2$:  
    **return** $sort(a_{1}, a_{2})$  
  $c_{1},...,c_{\frac{n}{2}} = MergeSort(a_{1},...,a_{\frac{n}{2}})$  
  $d_{1},...,d_{\frac{n}{2}} = MergeSort(a_{\frac{n}{2}},...,a_{n})$  
  **return** $Merge(c,d)$

**Time:** $O(n + m)$  

Prove by induction that $T(n) \leq cnlog_{2}(n)$  
Base $n=2$, we know $T(2) \leq c \leq c2log_{2}(n) = c$  
Induction: assume True for $\frac{n}{2}$  
$$ T(n) \leq 2T(\frac{n}{2}) + cn $$
$$ \leq 2c(\frac{n}{2})log_{2}(\frac{n}{2}) + cn \space\space\space\space(by \space induction)$$
$$ = cn(log_{2}(n) - 1) + cn $$
$$ = cnlog_{2}(n) - cn + cn = cnlog_{2}(n) $$  

Example
-------
My rank: d a c b e  
--James: c a e d b  

count # of inversions - pairs out of order: _5 inversions_  
rename s.t. my order is $1,2,3,...$ and other order is $a_{1},a_{2},...$

My rank: 1 2 3 4 5  
--James: 3 2 5 1 4  

$$ w = CountInversions(a_{1},...,a_{n}) $$
$$ x = CountInv(a_{1},...,a_{\frac{n}{2}}) $$
$$ y = CountInv(a_{\frac{n}{2}+1},...,a_{n}) $$
$$ z = CountCrossInv(a_{1},...,a_{\frac{n}{2}},a_{\frac{n}{2}+1},...,a_{n}) $$
$$ w = x + y + z $$  
a cross-inversion is an $i \leq \frac{n}{2}, j > \frac{n}{2}$ with $a_{i} > a_{j}$  

Closest Pair
------------
**Input:** $(x_{1}),y_{1}),...,(x_{n},y_{n})$  
    find $i$ and $j$ s.t. $dist((x_{i},y_{i}),(x_{j},y_{j}))$ is as small as possible  
    assume $x_{i}$'s are distinct  

Can compare all pairs in $O(n^2)$, but we'll do it in $O(n \space log(n))$  

Divide plane into right $(R)$ and left $(L)$  
  1. sort so $x_1 < x_2 < ... < x_n$  
  2. set $L = \{1,...,\frac{n}{2}\}$, $R = \{\frac{n}{2}+1,...,n\}$  
  3. Find closest pair in $L$. Let $\delta_L$ be its distance.  
  4. Find closest pair in $R$. Let $\delta_R$ be its distance.  
  5. $\delta = min(\delta_L,\delta_R)$  
  6. will find closes $L$/$R$ pair if its distance is $< \delta$  

Idea:  
  - we only need to look at points withing $\delta$ of the center line.  
  - we only need to compare $L/R$ pairs within $\delta$ of y-coordinate.  
  - Sort points on y-coordinates.  
  - For each point in $L$, only need to compare to the 10 points in $R$ with closest y-coordinates  
  - So, takes linear time to compare $L$ and $R$.

Explanation:  
  - at most 1 point per box.
  - For every point on left, are at most 10 boxes on right with points at distance $\leq \delta$  
  - after sort, linear time to scan 10 proximal boxes, but sort take $log(n)$  
  - $T(n) \leq 2T(\frac{n}{2}) + c \space n \space log_2(n)$  
  - get $\space T(n) \leq c' n(log_2(n))^2)$  
  - Can sort on y-coordinates up front (pay $n\space log \space n$ once), and then keep track of order when split.  
  - $T(n) \leq 2T(\frac{n}{2}) + c \space n$

Test on 2/24/15
---------------
  - test will be split into 2 rooms  
  - content  
    + memorization-heavy
    + 3 problems, pretty much straight out of class or PSETS
    + memorize problems from PSETS and class  
    + follow directions carefully, test is one thing where no name is on every page. only first.



 
