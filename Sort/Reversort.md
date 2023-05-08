
# Reversort Algorithm
Assumptions;
1. The elements in the list L are unique.

```
Input: L := List of unique numbers.

Reversort(L):
	for i := 0 to (length(L)-1)
	do
		j := Index of minimum element in L[i:end], inclusive
		Reversort(L[i:j])
	endfor
```

# Analysis

Cost:  
- Iterating over an array of n length => cost n
- Reversing an array of length n => cost n

Cases:
- Best Case: $O(n)$ ; Already sorted list
- Worst Case: $n+(n-1)+(n-2)+...+(2)\approx O(n^2)$
	- eg: \[2,4,5,3,1]

# Implementations
```c++
int reversort(vector<int>& L){
    /*Input: An array L with unique elements.
    Output: Cost of sorting. [Sorted L in increasing order inplace].
    Method: Reverse-sort; recursive calls.*/
    
    int cost=0;
    /* Iterating over an array of n length => cost n
    Reversing an array of length n => cost n*/

    int n = L.size(); //length of the vector

    for(int i=0; i<n-1; i++){

        //get the minimum element
        int min = *min_element(L.begin()+i, L.end()); //in subsequence[i,n]
        
        //get the index of the minimum element
        auto min_idx = find(L.begin()+i, L.end(), min); //ASSUMING ALL ARE UNIQUE ELEMENTS;

        //reverse the subsequence
        reverse(L.begin()+i, min_idx+1);

        cost += distance(L.begin(),min_idx) - i +1;
    }
    return cost;
}
```
