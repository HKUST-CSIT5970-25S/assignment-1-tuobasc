[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Cao Zilin
### Student Id: 21085247
### Email: zcaoas@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Measurement: `phoronix-test-suite`
    >
    > (1) For CPU performance, `pts/compress-gzip` is selected to be the benchmark, which is a typical and easy benchmark to test CPU performance. We use the default configuration to run the test for 3 times. The benchmark will output the time used to run the test.
    >
    > (2) For memory performance, `pts/ramspeed` is selected to be the benchmark. We choose to test the `Copy` scenario, which copies data from one memory area to another, and can reflect the memory bandwidth and data transfer capabilities. The test will use `Floating Point` numbers for calculations. For scenarios that require high precision and scientific computing, floating-point tests can better reflect the actual computing power of the system. We run the test for 3 times. The benchmark will output the memory bandwidth.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 133.432s | 11691.56 MB/s |
    | `t2.medium`  | 51.012s | 19397.26 MB/s |
    | `c5d.large` | 46.053s | 14140.45 MB/s |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    >
    > The CPU performance improves significantly when the instance goes from t2.micro to t2.medium, which is directly related to the increase in the number of vCPUs. On the other hand, when going from t2.medium to c5d.large, although the number of vCPUs is the same, there is still some performance improvement, probably because of the use of a more efficient CPU architecture.
    >
    > Memory performance improves significantly from t2.micro to t2.medium and c5d.large, indicating that increased memory resources and better memory configuration can improve bandwidth.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3880           | 0.261    |
    | `m5.large` - `m5.large`   | 4910           | 0.145    |
    | `c5n.large` - `c5n.large` | 9430           | 0.107    |
    | `t3.medium` - `c5n.large` | 2340           | 0.722    |
    | `m5.large` - `c5n.large`  | 2530           | 0.658    |
    | `m5.large` - `t3.medium`  | 4290           | 0.458    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.
    >
    > We can find that EC2 instances of the same type can achieve better network performance in the same region. When different types of instances communicate, bandwidth will decrease and latency will increase.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 3110           | 63.382   |
    | N. Virginia - N. Virginia | 4780           | 0.289    |
    | Oregon - Oregon           | 4770           | 0.174    |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
    >
    > Obviously, the network performance within the same region is significantly better than that across regions. Node transmission within the same region has higher bandwidth and lower latency.
