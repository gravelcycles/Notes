# A Brief Overview of Computer Architecture
- Big ideas: Memory, CPUs, and Networking
![](http://easy-directory.info/wp-content/uploads/2017/06/arch1.gif)


## Memory Layout
![](https://www.researchgate.net/profile/Bojan_Jovanovic/publication/281805561/figure/fig1/AS:324966131224576@1454489371431/Typical-structure-of-a-computer-memory-hierarchy.png)

## CPU Layout and Networking
![](https://www.gamingscan.com/wp-content/uploads/2017/12/cpu-core-for-gaming.jpg)

## Numbers Every Programmer Should Know
[Source](https://gist.github.com/hellerbarde/2843375) and Jeff Dean, Peter Norvig, etc.

Latency Comparison Numbers (~2012)
----------------------------------
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                      14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                      20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy             3,000   ns        3 us
Send 1K bytes over 1 Gbps network       10,000   ns       10 us
Read 4K randomly from SSD             150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
Disk seek                           10,000,000   ns   10,000 us   10 ms  20x datacenter roundtrip
Read 1 MB sequentially from disk    20,000,000   ns   20,000 us   20 ms  80x memory, 20X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms

Notes
-----
1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns

## Humanized Latency Numbers
Lets multiply all these durations by a billion:

Magnitudes:
----------------------------------------------

Minute:
-----
    L1 cache reference                  0.5 s         One heart beat (0.5 s)
    Branch mispredict                   5 s           Yawn
    L2 cache reference                  7 s           Long yawn
    Mutex lock/unlock                   25 s          Making a coffee

Hour:
-----
    Main memory reference               100 s         Brushing your teeth
    Compress 1K bytes with Zippy        50 min        One episode of a TV show (including ad breaks)

Day:
----
    Send 2K bytes over 1 Gbps network   5.5 hr        From lunch to end of work day

Week:
-----
    SSD random read                     1.7 days      A normal weekend
    Read 1 MB sequentially from memory  2.9 days      A long weekend
    Round trip within same datacenter   5.8 days      A medium vacation
    Read 1 MB sequentially from SSD    11.6 days      Waiting for almost 2 weeks for a delivery

Year
----
    Disk seek                           16.5 weeks    A semester in university
    Read 1 MB sequentially from disk    7.8 months    Almost producing a new human being
    The above 2 together                1 year

Decade
-----
    Send packet CA->Netherlands->CA     4.8 years     Average time it takes to complete a bachelor's degree


## Conclusions
1. CPUs are really fast
    - The goal: Have the CPU always be working
2. Memory is fast-ish, hard drives are slow
3. You want to make your CPU happy by always giving it data that is close at hand
4. Networks are reeeeaaally slow

# Processes and Threads
1. Since there's so much downtime when we need to get data from disk or over a network, we can use the CPU for another task
2. Switching between tasks can keep our CPU working
    - 3,000-30,000 ns for a context switch, or about 50 minutes to 8 hours in human time
3. To allow this, we want multiple different programs that are able to run at the same time
    - These are called processes
4. A process has 4 parts
    - The program code
    - The Call Stack (what is actually happening in the program)
    - Variables, files, memory, etc
5. Within a program, we have also have multiple mini-programs running
    - These are called threads
    - A child thread and its parent share the same memory (usually)

# Parallel Programming
1. Lots of models
2. Hard to do super efficiently
3. However, there are some easy ways to get gains

## Map Reduce


## Python 
1. Subprocess
2. multiprocessing
    - https://stackoverflow.com/questions/2846653/how-to-use-threading-in-python

## Issues
1. Big Data
2. 

