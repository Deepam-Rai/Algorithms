# **Count the number of Characters in the Document**

# **Set up:**

# **Objective:**

# **Solution:**
## **Approaching the Problem:**

# **Map:**
>Input: (word, word) for each word in the chunk.

>Output: (c,1) for each character in the chunk
# **Combiner:**

# **Reduce:**
>Input:

>Output:

# Implementation:

## Pyspark:
```
#Taking the document as RDD
>>> rdd = sc.textFile('document.txt')
>>> rdd.map( lambda line: len(line.split())).reduce(lambda c1,c2: c1 + c2)
>>>
```
Or more neatly
```
>>> rdd.map( lambda line: len(line.split())).sum()
```