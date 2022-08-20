# Bubble Sort
## Notations
>list: list of items
>- Indexing starts from 0.   
>- list[i] indexes i'th element of the list  
>- $n$ = number of items in the list
```
Bubble Sort(list)
{
    #Sorts in ascending order
    for i in range(0, (length(list)-1))
    {
        changed = False  #to denote if any change occurred in the i'th iteration
        for j in range(0, i)
        {
            if ( list[i]>list[j])   #'>'Sorts in ascending order
            {
                #swap(list[i], list[j])
                tmp = list[i]
                list[i] = list[j]
                list[j] = tmp
                changed = True
            }
        }
        if (changed == False)
        {
            exit    #The sorting was done.
        }
    }
}
```

# Time Complexity
```
    for i in range(0, (length(list)-1)) ---O(n)
    {
        .
        -------------------------------O(1)
        .
        for j in range(0, i)    -------O(n)
        {
            .
            ---------------------------O(1)
            .
        }
        .
        -------------------------------O(1)
        .
    }
```
> 1. **Worst Case**$= O(n*n) = O(n^2)$  
>> - _The list is oppositely ordered; here descending._
>> - The outer loop will run $n$ times.
>> - The inner loop will run $i'th$ times in i'th iteration.
> 2. **Average case** $= O(n^2)$  
> 3. **Best Case** $= O(n)$
>> - _The list is already sorted._
>> - The outer loop will run 1 time.  
>> - The inner loop will run $n$ times.
>> - The if-else check with 'changed' flag will end the algorithm.
# Space Complexity
> $O(1)$
>> The space used is fixed and isn't dynamic.

<br><br>

# Implementations
### C
```
void BubbleSort(int* arr, int n)
{
    //n = number of elements in the array arr
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
