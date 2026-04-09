# Concurrency

Concurrency (different from parallelism) is the ability of a program to handle multiple tasks at the same time by overlapping their execution. Concurrency is all about structure (delicate balance or else your code can have nasty bugs).

## Concurrency vs Parallelism

While both are similar, concurrency is the concept of **dealing** with many things at once, while parallelism is about **doing** many things at once. With concurrency, there's no guarantee that you'll be doing multiple tasks at the same time but concurrent code is able to handle that. With parallelism, the code is executed in parallel at the same time requiring the exact same hardware and compute. Parallelism is a **subset** of concurrency.

> In general, processes are used for parallelism while tasks are used for concurrency

## Async / Await

In async/await, code can run asynchronously meaning the current thread will resume execution while that specific code gets processed in the background. Await allows you to pause until all async calls are satisfied.

## Process

By default, an OS creates one process which contains one main thread. A process is a completely independent execution environment with its own private memory and interpreter. You would spawn a new process when a task is CPU-bound, meaning your program spends most of its time performing heavy calculations (ie. training a ML model).

```python
import multiprocessing

def heavy_math(n):
    count = 0
    for i in range(10**7):
        count += i

if __name__ == "__main__":
    # Spawning 2 separate processes
    p1 = multiprocessing.Process(target=heavy_math, args=(1,))
    p2 = multiprocessing.Process(target=heavy_math, args=(2,))

    p1.start()
    p2.start()

    p1.join()
    p2.join()
```

## Threads

Smallest unit of execution an OS can manage. Threads share the same memory space as the parent thread. This lets them run incredibly fast, but can run into concurrency issues (ie. Race Condition). You would spawn a new thread when a task is I/O-bound, meaning most of its time is spent waiting for a response from a source outside of the CPU (ie. querying a database).

```python
import threading

def slow_io_task(name):
    time.sleep(2)  # Blocking wait

# Create threads
t1 = threading.Thread(target=slow_io_task, args=("A",))
t2 = threading.Thread(target=slow_io_task, args=("B",))

t1.start()
t2.start()

t1.join() # Wait for thread 1 to finish
t2.join() # Wait for thread 2 to finish
```

If your I/O-bound task supports `async/await`, it is actually preferred to use `tasks`.

## Tasks

In modern programming, a task is a higher-level abstraction that serves as a wrapper around a coroutine. A task won't stop until it hits an `await` expression. You can easily run hundreds of thousands of tasks on a single thread. This is normally the default for I/O-bound operations in python.

```python
import asyncio

async def fetch_data(id, delay):
    await asyncio.sleep(delay)  # Non-blocking wait
    return f"Data {id}"

async def main():
    # Schedule two tasks to run concurrently
    task1 = asyncio.create_task(fetch_data(1, 3))
    task2 = asyncio.create_task(fetch_data(2, 2))

    # Wait for both to finish
    results = await asyncio.gather(task1, task2)

asyncio.run(main())
```

> asyncio is the standard Python library used to write concurrent code.
