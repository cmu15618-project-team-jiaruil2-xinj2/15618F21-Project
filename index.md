# Work stealing Fork Join Parallel Framework in C++ (Jiarui LI (jiaruil2), Xin JIN (xinj2))

## URL
https://xinj10.github.io/15618-project/

## Summary
We are going to implement a fork-join parallel framework with work-stealing distributed queues in C++. Also, we would like to analyze its performance, overhead, and bottleneck under different configurations, such as a child or continuation stealing, lock-free queue, etc.

## Background 
The fork-join model is a simple and efficient approach to executing parallel programs. This parallel pattern allocates threads and divides tasks recursively until every task reaches a certain granularity. Then all the threads execute programs parallel to each other with their own task. Threads will join at some subsequent points to continue sequential execution. 
Problems that suffer a heavy workload of computing can benefit from this parallel scheme. Similar to divide-and-conquer algorithms, tasks are divided and are computed on multiple cores. Without much overhead, programs can be sped up at a significant level.
![Image of fork-join model](https://github.com/xinj10/15618-project/blob/gh-pages/assets/iu.png?raw=true)
(https://self-learning-java-tutorial.blogspot.com/2015/07/java-fork-join-framework.html)

## The challenge
Implementing a fork-join framework is challenging for the following reasons. For one thing, in order to get the expected speedup, the scheduling cost should be as small as possible. And there are a lot of possible implementation strategies with their own tradeoffs. For instance, the number of works a thread should steal when it becomes idle is a key design problem. In addition, the implementation of the work queue also has a lot of possibilities, such as a single centralized queue or per-thread distributed queues with coarse/fine-grained locks or even lock-free synchronization. For another, from the users’ perspective, a simple and easy-to-use API is also important. Doing this project, we want to learn the internals of fork-join parallel paradigms, explore different implementation strategies and analyze their pros and cons.

*Workload*:
Using the fork-join paradigm, tasks may block to wait for the completion of their subtasks. Therefore, there are dependencies between parent tasks and sub-tasks. However, since the task abstraction is separated from the execution instances and we tend to use dynamic assignment and work-stealing strategy, it is less likely to have divergent execution.

*Constraints*: 
Using the fork-join paradigm, because of the nature of dynamic task generation, it is hard to anticipate the workload of each task and impractical to statically map each task to each parallelization instance. Therefore, a distributed work queue with a work-stealing strategy is a better solution.

## Resources
Although we will start from scratch, we have found some good paper to begin with which discusses how this parallel design pattern works in detail. 
* R. D. Blumofe and C. E. Leiserson, "Scheduling multithreaded computations by work stealing," Proceedings 35th Annual Symposium on Foundations of Computer Science, 1994, pp. 356-368, doi: 10.1109/SFCS.1994.365680.
* Färnstrand, Linus. “Parallelization in Rust with fork-join and friends: Creating the fork-join framework.” (2015).
* Matteo Frigo Charles E. Leiserson Keith H. Randall, “The Implementation of the Cilk-5 Multithreaded Language”
* Doug Lea, “A Java Fork/Join Framework”

## Goals and deliverables
### PLAN TO ACHIEVE
We plan to implement a framework that can be used for fork-join parallel schemes. The framework is expected to for tasks and use a thread pool to execute tasks. Also, this framework will include two strategies which are child stealing and continuation stealing. On one hand, we will apply this model to a simple problem like merge sort to analyze the performance and speedup under different numbers of processors compared with a simple sequential version of the problem. On the other hand, we would like to achieve the results using different strategies,  break down the time spent in the execution and discuss the overall performance.

### HOPE TO ACHIEVE
If time permits, we would like to build an auxiliary visualization tool to demonstrate the working process and the scheduling graph of the fork-join model.

Besides, we would like to explore the different granularity of parallelism. To be specific, we want to try using processes, threads, and user-level threads as the task workers.

In addition, we would like to construct a distributed fork-join framework and get rid of the limitation of one single machine.
For the demo, we decided to show the plots of speedup compared to the performance on a single processor to demonstrate the effectiveness of our fork-join model. Running the program to show the output result is also a good way to demonstrate the correctness of our framework. If we finish the visualization tool we hope to achieve, we can use it to generate a gif that can be more intuitive to see the process of the fork-join model.
We hope to learn more about the advantages and disadvantages of this parallel model when dealing with different amounts of workloads and different numbers of processors.

## Platform choice
We would like to implement this framework in C++ 11. In addition, we plan to test our framework on GHC machines and PSC machines. Since GHC machines are free and have eight cores, they will be a good place for us to do benchmarking and initial testing on speedup. We also hope to see the performance with a larger number of cores, because we want to know if there is a bottleneck or other factors which limit the speedup. This is one of our focuses in this project, and that’s why we choose PSC to do some final testing. 

## Schedule
```
Week 1: Literature review, API design + Thread pool
Week 2: Dedicated Distributed Queue
Week 3: work stealing I (intermediate checkpoint)
Week 4: work stealing II
Week 5: measure the performance
Week 6: finalize our design, write a report and prepare for the poster session
```
### Writeups
[Project Proposal](https://github.com/xinj10/15618-project/blob/gh-pages/assets/proposal.pdf)
