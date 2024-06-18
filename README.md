# Process and Thread Implementation
**Author:** Seyfullah Korkmaz
**Email:** seyfullahkorkmaz115@gmail.com  
**Date:** Sunday 26th May, 2024  

## Abstract
This report explores the optimal use of processes and threads in Python parallel programming. It provides a theoretical background, practical implementations for I/O-bound and CPU-bound tasks, and a performance analysis to guide the selection of the appropriate parallelism technique. Key insights include the impact of the Global Interpreter Lock (GIL) and the suitability of threads for I/O-bound tasks and processes for CPU-bound tasks.

## 1. Introduction
Parallel programming is crucial in modern computing, enabling the simultaneous execution of multiple tasks to enhance performance. In Python, parallelism can be achieved using processes or threads. Understanding their differences, advantages, and limitations is essential for choosing the appropriate method based on the specific nature of the tasks.

## 2. Theoretical Background
### 2.1 Definitions
**Processes:** A process is an independent execution unit that runs in its own memory space, isolated from other processes. Processes are suitable for CPU-bound tasks.
  
**Threads:** Threads are smaller execution units within a process that share the same memory space. They are lighter weight and suitable for I/O-bound tasks.

### 2.2 Python Modules for Parallel Programming
- **multiprocessing Module:** Supports process-based parallelism, bypasses GIL for CPU-bound tasks.
  
- **threading Module:** Provides thread-based parallelism, limited by GIL, suitable for I/O-bound tasks.

### 2.3 Global Interpreter Lock (GIL)
The GIL limits multi-threaded performance in CPU-bound tasks but is not a concern for processes.

### 2.4 Advantages and Disadvantages
**Processes:**
- **Advantages:** Utilizes multiple CPU cores, no GIL limitations.
- **Disadvantages:** Higher memory usage, more overhead in management.

**Threads:**
- **Advantages:** Lower memory footprint, faster to create/manage.
- **Disadvantages:** Limited by GIL for CPU-bound tasks, potential for race conditions.

## 3. Practical Implementation
### 3.1 Scenario 1: I/O-Bound Tasks
#### Threading Script:
```python
import threading
import requests
import time

def download_file(url):
    response = requests.get(url)
    print(f"Downloaded {url} with {len(response.content)} bytes")

urls = ["http://example.com/file1", "http://example.com/file2", "http://example.com/file3"]
start_time = time.time()
threads = []
for url in urls:
    thread = threading.Thread(target=download_file, args=(url,))
    threads.append(thread)
    thread.start()
for thread in threads:
    thread.join()
end_time = time.time()
print(f"Threading took {end_time - start_time} seconds")
```

#### Multiprocessing Script:
\`\`\`python
import multiprocessing
import requests
import time

def download_file(url):
    response = requests.get(url)
    print(f"Downloaded {url} with {len(response.content)} bytes")

urls = ["http://example.com/file1", "http://example.com/file2", "http://example.com/file3"]
start_time = time.time()
processes = []
for url in urls:
    process = multiprocessing.Process(target=download_file, args=(url,))
    processes.append(process)
    process.start()
for process in processes:
    process.join()
end_time = time.time()
print(f"Multiprocessing took {end_time - start_time} seconds")
\`\`\`

### 3.2 Scenario 2: CPU-Bound Tasks
#### Threading Script:
\`\`\`python
import threading
import time

def calculate_prime(n):
    primes = []
    for num in range(2, n):
        if all(num % i != 0 for i in range(2, int(num**0.5) + 1)):
            primes.append(num)
    return primes

n = 100000
start_time = time.time()
threads = []
for _ in range(4):  # Assume 4 threads
    thread = threading.Thread(target=calculate_prime, args=(n,))
    threads.append(thread)
    thread.start()
for thread in threads:
    thread.join()
end_time = time.time()
print(f"Threading took {end_time - start_time} seconds")
\`\`\`

#### Multiprocessing Script:
\`\`\`python
import multiprocessing
import time

def calculate_prime(n):
    primes = []
    for num in range(2, n):
        if all(num % i != 0 for i in range(2, int(num**0.5) + 1)):
            primes.append(num)
    return primes

n = 100000
start_time = time.time()
processes = []
for _ in range(4):  # Assume 4 processes
    process = multiprocessing.Process(target=calculate_prime, args=(n,))
    processes.append(process)
    process.start()
for process in processes:
    process.join()
end_time = time.time()
print(f"Multiprocessing took {end_time - start_time} seconds")
\`\`\`

## 4. Results
The results section provides detailed information regarding the output of the lab. This section is often dominated by tables and screenshots. Ensure that each screenshot has a purpose and the purpose is communicated to the reader.

### 4.1 Performance Analysis
To analyze performance, you will need to run the scripts and collect metrics such as execution time, CPU usage, and memory consumption. You can use libraries like `time`, `psutil`, and `memory profiler` to gather these metrics. Visualizations can be created using `matplotlib` or `seaborn`.

#### Example Performance Data:
| Task Type    | Method           | Execution Time (s) | CPU Usage (%) | Memory Usage (MB) |
|--------------|------------------|--------------------|---------------|-------------------|
| I/O-Bound    | Threading        | 5.2                | 25            | 100               |
| I/O-Bound    | Multiprocessing  | 6.8                | 30            | 150               |
| CPU-Bound    | Threading        | 15.4               | 50            | 200               |
| CPU-Bound    | Multiprocessing  | 9.6                | 75            | 250               |

## 5. Conclusion and Recommendations
### 5.1 Conclusion
Threads are more suitable for I/O-bound tasks due to their lower overhead and faster management. Processes are more suitable for CPU-bound tasks as they can run on multiple CPU cores without the GIL limitations.

### 5.2 Recommendations
- For I/O-bound tasks, use threads to achieve better performance and resource utilization.
- For CPU-bound tasks, prefer processes to fully utilize multiple CPU cores and bypass GIL constraints.
- Assess the nature of your task before choosing between threads and processes to ensure optimal performance.
