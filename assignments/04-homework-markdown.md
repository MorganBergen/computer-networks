#  cs 664 computer networks

---

homework 4

name:  morgan bergen

wsu id:  b493r546

date:  july 15 2024

due:  july 19 2024

---

1.  consider the following network with the indicated link costs, use dijkstra's shortest-path algorithm, and showing your work using a table similar to our lecture table for dijkstra's shortest path algorithm, compute the shortest path from 't' to all network nodes.

<p align=center>
    <img src="./figure5.1.png">
<p>

-  $c_{x, y}$:  direct link cost from node $x$ to node $y$; $= \infty$ if not direct neighbors
-  $D(v)$:  current estimate of cost of least-cost-path from source to destination $v$
-  $p(v)$:  predecessor node along path from source to $v$
-  $N'$: set of nodes whose least cost path definitively known

```pseudo
initialization:
    N' = {t}
    for all nodes v
        if v adjacent to t
            then D(v) = c_{t, v}
        else D(v) = ∞

loop:
    find w not in N' such that D(w) is a minimum
    add w to N'
    update D(v) for all v adjacent to w and not in N':
        D(v) = min(D(v), D(w) + c_{w, v})
    /* new least path cost to v is either old least cost path to v or known
    least cost path to w plus direct cost from w to v */

    until all node in N'
```

| step | $N'$        | $D(u), p(u)$ | $D(v), p(v)$ | $D(w), p(w)$ | $D(x), p(x)$ | $D(y), p(y)$ | $D(z), p(z)$ |
|:----:|:-----------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|
|  0   | t           | **2,t**      | 4,t          | ∞            | ∞            | 7,t          | ∞            |
|  1   | tu          |  ---------   | **4,t**      | 5,u          | ∞            | ∞            | ∞            |
|  2   | tuv         |  ---------   |  ---------   | **5,u**      | 8,v          | 13,v         | ∞            |
|  3   | tuvw        |  ---------   |  ---------   | ---------    | 

```
update D(b) for all b adjacent to a and not in N'
    D(b) = min(D(b), D(a) + c_{a, b}) 
    D(x) = min(D(x), D(w) + c_{w, x}) = min(8, ? + )
```    


```
update D(b) for all b adjacent to a and not in N'
    D(b) = min(D(b), D(a) + c_{a, b})
    D(w) = min(D(w), D(v) + c_{v, w}) = min(5, 5 + 4) = 5
    D(x) = min(D(x), D(v) + c_{v, x}) = min(∞, 5 + 3) = 8
    D(y) = min(D(y), D(v) + c_{v, y}) = min(∞, 5 + 8) = 13
```

```
update D(b) for all b adjacent to a and not in N'
    D(b) = min(D(b), D(a) + c_{a, b})
    D(v) = min(D(v), D(u) + c_{u, v}) = min(4, 2 + 3) = 4
    D(w) = min(D(w), D(u) + c{u, w}) = min(∞, 2 + 3) = 5
```
    