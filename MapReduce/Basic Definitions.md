# Basic Definitions:

### **Distributed File System(DFS):**  
 is a cluster of commodity hardwares, termed compute nodes that are used for storing data in distributed fashion.
 - Implementation examples: Google File System(GFS), Hadoop Distributed File System(HDFS).

### **Elements:**
an element is considered as the fundamental unit of files and no element is stored across multiple chunks. They can by of any data-type.
### **Chunks:**
 is a collection of elemtns and are considered as the basic units of storage in DFS.
 - The size of the chunks can be specified by the user, typcial chunk size in Hadoop DFS is 64MB.
 - A file can be stored in multiple chunks.
 - In case the data size is smaller than the chunk size, still the whole chunk is used to store that data.
 - They are replicated at multiple compute nodes to increase the reliability of the system, the compute nodes holding replicas should be in different racks.
 - The organization of the chunks, their metadata is handled by the Name Node.
 - The compute nodes where the chunks are present are called Data Nodes.

### **MapReduce:**
 is a framework( and independent software infrastructure ), using parallel processing technique using which we can perform distributed computing on massive data stored in DFS.
 - Implementation examples: Google's MapReduce, Apache's Hadoop, etc.

### **Map:**
 refers to the task that is performed on each chunk and produces a (key, value) pair as output.
  - Input to it is a (key, value) pair and so is their output(this favors the composition of several Map-Reduce tasks).
  - Users implement this function.
  - Number of map tasks is typically equal to number of chunks upon which task is to be run.
  - Multiple map tasks can be run simultaneously on one node.

### **(Key, Value) Pair:**
 is the input/output of Map and Reduce tasks, where both key and value can be of any data-type.

### **Combiners:**
 are the task run at the output of each Map task.
 - Users can implement this function, by default they are identity functions.
 - They are helpful if the operation performed at Reduce task is Associative and Commutative in nature, as then we can perform part of that task at each Map task running node itself, thus reducing the workload for Reduce task and also benefitting parallelism(combiners running paralley at different nodes).

### **Hashing (key, value) Pair:**
the (key, value) pairs are hashed into multiple intermediate files.
 - This makes sure that all (key, value) pairs of a specific key is hashed to an intermediate file, say A. Then we can be sure that for all Map tasks running across all nodes, that specific key is present at their intermediate file A only.
### **Intermediate File:**
are the files located locally(not in DFS) at the compute node and holds a part of the output of a Map task.
 - For n Reduce tasks, there will be n intermediate files for each Map task.
 - Each output of a Map task is hashed into one of these intermediate files.


### **Sort and Shuffle:**
 refers to the phase inbetween Map and Reduce and outputs key and its value list as a pair for each unique key.
 - This phase is carried out by the MapReduce framework.

### **Reduce:**
 refers to the task that is performed on the list of values for a specific key.
  - The input and output both are (key, value) pair.
  - Users implement this function.
  - All key values of the same key always comes to the same Reduce task.
 - A node may be running map task(s) and reduce task(s) at the same time.


### **Master and Worker nodes:**
**Master node:** is the node to whom the job is submitted by the user, it need not be necessarily unique(to increase reliability).
 - Even after assigning task to worker nodes, master node still keeps pinging them to check failure of nodes.

**Worker node:** is the node on which the tasks(Map, Reduce) are executed by the master.
 - Following the Big Data principle:_"Bring the algorithm to the data."_ the Map task is run on the worker node who posess the data chunk in context.

### **Name and Data nodes:**
**Name node:** contains the metadata of the chunks.
**Data node:** contains the chunks.
 - A node can be name node and data node at the same time.