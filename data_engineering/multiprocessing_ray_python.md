# Multiprocessing and Ray on Python
On Python parallel processing can be achieved using either Multiprocessing and Ray. Both libraries utilise the CPU cores to execute tasks synchronously or asynchronously.

## Multiprocessing
Multiprocessing library has 2 types of parallel processing; Process and Pool. 

### Important Notes
- Multiprocessing execution should be done after `if __name__ == '__main__'` on a Python script. Multiprocessing won't work on an interactive platform, ex. Jupyter Notebook. 
- Multiprocessing function doesn't work well with global variables, always pass a variable directly to the function as an argument.
- When giving a very large iterable object to a parallel process function, break down into smaller chunks to avoid memory error. 

### Multiprocessing - Process
Process is used for function-based parallelism. 
~~~
# multiprocessing Process
import multiprocessing as mp

process_li = []
for devices in device_tuples:
  process = mp.Process(target=function_name, args=(x,))  # "," is used in (x,) to define it as a tuple with 1 element
  process.start()
  process_li.append(process)
  [process.join() for process in process_li]
~~~
`.start()` starts a process with a given function asynchronously <br>
`.join()` waits for the process to finish <br>
Having `.join()` for all processes make the Python interpreter to wait for all processes to finish before moving on. <br>
Results can be retrieved using Manager, Queue or Pipes.

### Multiprocessing - Pool
Pool allows a single function to be executed in parallel with multiple inputs. Hence, it's great for data-based parallelism. <br>
Pool can execute tasks synchronously or asynchronously. Note that asynchronous execution may not occur in the given order, yet, the execution is faster without blockage between processes.
~~~
# Multiprocessing Pool
import multiprocessing as mp

with mp.Pool(mp.cpu_count()) as pool:  # mp.cpu_count() returns available cpu cores (logical)
  results = [pool.apply(function_name, args=(x, 1, 2) for x in range(4)]
  pool.join()
~~~
The above example uses `.apply()`, which is a synchronous execution.<br>
`.join()` waits for all the processes in the pool to finish before moving on.<br>
`.close()` will close the pool, yet, in the example, `with` is used instead.

#### Synchronous execution
- `.map()`: takes in only single iterable argument `.map(function_name, [1, 2, 3])`
- `.starmap()`: takes in single iterable argument but iterable elements are allowed `.starmap(function_name, [(1, x, y), (2, x, y)])
- `.apply()`: takes in multiple arguments `.apply(function_name, args=(x, y, z))`

#### Asynchronous execution
- `.map_async()`
- `.starmap_async()`
- `.apply_async()`
~~~
def log_result(result):
  global results
  results.append(result)
    
def log_error(error):
  global errors
  errors.append(error)

if __name__ == '__main__':
  results = []
  errors = []
  pool = mp.Pool()
  for i in iteratable:
    pool.apply_async(function_name, args = (i, ), callback = log_result, error_callback = log_error)
  pool.close()
  pool.join()
~~~
`callback` & `error_callback` arguments are used in asynchronous methods to store result logs and error logs. If error occurs, then error log is stored instead of the result log. The logs get saved in the order of process completion.

## Ray
Ray provides more efficient parallel processing with large objects and numercial data, as well as, allowing to build microservices and actors that have states and can communicate.

### Installation
~~~
pip install ray
~~~

### Example
~~~
import ray
import time

# initiate ray process 
ray.init()

@ray.remote  # declares a remote function (to be executed remotely and asynchronously)
def f(x):
  time.sleep(1)
  return x

# execute the remote function with 4 tasks
result_ids = []
for i in range(4):
  result_ids.append(f.remote(i))  # .remote() is used to execute the remote function asynchronously

# retrieve results
results = ray.get(result_ids)  
# [0, 1, 2, 3]
~~~

## Reference
[Multiprocessing](https://lih-verma.medium.com/multi-processing-in-python-process-vs-pool-5caf0f67eb2b) <br>
[Ray](https://towardsdatascience.com/modern-parallel-and-distributed-python-a-quick-tutorial-on-ray-99f8d70369b8)
