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

# PySpark Implementation:
```
#Taking the document as RDD
>>> rdd = sc.textFile('document.txt')
#Function to count the characters
>>> def char_counts( line):
...     #doesnt count the newline characters [wc does]
...     return len( list(line))
...
>>> rdd.map(lambda line: char_counts(line)).reduce(lambda c1,c2: c1 + c2)
>>>
```