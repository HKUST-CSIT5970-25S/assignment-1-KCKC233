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
    >•	--num-threads=4: This defines the number of threads used during the test. I chose 4 threads to simulate a multi-core processing environment, as most modern CPUs are multi-  
 core, and this would provide a better representation of how the CPU performs under parallel processing.
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
    

    
    

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |                |          |
    | `m5.large` - `m5.large`   |                |          |
    | `c5n.large` - `c5n.large` |                |          |
    | `t3.medium` - `c5n.large` |                |          |
    | `m5.large` - `c5n.large`  |                |          |
    | `m5.large` - `t3.medium`  |                |          |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |                |          |
    | N. Virginia - N. Virginia |                |          |
    | Oregon - Oregon           |                |          |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
