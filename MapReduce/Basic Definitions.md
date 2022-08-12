**Distributed File System(DFS):** is a collection of large number of interlinked which are commodity hardwares, called compute nodes which are independent of each other and are used for storage, computations, etc.

**Chunks:** are the basic units of storage in DFS.
 - The size of the chunks can be specified by the user, typcial chunk size in Hadoop DFS is 64MB.
 - A file can be stored in multiple chunks.
 - In case the data size is smaller than the chunk size, still the whole chunk is used to store that data.
 - They are replicated at multiple compute nodes to increase the reliability of the system.
 - The organization of the chunks, their metadata is handled by the Name Node.
 - The compute nodes where the chunks are present are called Data Nodes.

**MapReduce:** is a framework( and independent software infrastructure ), that allows us to harvest the benefits of DFS for herculent data computations by means of parallelism.

**Map:** refers to the task that is performed on each chunk.

**Combiners:** are the task run at the output of each Map task.
 - They are helpful if the operation performed at Reduce task is Associative and Commutative in nature, as then we can perform part of that task at each Map task running node itself, thus reducing the workload for Reduce task and also benefitting parallelism(combiners running paralley at different nodes).


**(Key, Value) Pair:** is the output of each Map task, where both key and value can be of any data-type.

**Sort and Shuffle:** refers to the phase inbetween Map and Reduce and it sorts all the keys and assigns to each their values as a list.

**Reduce:** refers to the task that if performed on the list of values for a specific key.