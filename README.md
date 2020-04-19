## Data Streaming Nanodegree
### SF Crime Statistics with Spark Streaming 

**1. How did changing values on the SparkSession property parameters affect the throughput and latency of the data?**

By referring to the `processedRowsPerSecond`. The higher number we get on here, it means that we could process more rows in second, which means higher throughput. Several recommended parameters that we could change are defined below.


**2. What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?**
The goal on here is to maximize the `processedRowsPerSecond` in long run. So, I choose:
- spark.default.parallelism
- spark.streaming.kafka.maxRatePerPartition
- spark.sql.shuffle.partitions

Before we jump in into configuration part, we need to define the input and infrastructure first. For instance, if we are working with HDFS, the maximum partition size is 128mb. And then, we need to define the expected dataset size. These parameters could be used to define how many partition that we need with the formula of `total input dataset size / partition size = total partition`.

And then, for the parallelism part, it depends on the total number of all cores on all machines. The rule of thumb is to set `3 per CPU core`. For example, if we have 4 quad-core machines, we could set the `spark.default.parallelism` into `48`

Last, `spark.streaming.kafka.maxRatePerPartition` would be useful to make the stream process running stable and efficient. This configuration set the maximum rate for each partition, and it prevents from being overwhelmed when there is a large number of unprocessed messages. 