<!--
.. title: Python多线程学习
.. slug: pythonduo-xian-cheng-xue-xi
.. date: 2021-04-13 21:43:08 UTC+08:00
.. tags: python, 并发
.. category: python
.. link: 
.. description: 记录在python并发上的笔记
.. type: text
-->

> 理解python docs，首先是Executor Objects, Thread/ProcessPoolExecutor, 然后是Module Functions, 最后是Exception classes，不止是学习API，还要了解它的思维框架


## Intro
The concurrent.futures module provides a high-level interface for asynchronously executing callables

keyword: callables, high-level

## 基本框架
```python
for future in concurrent.futures.as_completed(future_to_url):
    url = future_to_url[future]
    try:
	data = future.result()
    except Exception as exc:
	print('%r generated an exception: %s' % (url, exc))
    else:
	print('%r page is %d bytes' % (url, len(data)))
```

## 提交任务
```python
executor.submit(task, args) # 创建并提交一个任务
executor.map(func, *iterables) # 对iterables中每个值都创建并提交一个任务
等价于:
for arg in iterables:
    executor.submit(func, arg)
```

## 查询任务本身状态？
```python
future.[cancel(), cancelled(), running(), done(), result(timeout=None), exception(timeout=None)]
```

## 下班
```python
executor.shutdown(wait=True) # 下班了!
wait=True会等待所有工作做完，才下班
```

## 有趣的方法！
```python
future.add_done_callback(fn): fn的唯一参数为future
```
>怀念js，怀念functional

## Executor Objects
abstract class: concurrent.futures.Executor(need to be implemented!)
继承关系：
> 继承包含实现继承和接口继承
```
Executor[abstract] -> Threadpoolexecutor(max_workers=1)[concrete]
		   -> Processpoolexecutor(max_workers=1)[concrete]
```

## ThreadPool or ProcessPool?
CPU bottleneck? => ProcessPool（亲测，如果是CPU bottleneck，要比ThreadPool快很多）

I/O bottleneck? => ThreadPool（如果bottleneck不在CPU上，就没必要用ProcessPool了，ThreadPool开销更小）

## Module Functions

1. 等待所有任务完成？（注意：这会阻塞当前线程）
```python
concurrent.futures.wait(fs, timeout=None, return_when=ALL_COMPLETED)
"""
return_when => 
FIRST_COMPLETED: any future finishes or is cancelled
FIRST_EXCEPTION: any future finishes by raising an exception or all completed
ALL_COMPLETED: when all future finishes or cancelled
"""
```

2. 获取所有已经完成的任务（或者已经被取消的任务）
```python
for future in concurrent.futures.as_completed(fs, timeout=None):
    ...
```

## Exception Classes
> 异常是程序无法语料之事，已经无法继续运行，当异常出现时，可以通过两种方式修补，一是提前预防(pre-process)，一种是通过适当的逻辑进行修补，提前判断和过滤，第二种是亡羊补牢(post-process)，在异常发生后，通过try...except进行捕获与处理.

```
1. CancelledError: Raised when a future is cancelled
2. TimeoutError: Raised when a future operation exceeds the given timeout
3. BrokenError: Derived from RuntimeError, raised when a executor is broken for some reason
4. InvalidStateError: an operation is performed on future that is not allowed in the curren state
5. BrokenThreadPool: when one of the workers of a ThreadPoolexecutor has failed initializing
6. BrokenProcessPool: when one of the workers of Processpoolexecutor has failed initializing
```

## FAQ
1. concurrent.futures vs multiprocessing, why concurrent.futures are preferred?

concurrent.futures在threading和multiprocessing上做了封装，以灵活性为代价提供了更简单的接口
(见)[https://stackoverflow.com/questions/20776189/concurrent-futures-vs-multiprocessing-in-python-3]
