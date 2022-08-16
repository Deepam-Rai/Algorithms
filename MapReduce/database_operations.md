# **Database Operations in MapReduce**

# Projection $\pi_S(R)$
> From a given relation/schema R pick out subset attributes S. ~like selecting some columns from a table.
# Selection $\sigma_R(c)$, c is some condition.
> From a relation R, pick only those tuples that satisfy condition $c$. ~like selecting rows, based on condition, from a table.
# Union $\cup(R,S)$
>For two relation $R$ and $S$ having identical attributes, get the union of their values. ~like appending a table to another table with identical columns.
# Intersection $\cap(R,S)$
>For two relation $R$ and $S$ having identical attributes, get the intersection of their values. ~like taking values that are common in two tables.
# Difference $\sim(R,S)$
> Take the tuples in $R$ which are not present in $S$.
# Grouping and Aggregation $\gamma_{s,f(s)}$

# Natural Join $\Join$