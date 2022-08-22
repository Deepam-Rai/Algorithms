# Counting most frequent character - MapReduce

# Implementation

## Pyspark:
```
>>> rdd = sc.textFile('path/document.txt')
>>> rdd.flatMap(lambda line: line.split()).flatMap(lambda word: list(word)).map(lambda char: (char,1)).reduceByKey(lambda v1, v2: v1+v2).sortBy(lambda pair: pair[1], ascending=False).first()
```
Explanation:
1. ```rdd.flatMap(lambda line: line.split())```: Map gets a line of document as input which it splits into words.
2. ```.map(lambda word: list(word))```: This map gets words as input which it converts into a list of characters.
3. ```.map(lambda char: (char,1))```: Makes (char,1) tuples so that later we can count char frequency.
4. ```.reduceByKey(lambda v1,v2: v1+v2)```: performs for each unique character(since char is set as key in above map) and adds their values, which eventually gives us total occurrence of that character in the document.
5. ```.sortBy(lambda pair: pair[1], ascending=False)```: From above map we get (char,frequency) pair which is now sorted on frequency in descending order.
6. ```.first()```: The most occurring character is sorted to the top.