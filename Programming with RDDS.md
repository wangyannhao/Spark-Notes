# Programming with RDDS

## RDD Basics
An RDD in Spark is simply an immutable distributed collection of objects. Each RDD is split into multiple partitions, which may be computed on different nodes of the cluster. RDDs can contain any type of Python, Java, or Scala objects, including user- defined classes.

Users create RDDs in two ways: by loading an external dataset, or by distributing a collection of objects (e.g., a list or set) in their driver program. We have already seen loading a text file as an RDD of strings using SparkContext.textFile().

Once created, RDDs offer two types of operations: transformations and actions. Transformations construct a new RDD from a previous one. 

##### Transformation Example: 

	>>> pythonLines = lines.filter(lambda line: "Python" in line) 

Actions, on the other hand, compute a result based on an RDD, and either return it to the driver program or save it to an external storage system (e.g., HDFS). 

##### Action Example:

	>>> pythonLines.first()

## Transformations VS Actions
* **Transformation are lazy**, results not immediately computed.
	
	Returns another RDD.
* **Actions are eager**, their result is immediately computed. 

	Returns value.

## Persist VS Cache
To avoid computing an RDD multiple times, we can ask Spark to persist the data. When we ask Spark to persist an RDD, the nodes that compute the RDD store their partitions. If a node that has data persisted on it fails, Spark will recompute the lost partitions of the data when needed. We can also replicate our data on multiple nodes if we want to be able to handle node failure without slowdown. 
	
Spark has many levels of persistence to choose from based on what our goals are

| Level               | Space Used | CPU time | In memory | On disk |
|---------------------|------------|----------|-----------|---------|
| MEMORY_ONLY         | High       | Low      | Y         | N       |
| MEMORY_ONLY_SER     | Low        | High     | Y         | N       |
| MEMORY_AND_DISK     | High       | Medium   | Some      | Some    |
| MEMORY_AND_DISK_SER | Low        | High     | Some      | Some    |
| DISK_ONLY           | Low        | High     | N         | Y       |


Example: *persist()* in Scala

	val result = input.map(x => x * x)
	result.persist(StorageLevel.DISK_ONLY)
	println(result.count()) 	
	println(result.collect().mkString(","))
	
cache() is similar to persist() with default storage level which is MEMORY_ONLY.

