# KNN (K Nearest Neighbours)

- It is a classification technique for multi-class classification problem.
- It approaches the problem from geometrical point of view, thus by core it is a non-probabilistic technique.

## Hyperparameter:
1. $k$: number of neighbours to consider while computing the class of a node.
> It is suggested to NOT keep $k$ as multiple of number of classes in the dataset. If done so the count of classes in neighbours may tie, leaving algorithm in dilemma in choosing label.
>> e.g, For binary classification if $k$ be given 8 and if 4 of the neighbours have one class while other four have another class then ambiguity occurs in classifying new datapoint.


# Algorithm:
> $X$: Collection of train datasets, excluding their labels.

> $y$: List of labels for corresponding elements in $X$.  

> $X$: New datapoint whose class is to be determined.
```
1. Calculate distances between $x$ and all datapoints in the train dataset.
2. Sort the distances in ascending order and take first $k$ of them to get k-nearest neighbours of $x$.
3. Count the classes of these $k$ nearest neighbours are counted.
4. Determine the class with the highest frequency, and that is considered as the class of $x$. 
Note: Tie in frequencies are not expected to occur as $k$ is not kept as multiple of the classes in the dataset.
```

# Implementation
## Python:
Implementation as a class using ```numpy``` package:
```Python
import numpy as np

class KNN():
    '''
    Implementing KNN classification algorithm using numpy.
    X: list of observations excluding their class column.
    y: list of target variables; Classes of respective datapoints in X.
    k: Hyperparameter k; number of neighbours to consider.
    '''
    def __init__(self,k, X=None,y=None):
        self.X = np.array(X)
        self.y = np.array(y)
        if self.X.shape[0] != self.y.shape[0]:
            print("Error: The length of X and y is expected to be equal.")
            return None
        self.classes = np.unique(self.y) #unique classes
        if k<1:
            print("Error: k should be greater than 0.")
            return None
        elif  k%len(self.classes) == 0:
            print("Error: k should not be a multiple of number of classes in the dataset.")
        else:
            self.k=k
    
    def __predict(self,x):
        '''
        Arguments:
        y: new datapoint whose class id to be determined, based upon previously supplied X.
        Returns:
        Numerical class label for y.
        '''
        distances = self.__get_distances(x) #gets distance of x from all points in X
        neighbours = self.__get_K_neighbours(distances) #returns 'indexes' of k nearest neighbours
        labels = self.__get_labels(neighbours) #using the 'indexes' returns their labels.
        #return the most frequent label
        unique, indexes = np.unique(labels, return_counts=True)
        return unique[indexes.argmax()]
    
    def predict(self, Xtest):
        '''Similar to self.__predict() but can work for multiple datapoints.'''
        ypred = []
        for x in Xtest:
            ypred.append(self.__predict(x))
        return ypred
    
    def __get_K_neighbours(self,distances):
        '''Arguments:
        distances: 1D numpy array of distances of point x from datapoints in X, at the corresponding indexes.
        Returns:
        kx1 numpy array of 'indexes' of the distances sorted in ascending order.
        '''
        return np.argsort(distances)[:self.k]
    
    def __get_labels(self,neighbours):
        '''Arguments:
        neighbours: 1D numpy array of 'index'es for points in X.
        Returns:
        labels for the neighbours.
        '''
        return np.array([self.y[index] for index in neighbours])
        
    
    def __get_distances(self, x):
        '''
        Arguments:
        x: The point for which distance is to be calculated.
        Returns:
        The list of distance of the point from all the points in X, following the same indexing as X.'''
        distances = []
        for p in self.X:
            d =np.array(p) - np.array(x)
            d = d*d
            distances.append( np.sqrt(d.sum()))
        return distances
            
    def setK(self,k):
        '''
        Parameter k should be greater than 0.
        Returns True if k is set else False.
        '''
        if  k%len(self.classes) == 0:
            print("Error: k should not be a multiple of number of classes in the dataset.")
        if k<1:
            print("Error: k should be greater than 0.")
        else:
            self.k = k
            return True
        return False
    
    def getK(self,k):
        return self.k
    def getX(self):
        return self.X
    def gety(self):
        return self.y
    def getClasses(self):
        return self.classes
```