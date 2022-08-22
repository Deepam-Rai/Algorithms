# Bubble Sort
## Notations
>$list:$ list of items
>- Indexing starts from $0$.   
>- $list[i]$ indexes $i'th$ element of the list.  
>- $n$ = number of items in the list.
```
Bubble Sort(list)
{
    #Sorts in ascending order
    #in-place sorting
    for i in range(0, (length(list)-1))
    {
        changed = False  #change in the i'th iteration
        for j in range(0, n-i-1)
        {
            if ( list[j]>list[j+1])   #'>'Sorts in ascending order
            {
                swap list[j] and list[j+1]
                changed = True
            }
        }
        if (changed == False)
        {
            exit    #List is sorted.
        }
    }
}
```

# Time Complexity
```
    for i in range(0, (length(list)-1)) ---O(n)
    {
        .
        ---------------------------O(1)
        .
        for j in range(0, n-i-1) --O(i); n+(n-1)+...+1;
        {
            .
            -----------------------O(1)
            .
        }
        .
        ---------------------------O(1)
        .
    }
```
> 1. **Worst Case**$= O(n*(n+1)/2) = O(n^2)$  
>> - _The list is oppositely ordered; here descending._
>> - The outer loop will run $n$ times.
>> - The inner loop will run $(n-i)$ times in $i'th$ iteration.
> 2. **Average case** $= O(n^2)$  
> 3. **Best Case** $= O(n)$
>> - _The list is already sorted._
>> - The outer loop will run $1$ time.  
>> - The inner loop will run $n$ times.
# Space Complexity
> $O(1)$
>> The space used is fixed; isn't dynamic.

<br><br>

# Implementations
### C
```C
void BubbleSort(int* arr, int n)
{
    for(int i=0; i<n-1; i++)
    {
        bool changed = false;
        for(int j=0; j<n-i-1; j++)
        {
            if( arr[j]>arr[j+1])
            {
                int tmp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = tmp;
                changed = true;
            }
        }
        if( changed != true )
        {
            break;  //exit out of the 'outer loop'
        }
    }
}
```
<br><br>

# Variations
<!-- #TODO Add variations. -->
# Advantages
- Its an in-place sorting algorithm.
- Just one iteration is done for a sorted list.
# Disadvantages
- There are better algorithms with time complexity lesser than $O(n^2)$.

# Points
- Worst Time complexity $O(n^2)$ occurs when the list is sorted in opposite order.
- Performing sorting on pointers to the data is more efficient than sorting data itself, as swapping pointer values is lighter.
