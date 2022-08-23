# **Count of Characters in a Document - MapReduce**

## Set up:
A document is stored across $'m'$ chunks in DFS. We need to find the total number of characters in that document.

## Approaching the Problem:
1. Each map receives a chunk at a time.
2. We work upon a line from the chunk, at a time.
3. For each line we will output count of characters in it paired with a fixed_key. [fixed_key is just a value fixed across Maps and Reduce functions.]
4. Then combiner will be used to add the count of characters in a chunk.
5. Sort and shuffle will gather all the counts from different chunk, since the key is fixed across chunks, and provide to a reduce.
6. We will add the counts to get total count of characters in the document in reduce function.
# Map:
>Input: (chunk, chunk)
```
Map( (chunk,chunk))
{
    for each line in chunk:
        MyMap( (line,1))
}
MyMap( (line,1))
{
    count = number of characters in the line
    Output( (fixed_key,count))
}
```
>Output: (fixed_key, count) for each line in the chunk.
# Combiner:
>Input: (fixed_key, [count1, count2,..., countn for $n$ lines in the chunk]) and this function runs separate for each chunk.
```
Combiner( (fixed_key, [count1, count2,..., countn for n lines in the chunk]))
{
    Output( (fixed_key, count1+count2+...+countn))
}
```
>Output: (fixed_key, count) where $'count'$ is the count of character in that respective chunk.
# Reduce:
>Input: (fixed_key, [count1,count2,...,countm for $m$ chunks]) here counts of all $'m'$ chunks would come to a single reduce because of fixed_key.
```
Reduce((fixed_key, [count1,count2,...,countm for m chunks]))
{
    Output( ("total_count", count1+count2+...+countm))
}
```

>Output: ("total_count", count) where $'count'$ is the count of number of characters in the document.

<br><br>

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