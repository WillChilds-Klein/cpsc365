---
title: Homework 8
subtitle: '__Collaborators:__ Jay Hou, Thomas Weng, David Marcano'
author: 
  - name: Will Childs-Klein
  - email: wdc22
---



# Problem 1

## Algorithm

\begin{algorithm}[caption={3CG-Min-Weight-Approx}, label={alg1}, mathescape={true}]
  input: $G = (V,E,w), w: E \rightarrow \mathbb{R}^+$
  init: $c = \emptyset, n = |v|$
  begin
    $\text{sort V such that } \sum\limits_{\mathclap{(u,v_1) \in E}} w(u,v_1) > \space ... \space > \sum\limits_{\mathclap{(u,v_n) \in E}}w(u,v_n)$
    for $i = 1...n$:
      let $p_i = \{(u,i) \in E : \space u \in V\}$
      for $j \in \{1,2,3\}$:
        $\text{cost}_i^j = \sum\limits_{\mathclap{q \in p_i : c(p)=j}} w(p)$
      end for
    $c(i) = \argmin\limits_{j \in \{1,2,3\}} (\text{cost}_i^j)$
    end for
    return c
  end       
\end{algorithm}


## Complexity
$$ O(n + n \log n) $$

### Proof of Complexity
lorem ipsum dolor sit amet


## Correctness

### Theorem 8.1
`3CG-Min-Weight-Approx` will alsays find a color assignment $c$ whose total cost is at most

$$ \frac{1}{3} \sum\limits_{\mathclap{(u,v) \in E}} w(u,v) $$

#### Proof of Theorem 8.1




# Problem 2





# Problem 3





# Problem 4






\begin{algorithm}[caption={<NAME>}, label={alg<i>}, mathescape={true}]
  input:
  init:
  begin
    
  end       
\end{algorithm}