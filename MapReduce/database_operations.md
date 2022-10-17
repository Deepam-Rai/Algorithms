# **Database Operations in MapReduce**
## Notations:
>- R : Relation/Schema.
>- t : A tuple of Relation.
>- s : A subset attributes of the Relation.
>- t<sub>s</sub> : A subset of the relation with only 's' attributes.


<br><br>

# Projection $\pi_s(R)$
> **Projection:** From a given relation/schema R pick out subset attributes S. ~like selecting some columns from a table.
## Map:
>Input: Tuple 't' and subset attributes list 's' at a time.
```
Map( (t,s))
{
    ts <- subset of 't' containing only 's' attributes
    Output( (ts,ts))
}
```
>Output: (t<sub>s</sub>, t<sub>s</sub>)
## Reduce:
>Input: ( t<sub>s</sub>, [t<sub>s</sub>,t<sub>s</sub>,..., t<sub>s</sub>, ])  
Assuming that there can be duplicate tuples in the relation.
```
Reduce( (ts, [ts, ts, ..., ts]))
{
    Output( (ts,ts))
}
```
>Output: (t<sub>s</sub>, t<sub>s</sub>)

<br><br>

# Selection $\sigma_R(c)$, c is some condition.
> **Selection:** From a relation R, pick only those tuples that satisfy condition $c$. ~like selecting rows, based on condition, from a table.

## Map:
>Input: Tuple 't' and condition 'c'.
```
Map( (t,c))
{
    if (t satisfies condition c)
    {
        Output( (t,t))
    }
    else
    {
        Output( (None,None)) #Map HAS to produce (key,value) output
    }
}
```
>Output: (t,t) such that 't' satisfies condition c.
## Reduce:
>Input: (t, [t,t,...,t])  
Assuming that duplicate tuples are allowed in a Relation.
```
Reduce( (t, [t,t,...,t]))
{
    Output( (t,t))
}
```
>Output: (t,t) where tuple 't' satisfies condition c.

<br><br>

# Union $\cup(A,B)$
>**Union:** For two relation $A$ and $B$ having identical attributes, get the union of their values. ~like appending a table to another table with identical columns.

## Map:
>Input: Tuple 't' and relation name R.
```
Map(t,R)
{
    Output( (t,(t,R)))
}
```
>Output: (t,R) for every tuple 't' of both relation A or B.
## Reduce:
>Input: (t, [ (t,A),(t,A),...as many duplicate 't' in A, (t,B),(t,B),...as many duplicate 't' in B])  
Assuming that duplicate tuples are allowed in a relation.
```
Reduce( (t, [ (t,A),(t,A),...,(t,A), (t,B),(t,B),...,(t,B)]))
{
    #For each pair of 't' from A and B there will be one pair in output.
    #Any extra 't' in A or B, it will be put just as it is.
    
    countA = count of (t,A) in input value-list
    countB = count of (t,B) in input value-list

    for i in range(1 to  max(countA, countB))
    {
        Output( (t,t))
    }
}
```
>Output: (t,t) where collection of all (t,t) forms A $\cup$ B.

<br><br>

# Intersection $\cap(A,B)$
>**Intersection:** For two relation $A$ and $B$ having identical attributes, get the intersection of their values. ~like taking values that are common in two tables.

## Map:
>Input: Tuple 't' and relation R.
```
Map -> Identity
```
>Output: (t,R) for every tuple 't' of both A and B.
## Reduce:
>Input: (t, [A,A,...,A,B,B,...,B])  
Assuming that duplicate tuples are allowed in a relation.
```
Reduce( (t,[A,A,...,A,B,B,...,B]))
{
    countA = count of 'A' in value-list
    countB = count of 'B' in value-list
    if (countA>0 and countB>0)  #if tuple 't' exists in both A and B
    {
        for i in range(1 to min(countA,countB))
        {
            Output( (t,t))
        }
    }
}
```
>Output: (t,t) where collection of tuples (t,t) will make A $\cap$ B.

<br><br>

# Difference $\sim(A,B)$
> **Difference:** Take the tuples in $A$ which are not present in $B$. ~like (A-B)
## Map:
>Input: Tuple 't' and relation R.
```
Map-> Identity
```
>Output: (t,R) for every tuple 't' of both A and B.
## Reduce:
>Input: (t, [A,A,...,A,B,B,...,B])  
Assuming that duplicate tuples are allowed in a relation.
```
Reduce( (t,[A,A,...,A,B,B,...,B]))
{
    countA = count of 'A' in value-list
    countB = count of 'B' in value-list
    if (countA > countB)
    {
        for i in range(1 to (countA-countB))
        {
            Output( (t,t))
        }
    }
}
```
>Output: (t,t) where collection of all (t,t) forms $\sim(A,B)$.

<br><br>

# Grouping and Aggregation $\gamma_{s,f(s)}$

<br><br>

# Natural Join $\Join$