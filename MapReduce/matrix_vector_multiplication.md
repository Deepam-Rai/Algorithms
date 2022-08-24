# Matrix Vector Multiplication

>Matrix $A$ = $$\begin{bmatrix}
a_{11} & a_{12} & ... & a_{1n}  \\ 
a_{21} & a_{22} & ... & a_{2n}  \\ 
.&.&&. \\ 
.&&.&.  \\ 
a_{m1} & a_{m2} & ... & a_{mn}  
\end{bmatrix}$$
$a_{ij}$ denotes element at i'th row and j'th column.

>Vector $x$ =$$\begin{bmatrix} 
x_1 \\ x_2 \\ .\\.\\. \\ x_n
\end{bmatrix}$$
$x_j$ denotes j'th element of x.

>Vector b = $$\begin{bmatrix}
b_1 \\ b_2 \\ .\\.\\. \\ b_n
\end{bmatrix}$$ 
$b_j$ denotes j'th element of b.

>For a given matrix $A$ and vector  $x$ we will calculate $b$ as $Ax=b$

> We note that  
$\large b_j = A_{i1}*x_1 + A_{i2}*x_2 + .. + A_{in}*x_n =\sum_{j=1}^{n}{A_{ij}x_j}$

> Way of storage of matrix $A$ in DFS:  
(1,1,A<sub>11</sub>)  
(1,2,A<sub>12</sub>)  
.  
.    
(i,j,A<sub>ij</sub>)  
.  
.  
Thus at a time we wil get triplet (i,j,A<sub>ij</sub>) - one element of the matrix.
# Case 1: Vector x fits in main memory
Thus for every task we can have vector $x$ readily available in local main memory of compute nodes.
## Approaching the problem:
1. We need to do:  
$\large b_j = A_{i1}*x_1 + A_{i2}*x_2 + .. + A_{in}*x_n$
2. In map when we get the $A_{ij}$ we have $x_j$ to multiply that with in main memory.
3. Further we know that the above product is part of $b_j$. But at a time we will only have one product $A_{ij}*x_j$ with us in a map function.
4. And such products we need to sum up to get $b_j$,now to sum up all of them we can send all of them to one reduce function. And to do so we should use same key for all of them - in this case $j$ because it forms part of $b_j$.
5. Thus in map we just multiply them and set up the key.
6. Sort and shuffle will collect the required pairs for us.
7. In reduce we just need to add them up to get $b_j$.

## Map:
>Input: $(i,j,A_{ij})$ 
```
Map( (i,j,Aij))
{
    Output( (j, Aij * xj))
}
```
>Output: $(j, A_{ij}*x_j)$

## Reduce:
>Input: $( j, [A_{j1}*x_1, A_{j2}*x_2, ..., A_{jn}*x_n])$
```
Reduce(( j, [Aj1*x1, Aj2*x2, ..., Ajn*xn]))
{
    bj=0
    for value in list_of_values:
    {
        bj = bj + value
    }
    Output( (j,bj))
}
```
>Output: $(j, b_j)$
# Case 2: Vector x doesn't fit in main memory
## Solution:
1. Partition the vector $x$ into $n$ parts $x^{(1)}, x^{(2)},...,x^{(n)}$ and at a time we will fetch part of $x$ in our main memory.  
2. Partition $A$ also into same number of partitions _vertically_ - $A^{(1)},A^{(2)},...,A^{(n)}$ .
>$x^{(k)}$ and $A^{(k)}$ represents the $k'th$ partition of $x$ and $A$ respectively.
3. If we get $A^{(k)}$ and $x^{(k)}$ together then for them the same map and reduce functions from Case-1 can be used.
