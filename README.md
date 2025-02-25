[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: 
### Student Id: 
### Email: 

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > The measurement tool I used for this task is sysbench.
    > I configured sysbench to measure CPU performance and memory performance as follow:
    
         CPU Performance Test Configuration: **sysbench --test=cpu --num-threads=4 --cpu-max-prime=10000 run**
    >
    >•	--test=cpu: This specifies that we are testing CPU performance.
    >
    >•	--num-threads=4: This defines the number of threads used during the test. I chose 4 threads to simulate a multi-core processing environment, as most modern CPUs are multi-  core, and this would provide a better representation of how the CPU performs under parallel processing.
    >
    >• --cpu-max-prime=10000: This sets the maximum prime number used in the test. The higher the number, the more complex the computation. I selected 10000 to ensure the test is               challenging enough but not excessively long. It also helps measure the CPU’s ability to handle computationally intensive tasks.
    >
    >•	run: This command runs the test with the specified configuration.

          Memory Performance Test Configuration:**sysbench --test=memory --memory-block-size=1M --memory-total-size=10G run**
    >
    >•	--test=memory: This specifies that we are testing memory performance.
    >
    >•	--memory-block-size=1M: This sets the block size to 1MB, which means each memory read/write operation will transfer 1MB of data at a time. This is a reasonable size to measure         the throughput of memory without being too small or too large.
    >
    >•	--memory-total-size=10G: This sets the total amount of memory to be tested to 10GB. It helps simulate a larger memory workload and measures the system’s performance when               handling significant amounts of data.
    >
    >•	run: This command runs the memory performance test.
    >
          Why these parameters were set
	>•	Number of Threads (4): The number of threads simulates real-world multitasking environments. Using 4 threads represents a scenario where the system performs tasks concurrently. For systems with multiple CPU cores, running multiple threads can better utilize the available resources and provide a more accurate representation of performance.
    >
	>•	CPU Max Prime (10000): A value of 10000 for the maximum prime number is chosen to balance between the complexity of the computation and the time required for the test. This provides a reasonable level of CPU stress without overloading the system.
    >
	>•	Memory Block Size (1MB) and Total Size (10GB): A block size of 1MB is large enough to test the memory performance without causing excessive overhead. 10GB is a sufficiently large memory workload to simulate real-world scenarios where large datasets are being processed.
    >
     Explanation of the Results
    >The values obtained from the sysbench tests represent specific performance metrics, including:
    >
	>•	CPU Performance Results:
    >
	>•	The output of the CPU test will show the number of prime numbers computed and the total time taken to complete the test.
    >
	>•	One of the key metrics is “operations per second” (ops/sec), which indicates how many operations the CPU can perform per second under the given configuration.
    >
	>•	A higher ops/sec value indicates better CPU performance.

	>•	Memory Performance Results:
    >
	>•	The memory test will report “transferred” data (in GB) and “read/write” operations per second.
    >
	>•	This helps assess the system’s memory bandwidth and its ability to handle large-scale memory operations.
    >
	>•	A higher value indicates better memory throughput.
	




3. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 2296.49 event/s  | 18964.54 MiB/sec   |
    | `t2.medium` | 4212.17 event/s | 19112.27 MiB/sec   |
    | `c5d.large` | 1918.08 event/s | 20743.74 MiB/sec   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    >
    > 
    **For CPU performance**
    >The t2.medium instance, with 2 vCPUs, has the best CPU performance (4212.17 events/sec), which is more than double the performance of t2.micro with 1 vCPU (2296.49 events/sec). However, c5d.large with 2 vCPUs shows lower performance (1918.08 events/sec), which is unexpected given its higher memory resources and focus on compute. This suggests that while there is some correlation between CPU performance and the number of vCPUs, other factors like the specific instance type optimization may play a significant role.
   	
   **For Memory performance**
   >The c5d.large instance outperforms the others in memory performance with 20743.74 MiB/sec, which is expected due to its higher memory bandwidth optimization and focus on high-performance workloads. t2.micro and t2.medium have similar memory performance, with t2.medium showing a slight increase, which aligns with the increase in memory resources. The performance increase in memory seems more consistent with the increase in memory size.

   The performance increase in CPU performance does not always increase proportionally with the number of vCPUs. The t2.medium performs best for CPU tasks, while c5d.large with more specialized capabilities performs lower, possibly due to its different optimization focus. However, memory performance shows a more consistent increase with larger resources, with c5d.large clearly outpacing the other two instances.
   

    
    

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |     4080       | min/avg/max/mdev = 0.192/0.265/1.029/0.155 ms   |
    | `m5.large` - `m5.large`   |     4960       | min/avg/max/mdev = 0.122/0.136/0.148/0.008 ms   |
    | `c5n.large` - `c5n.large` |     4950       |  min/avg/max/mdev = 0.122/0.132/0.139/0.005 ms  |
    | `t3.medium` - `c5n.large` |     2140       | min/avg/max/mdev=0.733/0.742/0.763/0.009 ms     |
    | `m5.large` - `c5n.large`  |     2590       | min/avg/max/mdev = 0.566/0.575/0.603/0.011 m    |
    | `m5.large` - `t3.medium`  |     1740       | min/avg/max/mdev = 0.982/0.999/1.019/0.012 ms   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.





    

3. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |                |          |
    | N. Virginia - N. Virginia |                |          |
    | Oregon - Oregon           |                |          |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
