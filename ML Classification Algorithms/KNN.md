# KNN (K Nearest Neighbours)

- It is a classification technique for multi-class classification problem.
- It can be used for regression also.
- It approaches the problem from geometrical point of view, thus by core it is a non-probabilistic technique.

## Hyperparameter:
1. $k$: number of neighbours to consider while computing the class of a node.
> For classification it is suggested to NOT keep $k$ as multiple of number of classes in the dataset. If done so the count of classes in neighbours may tie, leaving algorithm in dilemma in choosing label.
>> e.g, For binary classification if $k$ be given 8 and if 4 of the neighbours have one class while other four have another class then ambiguity occurs in classifying new datapoint.


# Algorithm:
> $X$: Collection of train datasets, excluding their labels.

> $y$: List of labels for corresponding elements in $X$.  

> $X$: New datapoint whose class is to be determined.
```
1. Calculate distances between $x$ and all datapoints in the train dataset.
2. Sort the distances in ascending order and take first $k$ of them to get k-nearest neighbours of $x$.
3. Count the classes of these $k$ nearest neighbours.
4.  
    IF classification:
        Determine the class with the highest frequency, and that is considered as the class of $x$. 
        Note: Tie in frequencies are not expected to occur as $k$ is not kept as multiple of the classes in the dataset.
    ELSE IF regression:
        Return the mean of class values in k nearest neighbours.
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
    y: list of target variables; numerical datatype; Classes of respective datapoints in X.
    k: Hyperparameter k; number of neighbours to consider.
    regression: False->Returns most frequent class-label; True: Returns the mean of labels.
    '''
    def __init__(self,k, X=None,y=None, regression=False):
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
        self.regression= regression
    
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
        if self.regression is False:
            #return the most frequent label
            unique, indexes = np.unique(labels, return_counts=True)
            pred = unique[indexes.argmax()]
        else:
            #return the mean of labels
            pred = labels.mean()
        return pred
    
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
### Use case (.ipynb - IPython Notebook):
```Python
import matplotlib.pyplot as plt
import seaborn as sns

# Dataset
X = np.array([[1,4,0],[2,2,0],[1,2,0],[2,3,0],[3,4,0],[3,5,0], [3.5,6,0], [2,7,0],[4,3,0],
    [5,5,1],[6,7,1], [6,5,1],[7,6,1],[7,4,1],[7,8,1],[8,6,1],[6,8,1]])
# Visualizing Dataset
MAXX = MAXY = 11
sns.scatterplot(x=X[:,0], y= X[:,1], hue=X[:,2])
plt.axis(xmin=0,xmax=MAXX, ymin=0,ymax=MAXY)
plt.xticks( np.arange(MAXX,step=1))
plt.yticks( np.arange(MAXY,step=1))
plt.show()

#Classification
KModel = KNN(X=X[:,:2], y=X[:,2],k=5, regression=False) #Creating the model
predicted_classes = KModel.predict([[5,6],[0,0],[10,10]])

#Regression
KModel = KNN(X=X[:,:2], y=X[:,2],k=5, regression=True) #Creating the model
predicted_values = KModel.predict([[5,6],[0,0],[10,10]])
```