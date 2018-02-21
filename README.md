# MovieRecommendationSystemOnHadoop

1. Objective
  - Deployed simple item-based movie recommendation system in map-reduced form on Hadoop cluster
  - Configured the YARN (resource manager of Hadoop) to optimise the performance via best utilising the cluster
  
2. Steps
  - recommendation system
    -> Divide data by user id
    -> Build co-occurrence matrix
    -> Normalize the co-occurrence matrix
    -> Build rating matrix
    -> Multiply co-occurrence matrix and rating matrix
    -> Generate recommendation list
  - set up the hadoop environment
    -> created a hadoop cluster on Amazon Elastic MapReduce EC2 instances with 8 datanodes.

3. Experiment with the YARN setting
  - default: we observed that there is a bottleneck stage in building the co-occurrence matrix in O(N^2)
  - increase the number of mapper in stage 2: 
    -> via adjsting 
      > input block size
      > number of map tasks
      
4. Result
   1. In our test, we use 20 mapper, 40 mapper 60 mapper and 80 mapper to run the second step in the recommendation film system and track the work time respectively. The more mappers we used, the less time the system needs to compute the result definitely. Since one process per container in the datanode, and more processes run simultaneously with the allocated CPU resource will improve the performance of CPU. 
   
   2. We found out that the reduce phase has 3 steps: shuffle, sort and reduce.  Shuffling is the process of transferring data from the mappers to the reducers. So shuffling can start even before the map phase has finished, to save some time. That's why we can see a reduce status greater than 0% (but less than 33%) when the map status is not yet 100%.
   
   3. In conclusion, we configured the CPU-related setting to optimise the CPU-bound application for the mapper.
   
   
    
