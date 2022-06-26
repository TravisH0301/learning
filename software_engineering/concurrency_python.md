# Concurrency in Python

- [Concurrent programming](#concurrent-programming)
  - [Difference to parallel programming](#difference-to-parallel-programming)
- [Concurrency using threading](#concurrency-using-threading)
  - [Limitation of threading](#limitation-of-threading)
- [Concurrency using asyncio](#concurrency-using-asyncio)
- [Difference between threads and coroutines](#difference-between-threads-and-coroutines)

## Concurrent programming
Concurrent programming is when an application is processing multiple tasks at the same time, but not necessarily simultaneously.<br>
For example, CPU can process two tasks in concurrency.

![image](https://user-images.githubusercontent.com/46085656/175800744-bb1b90b1-5810-404e-868f-b89e5ccc3108.png)

### Difference to parallel programming
Parallel programming is when multiple processors are processing tasks simultaneously.

![image](https://user-images.githubusercontent.com/46085656/175800781-3f9174fe-cd32-40ef-8881-90a898aab3bc.png)

## Concurrency using threading
Concurrency can be achieved in Python using threading. A thread is an individual task.

    import threading
    import time

    def func1():
      for i in range(3):
        time.sleep(1)
        print('Inside Func1')

    def func2():
      for i in range(5):
        time.sleep(0.8)
        print('Inside Func2')

    threading.Thread(target = func1).start()
    threading.Thread(target = func2).start()

    print('Threads Started')

In above example, functions are defined and executed as a thread using *threading* library.<br>
Note that there are 3 threads in the execution - 2 threads for func1 & func2, and 1 thread for main Python code that has print() statement at the end.

### Limitation of threading
Synchronization is required when accessing shared data structures. The right lock is required to prevent deadlocking on the shared resource.

## Concurrency using asyncio
Python has a package called *asyncio* that supports concurrent programming.

    import asyncio
    import time

    async def func1():
      for i in range(5):
        print('Inside Func1')
        await asyncio.sleep(1)

    async def func2():
      for i in range(5):
        print('Inside Func2')
        await asyncio.sleep(0.8)

    start = time.time()
    async_tasks = asyncio.gather(func1(), func2())
    asyncio.get_event_loop().run_until_complete(async_tasks)
    end = time.time()
    print('Asyncio took {} seconds'.format(round(end-start),2))

In the above example, the async functions are defined using the keyword *async* during function definition.<br>
This keyword converts the function into a coroutine which allows Python to pause the function execution for concurrent processing.<br>
\**Coroutine is a function that can pause its execution, and indirectly pass processing control to another coroutine.*

And another keyword *await* is used within the async function. This keyword makes the async function to pause and allow other coroutine to run.<br>
Note that *asyncio.sleep()* is used instead of *time.sleep()*. This is because *time.sleep()* will hold the entire execution, while
*asyncio.sleep()* will only hold the respective async function.

*asyncio.gather()* and *asyncio.get_event_loop().run_until_complete()* is used to gather all async functions and execute until all tasks are complete.<br>
Using this event loop can give more control over the execution. However, for simplicity, *asyncio.run(func())* can be used alternatively.

## Difference between threads and coroutines
|Threads(threading)|Coroutines(asyncio)|
|--|--|
|Pre-emptive|Co-operative|
|The operating system decides when to context switch to another task.|The tasks themselves decide when to hand over the control.|
|Can switch to another thread at any point in time.|Only switched to another coroutine when there is an “await” keyword.|
|Using locks, we can tell when it is impossible to go to another thread.|Using await, we can tell when it is possible to go to another coroutine.|
