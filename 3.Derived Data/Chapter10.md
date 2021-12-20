# Chapter 10- Batch Processing
## Three types of systems
## Services
A service waits for a client to make a request and returns a response. Response time and availability are often the measurement of performance.

## Batch processing systems(offline)
A batch processing system takes a large amount of input data, runs a job to process it, and produces some output data. Performance measure is throughput.

## Stream processing systems (near-real-time systems)
A stream job operates on events shortly after they happen, whereas a batch job operates on a fixed set of input data. 

## Batch processing with Unix
let's assume we have a log file of urls(urls at 7th field). 
cat /var/log/nginx/access.log |
awk '{print $7}' |
sort |
uniq -c |
sort -r -n |
head -n 5

These Unix commands take the 7th field(urls), sorts them alphabetically, counts how many types each url repeated, makes them unique, the second sort sorts by occurrence counts
of each url and sorts them in reverse order to get the largest urls.
These commands can process GBS of data in seconds.
Many data analyses can be done in a few minutes using some combination of awk, sed, grep, sort, uniq, and xargs, and they perform surprisingly well
This chain of commands style data wrangling is good because it does not create intermediate files but rather temporarily holds the output in a buffer and
passes it onto the next command. This requires each input and output to have a uniform interface.

## MapReduce and Distributed file systems
1. MapReduce is like Unix pipes but distributed across possibly millions of machines.
2. Running a MapReduce job does not modify the input and does not have any side effects other than produced output(like Unix tools)
3. MapReduce uses a distributed file system(In Hadoop, it's called Hadoop distributed file system HDFS) to read and write inputs and outputs respectively.
4. Object storage services such as Amazon S3, Azure Blob Storage, and OpenStack Swift are similar in many ways to HDFS.
5. HDFS is based on the shared nothing principle.

## MapReduce job flow
MapReduce is a programming framework with which you can write code to process large datasets in a distributed filesystem like HDFS.
1. Read an input file and extract them as records.
2. Call the mapper function on the records to extract keys and values.
3. Sort the key value pairs by keys.
4. Use the reducer to process the key value pairs which are now adjacent due to the sorting.

Map reduce jobs are often chained together to create a workflow.

## Sort merge joins
In sort merge joins we take related information that have an association, join then, sort them and send them to a singular reducer for processing.
## Map Side Joins
Sort merge join is a Reducer Side join because it sends data to the reducer.
### Pros
we make no assumption about the type of data we get and pass it through the map and reduce flow.
### Cons
I/O for Map and reduce can be expensive.
Map Side joins make certain assumptions about data data due to which we can ignore the reducer part of the process.

## Examples
### Broadcast join
Let's say we have a log file of user activity and a database table of user records.
If we know that the user table is small, we can load it entirely in memory and put it in a hashmap instead of using a reducer for it.
We will still have multiple joins to process the user activity logs but we will put the user tables in memory in all these maps.
Thus in a way we're broadcasting the user table.

## The Output of Batch Workflows
1. Search Index’s for web.
2. Machine learning classified data.
