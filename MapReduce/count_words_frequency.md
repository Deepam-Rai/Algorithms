# **Counting The Occurrences of Words**

This is the 'helloWorld' program of MapReduce.

## **Set up:**
A massive document is divided into chunks and stored at different data nodes.
## **Objective:**
To count the number of occurrences of each word in the document.

## **Solution:**
### Approaching the problem:
1. A word may occur multiple times in the document and thus may be present at multiple chunks. We need to collect the number of occurrences from each chunk.
2. The sort and shuffle phase collects all the values, from across the Map function output, for a specific key and pairs it with that key, and it does this for all unique keys.
3. Thus if we make the words themselves as key in the Map output with their number of occurrence as paired value then sort and shuffle will collect all number of occurrences of that key and pair it with that word and provide to reduce phase.
4. Finally in the reduce phase we just need to add the number of occurrences.

 The implementation for Map and Reduce is as follows:

<br>

## Map:

>Input: (a line from the chunk) at a time.  

```
Map Algorithm:

Map( (chunk,chunk))
{
    for each line in chunk:
    {
        MyMap( (line,line))
    }
}

MyMap( (line,line))
{
    for each word in the line:
    {
        Output(word,1)  #this word appeared 1 time.
    }
}

```
>Output: (word, 1) for each word input received.

Thats it! After this implicitly Framework hashes each output into appropriate intermediate file.

<br>

## Sort and Shuffle(Performed by MapReduce Framework):
The below input comes from all intermediate files of all Map tasks.  
>Input: (word, 1) pairs for all words in the document.

Here for each unique word following is done:
1. Get all the values for that unique word from the intermediate files from all Map tasks.
2. Create (word, [1,1,...,1]) pair and provide to Reduce function.
>Output: (word, [1,1,...,1,1]) for each unique word in the document and append it into reduce input file.

<br>

## Reduce:
>Input: (reduce_input_file) created after sort and shuffle.

```
Reduce Algorithm:

Reduce( (reduce_input_file, reduce_input_file))
{
    for each line in reduce_input_file:
    {
        #Here each line is ( word, [1,1,...,1]) pair
        MyReduce( (word, list_of_values))
    }
}

MyReduce( (word, list_of_values))
{
    number_of_occurrences = 0
    for value in list_of_values:
    {
        number_of_occurrences += value
    }
    Output( (word, number_of_occurrences))
}
```
>Output: (word, n) for each unique word 'word' occurring 'n' times in the document.

The final output is present in parts in multiple reduce nodes. Master node aggregates all of them into a single file.

# Variation with Combiners:
**Inefficiency in above algorithm:** A given word can occur n>1 times in a chunk. As per above algorithm the Map task will produce (word,1) pair n number of times. And these n pairs might be communicated to other compute nodes when needed. But the same task can be done by 
>(word,n) instead of (word,1) n times.

This is where combiners come in. The input to the combiner is a local intermediate file. 
We can set the following combiner algorithm to each Map task:
```
Combiners Algorithm:

Combiner(intermediate_file)
{
    for each unique_word in the (word, 1) pairs:
    {
        n = Sum all 1s
        Output(word, n)
    }
}
```

Now for sort and shuffle phase following change will appear:
>Input: (word,n) for all words in a document.

Note that still there can be multiple (word,n1), (word,n2),... pairs for the same 'word' because our combiner just combined the output of one single map task. We still need to combine those n1,n2,... to get total number of occurrences of that corresponding 'word'.
>Output: (word, [n1, n2, ..., as many as the number of chunks containing that word])

And for Reduce algorithm input will change but the algorithm itself and the output wil remain unchanged:
>Input: (word, [n1, n2, ..., as many as the number of chunks containing that word]) for each unique word 'word' occurring 'n' times in the document.

>Output: (word,n) for each unique word 'word' occurring 'n' times in the document.

>>Note that above combiner operation was possible only because the operation(Addition) that we do in the Reduce function is Associative and Commutative in nature.

<br><br>

# Implementation
## Pyspark
```
>>> rdd = sc.textFile('path/document.txt')
>>> rdd.flatMap(lambda line: line.split()).map(lambda word: (word,1)).reduceByKey(lambda v1, v2: v1+v2).collect()
```
Explanation:
1. ```rdd.flatMap(lambda line: line.split())```: Map gets a line which is splits into words.
2. ```.map(lambda word: (word,1))```: Makes pair (word,1) so that counting can be done next.
3. ```.reduceByKey(lambda v1, v2: v1+v2)```: For each unique word add their values, which eventually gives us the frequency of that word in the document.
4. ```.collect()```: Do not use this for large documents, instead store the output in another variable. 