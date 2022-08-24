# Matrix Matrix Multiplication - MapReduce
>Matrix $A$ = $$\begin{bmatrix}
a_{11} & a_{12} & ... & a_{1n}  \\ 
a_{21} & a_{22} & ... & a_{2n}  \\ 
.&.&&. \\ 
.&&.&.  \\ 
a_{m1} & a_{m2} & ... & a_{mn}  
\end{bmatrix}$$
$a_{ik}$ denotes element at i'th row and k'th column.

>Matrix $B$ = $$\begin{bmatrix}
b_{11} & b_{12} & ... & b_{1p}  \\ 
b_{21} & b_{22} & ... & b_{2p}  \\ 
.&.&&. \\ 
.&&.&.  \\ 
b_{n1} & b_{n2} & ... & b_{np}  
\end{bmatrix}$$
$b_{kj}$ denotes element at k'th row and j'th column.

>Let the product matrix be $C =$ $$\begin{bmatrix}
c_{11} & c_{12} & ... & c_{1m}  \\ 
c_{21} & c_{22} & ... & c_{2m}  \\ 
.&.&&. \\ 
.&&.&.  \\ 
c_{m1} & c_{m2} & ... & c_{mp}  
\end{bmatrix}$$
$c_{ij}$ denotes element at i'th row and j'th column.  <br><br>
$\Large c_{ij}= \sum_{k=1}^n a_{ik}*b_{kj}$ for $1\le i\le m;$  $1\le j\le p;$ 

<br><br>

# Solution 1: Two Phase Map-Reduce
## Approaching the problem:
1. At a time we will get $A_{ik}$ or $B_{kj}$.
1. From $ c_{ij}= \sum_{k=1}^n a_{ik}*b_{kj}$ we know that i'th row of $A$ dot products with j'th column of $B$ to get $C_{ij}$.
1. And in doing so we note that element of i'th column of $A$ multiplies with each element of i'th row of $B$, of course each multiplication is for the different elements of $C$.
1. But the multiplication of k'th element of i'th column of $A$ and k'th element of j'th row of $B$ is only-and-only for $C_{ij}$.
1. And if we consider all $n$ columns of $A$ and $n$ rows of $B$ then summation of multiplication of such $k'th$ elements will give us $C_{ij}$.
1. Thus now we can draft plan as following:
- In map when we get such k'th elements - $A_{ik}$ or $B_{kj}$ we will just signify that, that element is to be multiplied with k'th counterpart's' of another matrix.
- In reduce we will just multiply those elements as told by map.
- But since we still need to sum appropriate products, work is still left.
- For that we will use another Map-Reduce.
- Since we know that $i$ from $A_{ik}$ and $j$ from $B_{kj}$ indicates to us that that particular product $A_{ik}*B_{kj}$ belongs to $C_{ij}$, we can pass this $i$ and $j$ down the line and at last based upon that add appropriate terms.
- But in map we can only receive one term, thus no possibility of adding items, thus Map in second phase will remain identity.
- In reduce we will add all those appropriate products.

## Phase-1: Just Multiplications
## Map-1
>Input: $('A',i,k, A_{ik})$ or $('A',i,k, A_{ik})$ where  $'A'$ or $'B'$ is just to indicate if it comes from $A$ or $B$.
```
```
>Output: $(k,('A',i))$ or $(k,('B',j))$ for respective input.
## Reduce-1
>Input: $(k, [('A',1),('A',2)...,('A',n),('B',1),('B',2),...,('B',n)])$
```
```
>Outputs: $((i,j),P_{ikj})$ where $P_{ikj}=A_{ik}*B_{kj}$
## Phase-2: Just Additions
## Map-2
>Identity: $Input=>Output$
## Reduce-2
>Input: $((i,j),[P_{i1j},P_{i2j},...,P_{inj}])$ where $P_{ikj}=A_{ik}*B_{kj}$
```
```
>Output: $( (i,j),C_{ij})$