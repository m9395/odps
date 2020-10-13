---
keyword: [extended MapReduce model, MR2]
---

# Extended MapReduce model

The extended MapReduce model \(MR2\) that is provided by MaxCompute uses optimized scheduling and I/O models to reduce unnecessary I/O operations during job running.

In the extended MapReduce model, Map and Reduce functions are written in the same way as MaxCompute. The difference lies in how the jobs run. For more information, see [Pipeline samples](/intl.en-US/Development/MapReduce/Program Example/Pipeline samples.md).

## Background information

A MapReduce model consists of multiple MapReduce jobs. In a traditional MapReduce model, the output of each MapReduce job must be written to a disk in a distributed file system such as HDFS or to a MaxCompute table. However, subsequent Map tasks may only need to read the outputs once to prepare for the Shuffle stage. This mechanism results in redundant I/O operations.

The computing and scheduling logic of MaxCompute supports more complex programming models. A Map operation can be succeeded by any number of consecutive Reduce operations without the need for a Map operation in between, such as Map-Reduce-Reduce.

## Comparison with Hadoop ChainMapper and ChainReducer

Similarly, Hadoop ChainMapper and ChainReducer also support serialized Map or Reduce operations. However, they are essentially different from the extended MapReduce model.

Hadoop ChainMapper and ChainReducer are based on the traditional MapReduce model. They support only Map operations after the original Map or Reduce operation. One benefit is that you can reuse the Mapper business logic to split a Map or Reduce operation into multiple Mapper stages. However, this does not modify the scheduling or I/O models.

