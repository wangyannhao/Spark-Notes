# Getting Start With Spark

* [Download Spark](http://spark.apache.org/downloads.html)

## Spark's Python and Scala Shell
	bin/pyspark  # Spark's python shell
	bin/spark-shell # Spark's Scala shell
	
You can also use IPYTHON or IPYTHON notebook to open shell:
	
	PYSPARK_DRIVER_PYTHON=IPYTHON ./bin/pyspark
	PYSPARK_DRIVER_PYTHON_OPTS="notebook" ./bin/pyspark

### Line Count Example

	>>> lines = sc.textFile("README.md") # Create an RDD called lines	>>> lines.count() # Count the number of items in this RDD	>>> lines.first() # First item in this RDD, i.e. first line of README.md
	
Variable lines is a **RDD** (*resilient distributed datasets*). 

## Core Spark Concepts

Driver programs access Spark through a SparkContext object, which represents a connection to a computing cluster. In the shell, a SparkContext is automatically created for you as the variable called sc.
	
	>>> sc	<pyspark.context.SparkContext object at 0x1025b8f90>

Once you have a SparkContext, you can use it to build RDDs. We called sc.textFile() to create an RDD representing the lines of text in a file. We can then run various operations on these lines, such as count().

## Standalone Applications
In Python, you simply write applications as Python scripts, but you must run them using the bin/spark-submit script included in Spark. The spark-submit script includes the Spark dependencies for us in Python. This script sets up the environ‐ ment for Spark’s Python API to function.

	bin/spark-submit my_script.py

## Initializing a SparkContext

	from pyspark import SparkConf, SparkContext	conf = SparkConf().setMaster("local").setAppName("My App")	sc = SparkContext(conf = conf)
	
Above is the minimal way to initialize a SparkContext. You need to pass two parameters:

* A cluster URL, namely local in these examples, which tells Spark how to connect to a cluster. local is a special value that runs Spark on one thread on the local machine, without connecting to a cluster.
* An application name, namely My App in these examples. This will identify your application on the cluster manager’s UI if you connect to a cluster.



