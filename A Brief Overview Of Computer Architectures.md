# A Brief Overview of Computer Architecture
- Big ideas: Memory, CPUs, and Networking
- Let's draw some pictures!

## Link to Lecture
https://tinyurl.com/rsmas-python2


## Memory Hierarchy
![](http://faculty.etsu.edu/tarnoff/othr2150/mem_hier.gif)

## CPU Layout and Networking
1. Somewhere between 1 and 16 cores
2. Modern processors will support hyperthreading / virtual cores
	- Oftentimes the benefit of two processors for the price of 1!
![](https://www.gamingscan.com/wp-content/uploads/2017/12/cpu-core-for-gaming.jpg)

## Numbers Every Programmer Should Know
[Source](https://gist.github.com/hellerbarde/2843375) and Jeff Dean, Peter Norvig, etc.

Latency Comparison Numbers (~2012)
-----------

| Event                              | Nanoseconds   | Microseconds | Milliseconds | Comparison    |
|------------------------------------|--------------:|--------:|----:|-----------------------------|
| L1 cache reference                 |           0.5 |       - |   - | -                           |
| L2 cache reference                 |           7.0 |       - |   - | 14x L1 cache                |
| Main memory reference              |         100.0 |       - |   - | 20x L2 cache, 200x L1 cache |
| Compress 1K bytes with Zippy       |       3,000.0 |       3 |   - | -                           |
| Send 1K bytes over 1 Gbps network  |      10,000.0 |      10 |   - | -                           |
| Read 4K randomly from SSD          |     150,000.0 |     150 |   - | ~1GB/sec SSD                |
| Read 1 MB sequentially from memory |     250,000.0 |     250 |   - | -                           |
| Round trip within same datacenter  |     500,000.0 |     500 |   - | -                           |
| Read 1 MB sequentially from SSD    |   1,000,000.0 |   1,000 |   1 | ~1GB/sec SSD, 4X memory     |
| Disk seek                          |  10,000,000.0 |  10,000 |  10 | 20x datacenter roundtrip    |
| Read 1 MB sequentially from disk   |  20,000,000.0 |  20,000 |  20 | 80x memory, 20X SSD         |
| Send packet CA → Netherlands → CA  | 150,000,000.0 | 150,000 | 150 | -                           |


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
    L2 cache reference                  7 s           Long yawn

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

## Rolling Your Own Processes in Python: `subprocess`
1. Running a script from the command line
```python
import subprocess
subprocess.call(['python3 run_me.py', shell=True])
```
2. Getting the stdout and stderr from a process
```python
output, errors = subprocess.Popen(['ls', '-la', '*.py'], 
			          stdout=subprocess.PIPE).communicate()
```
3. Piping processes
```python
p1 = subprocess.Popen(["cat", "file.log"], stdout=subprocess.PIPE)
p2 = subprocess.Popen(["tail", "-1"], stdin=p1.stdout, stdout=subprocess.PIPE)
p1.stdout.close()  # Allow p1 to receive a SIGPIPE if p2 exits.
output,err = p2.communicate()
```

# Parallel Programming
1. Lots of models
2. Hard to do super efficiently
3. However, there are some easy ways to get gains

## Multi-Core Parallel Computing vs. Distributed Parallel Computing
1. Using many cores from your machines is different than using many cores on multiple machines
2. Pegasus, as far as I can tell, makes it as if you are using a lot of cores on one machine

## Big Ideas that Matter

### `numpy` is your friend
1. Vectorizing functions is the easiest step to wriitng fast and parallel code
2. Many Numpy Operations already are parallelized across machines

### Split-Apply-Combine
1. Split
    - Think about how to break up the data so that you can compute the same thing on smaller parts of the larger dataset
2. Apply
    - Solve the problem a bunch of times on smaller versions of your large data
3. Combine
    - Then combine those mini solutions into one big solution

### Data Needs to be Independent
1. In image analysis, you can apply convolutions because they only matter on a small subset of the data 
2. If you need all of the data to do each computation, it's less efficient



## MapReduce or Map and then Reduce
```python
# Parallel version is equivalent to the following
def map_parallel(func, input_data):
    intermediate_data = []
    for i in input_data:
        intermediate_data.append(func(i)
    return intermediate_data

def reduce(func, intermediate_data):
    starting_value = None
    for value in intermediate_data:
        if starting_value == None:
          starting_value = value
        else:
          starting_value = func(starting_value, value)
    return value
```

### MapReduce in Python: `multiprocessing`
1. Prepare data to be split between processors
2. Use `multiprocessing` to apply `map` step
3. Combine the results of the `map` result as needed
4. Easy when your data is small
5. Good for simple functions
6. [Some good examples](https://stackoverflow.com/questions/2846653/how-to-use-threading-in-python)

```python
from multiprocessing import Pool

def f(x):
    return x*x

if __name__ == '__main__':
    with Pool(5) as p:
        print(p.map(f, [1, 2, 3]))

###################################

# Sequential
results = []
for item in my_array:
    results.append(my_function(item))

# Parallel
from multiprocessing.dummy import Pool as ThreadPool 
pool = ThreadPool(4) 
results = pool.map(my_function, my_array)

```

### `Numba`
1. For more complicated parallel tasks
2. Converts functions to C code
3. Can parallelize code easily (if you read the documentation :D )

```python
@numba.jit(nopython=True, parallel=True)
def logistic_regression(Y, X, w, iterations):
    for i in range(iterations):
        w -= np.dot(((1.0 / (1.0 + np.exp(-Y * np.dot(X, w))) - 1.0) * Y), X)
    return w
```


# Post Session Discussion
1. Virtual environments: https://conda.io/docs/user-guide/tasks/manage-environments.html
2. Figuring out your dependencies: `pip freeze`
3. `pylint <filename>` for linting
4. `autopep8 <filename>` for correcting easy linting errors

## Testing
1. Two types
	1. Unit Test
		- Just tests individual functions
		- Define some input and some desired output
		- Tests whether it works
	2. Integration Test
		- Combining functions
		- COmbining modules
		- Very simple example cases
2. Tests are written in a separate folder usually
