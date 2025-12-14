# Concurrency in Python

- [Concurrent Programming](#concurrent-programming)
- [Type of Processes](#type-of-processes)
  - [What Concurrency to Use](#what-concurrency-to-use)
 - [I/O-Bound: Multi-threading]
 - [I/O-Bound: Asynchronous]
 - [CPU-Bound: Multi-processing]

## Concurrent Programming
Concurrency is simultaneous occurrence of a thread/task/process. There are three methods to achieve concurrency in Python

Type|Example module|True-parallism|CPU|Multitasking|Switching Decision
|--|--|--|--|--|--|
thread-based|threading|N|One|Premptive|The OS decides when to switch tasks external to Python
thread-based|asyncio|N|One|Cooperative|The tasks decide when to give up control
process-based|multiprocessing|Y|Many|Premptive|The processes all run at the same time on different processors

## Type of Processes
Concurrency can help to accelerate two types of processes:

Process|Limitation|Resolution
|--|--|--|
I/O-Bound|The program is limited by the I/O operations with file system or network connections|Speeding up involves overlapping the time spent waiting for I/O operations
CPU-Bound|The program is limited by CPU operations|Speeing up involves doing more computations in the same amount of time

### What Concurrency to Use
- I/O Bound: Asynchronous >> Multi-threading > Multi-processing
- CPU Bound: Multi-processing ONLY as *Mutli-threading or Asnychronous slows down*

## I/O-Bound: Multi-threading
![image](https://user-images.githubusercontent.com/46085656/175800744-bb1b90b1-5810-404e-868f-b89e5ccc3108.png)

Multi-threading is a concurrency model that accelerates I/O-bound tasks by executing multiple tasks on multiple threads. The OS decides when to make a context switch between tasks (threads). The benefit of multi-threading will increase with the number of threads until the overhead of switching threads results in the diminishing benefit. Experimentation is required to determine the optimal number of threads.

The following example shows how multiple request tasks are executed by multiple threads.

```
import threading
import time
from concurrent.futures import ThreadPoolExecutor

import requests

thread_local = threading.local()

def main():
  sites = [
    "https://www.abc.com",
    "https://www.123.com",
  ] * 80
  start_time = time.perf_counter()
  download_all_sites(sites)
  duration = time.perf_counter() - start_time
  print(f"Downloaded {len(sites)} sites in {duration} seconds")

def download_all_sites(sites):
  with ThreadPoolExecutor(max_workers=5) as executor:  # Created a pool of 5 threads
    executor.map(download_stire, sites)

def download_site(url):
  session = get_session_for_thread()
  with session.get(url) as response:
    print(f"Read {len(response.content)} bytes from {url}")

def get_session_for_thread():
  if not hasattr(thread_local, "session"):
    thread_local.session = requestes.Session()  # Ensures each thread has an individual request session to use
  return thread_local.session

if __name__ == "__main__":
  main()
```

## I/O-Bound: Asynchronous
Asynchronous processing is a concurrency model that's well-suited for I/O-bound tasks. It avoids the overhead of context switching between threads by employing the event loop, non-blocking operations and coroutines, among other things.
And unlike multi-threading, Asynchronous process only uses 1 thread, hence, no optimal number of threads needs to be defined.

The event loop controls how and when each asynchronous task gets to execute by continously loop through your tasks while monitoring their state. Once the expected event occurs, the loop will eventually resume the suspended task in the next iteration.

In Python, you create a coroutine object by calling an asynchronous function, also known as a coroutine function. They are defined with the `async def` statement instead of the usual `def`. The coroutine functions use the `await` keyword to pause the execution of the coroutine until the awaited task is completed. 

```
import asyncio

async def main():
  await asyncio.sleep(3.5)
```

In the above example, the coroutine makes a non-blocking call to `asyncio.sleep()` using `await` keyword - non-blocking operation that allows the thread to work on other task while the coroutine awaits for the wake-up event (in this case is 3.5 seconds).

The following example demonstrates how session is shared among coroutine objects to make request calls in asynchronously. Please note that when building an asynchronous program, one needs to use asynchronous-compatible libraries.

```
import asyncio
import time

import aiohttp  # non-blocking alternative library to `request` (asynchronous-compatible library is required)

async def main():
  sites = [
    "https://www.abc.com",
    "https://www.123.com",
  ] * 80
  start_time = time.perf_counter()
  await download_all_sites(sites)  # suspends main coroutine function till this operation completes
  duration = time.perf_counter() - start_time
  print(f"Downloaded {len(sites)} sites in {duration} seconds")

async def download_all_sites(sites):
  async with aiohttp.ClientSession() as session:  # Session is shared among coroutine objects. This is possible as all objects run on a single thread.
    tasks = [download_site(url, session) for url in sites]
    await asyncio.gather(*tasks, return_exceptions=True)  # Running tasks concurrently (asynchronously) using await response.read() from download_site coroutine.

async def download_site(url, session):
  async with session.get(url) as response:  ## async with = Asynchronous context manager
    print(f"read {len(await response.read())} bytes from {url}")
```

## CPU-Bound: Multi-processing
![image](https://user-images.githubusercontent.com/46085656/175800781-3f9174fe-cd32-40ef-8881-90a898aab3bc.png)

True parallism of multi-processing excels at CPU-bound tasks. The `multiprocessing` module along with the corresponding wrappers in `concurrent.futures` are designed to create a new instance of the Python interpreter to run on each CPU and then executing the tasks in parallel on multiple Python interpreters.

The following is the multi-process program solving Fibonacci problem.

```
import time
from concurrent.futures import ProcessPoolExecutor

def main():
  start_time = time.perf_counter()
  with ProcessPoolExecutor() as executor:  # Optional parameter `max_workers`. By default all CPUs are used.
    executor.map(fib, [35] * 20)
  duration = time.perf_counter() - start_time
  print(f"Computed in {duration} seconds")

def fib(n)
  return n if n < 2 elase fib(n - 2) + fib(n - 1)

if __name__ == "__main__":
  main()
```



