# **Counting the most frequent word in the Document**

# **Set up:**
- The document is stored in 'n' chunks.
- Number of Map tasks run = n

# **Solution:**

## **Approaching the problem:**
1. First we need to compute the frequency of each word in the document.
2. Next, we need to compare the frequencies.
3. An easy solution is using two Map-Reduce tasks in sequence.  
    1. **Fist Map-Reduce:**  
        **Map:** will pair (word, 1) for each word in the sentence.  
        **Shuffle:** will collect all [1,1,..,1] values from all Map tasks output for us and make a (word, [1,1,..,1]) pair for us.  
        **Reduce:** will add the 1s and we will get the frequency of each word in the document.

    2. **Second Map-Reduce:**  
        **Map:** Because we want to compare the frequency of all the words, we will send all words and their frequencies to a single Reduce function. To do that we make pairs as ('fixed_key_for_all_word', (word, frequency)).  
        **Shuffle:** Because there is just one key - 'fixed_key_for_all_word' the shuffle will result in just one single pair as: ('fixed_key_for_all_word', [ (word1,f1), (word2,f2)... as many as there are unique words in the document]).  
        **Reduce:** Because all words and frequency is coming to a single reduce function at once, we can compare them easily. Though this also puts load on just one single reduce in this second phase.

# **First Phase:**
## **Map_First:**
>Input: (line, line) for each line in the chunk.
```
Map( (chunk,chunk))
{
    for each line in the chunk:
    {
        MyMap( (line, line))
    }
}
MyMap( (line,line))
{
    for each word in the line:
    {
        Output( (word,1))
    }
}
```
>Output: (word,1) for each word in the line given.
## **Reduce_Reduce:**
>Input: (word, [1,1,..,1]) for each unique word in the document.
```
Reduce( (word, list_of_values))
{
    frequency = 0
    for each value in list_of_values:
    {
        frequency += value
    }
    Output( (word,frequency))
}
```
>Output: (word,frequency) for a 'word' appearing 'frequency' many times in the document.

# **Second Phase:**
## **Map_Second:**
>Input: (word,n) for each unique word in the document.
```
Map( (word,n))
{
    Output( 'fixed_key', (word,n))
}
```
>Output: (fixed_key, (word,n)) same fixed_key is used for all words.

## **Reduce_Second:**
>Input: ( fixed_key, [(word1,f1),(word2,f2),... as many as unique words in the document])
```
Reduce( (fixed_key,list_of_values))
{
    #Here each element in list_of_values is (word,frequency) for each unique word in the document.

    sorted_list =  sorted list_of_values by value.frequency in descending order

    (most_frequent_word, its_frequency) = first element of sorted_list
    
    Output( (most_frequent_word, its_frequency))
}
```
>Output: (word,f) where 'word' is the most frequent word in the document appearing 'f' times.