# Counting Words

This is the 'helloWorld' program of MapReduce.

## Set up:
A massive document is divided into chunks and stored at different data nodes.
## Objective:
To count the number of occurences of each word in the document.

## Solution:
We use MapReduce. The implementation for Map and Reduce is as follows:

## Map:
>Input: (word,word) for each word in the document.

>Output: (word, 1) for each word input received.
## Sort and Shuffle(Performed by MapReduce Framework):
>Input: (word, 1) pairs for all words in the document.

>Output: (word, [1,1,...,1,1]) for each unique word in the document.
## Reduce:
>Input: (word, [1,1,...,1,1]) for each unique word in the document.

>Output: (word, n) for each unique word 'word' occuring 'n' times in the document.

# Variation with Combiners:
