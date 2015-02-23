---
title: PSET 3, Problem 1
author: Will Childs-Klein {wdc22}
header-includes:
    - \usepackage{fancyhdr}
    - \pagestyle{fancy}
    - \fancyhead[CO,CE]{wdc22}
---

# 1

```
M: global array of "opt" objects, each with .val (total weight of matching) 
   and .set (set of edges in matching) attributes. ith entry of M is opt 
   matching object for subset of P [1,i] where 1 <= i <= n-1
   init all entries to null
P: list of "people", length n
w: list of weights, ith entry indicates weight for edge (i,i+1) for i,i+1 in P

def max_match(P, w):
  
  for i = [1,n-1]:
    compute_opt(i)

  return compute_opt(n-1)

end def

def compute_opt(j):
  if j == 0
    return null
  else if M[j] =\= null
    return M[j]
  else
    M[j] = max(w[j] + compute_opt(j-1), compute_opt(j+1))
    return M[j]
end def
```

## Runtime
$O(n)$ because there will be max $n$ iterative calls to compute_opt, and max 2 recursions per iterative call.