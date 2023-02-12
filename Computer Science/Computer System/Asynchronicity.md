# Asynchronicity

## Table of Contents
- [Background](#background)
- [Sequential vs Concurrent vs Parallel](#sequential-vs-concurrent-vs-parallel)
- [Blocking vs Non-Blocking](#blocking-vs-non-blocking)
  - [Blocking](#blocking)
  - [Non-Blocking](#non-blocking)
- [Synchronous vs Asynchronous](#synchronous-vs-asynchronous)
  - [Synchronous](#synchronous)
  - [Asynchronous](#asynchronous)
  - [Thread(s) and Sync/Async](#threads-and-syncasync)
- [Blocking/Non-Blocking vs Sync/Async](#blockingnon-blocking-vs-syncasync)
  - [Blocking Sync](#blocking-sync)
  - [Non-Blocking Sync](#non-blocking-sync)
  - [Non-Blocking Async](#non-blocking-async)
  - [Blocking Async](#blocking-async)

## Background
<p align="center">
  <img src="https://raw.githubusercontent.com/karlrupp/microprocessor-trend-data/master/50yrs/50-years-processor-trend.png" alt="50 Years of Microprocessor Trend" width="80%" /><br/>
  <em>50 Years of Microprocessor Trend</em><br/>
  <em>https://github.com/karlrupp/microprocessor-trend-data</em>
</p>

- By nature, all (hardware) electronics operate _synchronously_.
  - Ex: a computer operations in accordance to the clock (rising/falling edge).
- This applies to software programs as well.
  - When funcA calls funcB internally, funcA has to wait for funcB to resolve before continuing to the next line of code. &larr; **_Synchronous and Blocking_** | **_start of multiprocess era_**
- **Problem:** But this means that when a large function is executing, code behind it will have to wait a long time for their turn.
  - Instead of waiting, why not separate these and allow funcA to continue?
    - funcA encounters funcB and delagates it (Ex: to another thread) while continuing through the rest of its code. &larr; **_Non-Blocking_**
- **Problem:** But funcA will have to keep checking on whether funcB is resolved or not.
  - Instead of having to continuously check on funcB, why not completely delegate it (Ex: to another thread)?
    - funcA encounters funcB, delegates it and forgets about it. funcA goes on about executing. When funcB resolves, it is placed in a callback queue to be retrieved by funcA when funcA is ready. &larr; **_Asynchronous ft. callbacks/closure_** | **_start of multithreading era_**
  - The use of multiple threads greatly improved performance (since thread-switching is cheaper than process-switching).
- **Problem:** Infinite number of threads does not mean better task/time efficiency. There is a point where thread-switching becomes a problem where there are too many idle threads (economies of scale).
  - Instead of creating a thread for every single thing, have a certain amount and work with those.
- **Problem:** Is the program thread safe?
  - Multithreading is prone to race condition.
    - Ex: working with global variables.
  - Instead developers in most cases should not personally make threads. Rather, focus on a smaller unit (closure/callback) and the OS will manage the threads, queuing etc.
- **Problem:** Callback Hell
  - The use of asynchronous callbacks can lead to deeply nested callbacks.
  - Therefore, handle asynchronous operations using `async/await`, `Rx`, etc.

## Sequential vs Concurrent vs Parallel
- **Sequential:** executing tasks one by one.
- **Concurrent:** dealing with multiple tasks at once.
- **Parallel:** executing multiple tasks at once.

## Blocking vs Non-Blocking
- 제어의 관점.
- “Does the callee immediately return? Effectively returning control of execution to the caller?”
- "Who has the control of execution and when is it returned?"
### Blocking
- The caller's control of execution is passed to the callee. Therefore it has to wait for the callee to finish executing first.
  - i.e. the callee keeps hold of the caller’s control of execution. The callee returns the caller's control of execution along with its result.
### Non-Blocking
- The caller's control of execution is passed to the callee. But the callee immediately returns the control of execution along with a "result" indicating that it is not ready to return the actual result yet.
  - i.e. the caller keeps hold of its control of execution. The callee may have returned its result along with it or is yet to return its result.
### Example - when funcA calls funcB
- **Blocking:** funcA has no control of execution over itself. It has to wait for funcB to finish executing.
- **Non-Blocking:** funcA has control of execution over itself. It does not have to wait for funcB to finish executing.

## Blocking vs Non-Blocking
- 제어의 관점.
- “Does the callee immediately return? Effectively returning control of execution to the caller?”
- "Who has the control of execution and when is it returned?"
### Blocking
- The caller's control of execution is passed to the callee. Therefore it has to wait for the callee to finish executing first.
  - i.e. the callee keeps hold of the caller’s control of execution. The callee returns the caller's control of execution along with its result.
### Non-Blocking
- The caller's control of execution is passed to the callee. But the callee immediately returns the control of execution along with a "result" indicating that it is not ready to return the actual result yet.
  - i.e. the caller keeps hold of its control of execution. The callee may have returned its result along with it or is yet to return its result.
### Example - when funcA calls funcB
- **Blocking:** funcA has no control of execution over itself. It has to wait for funcB to finish executing.
- **Non-Blocking:** funcA has control of execution over itself. It does not have to wait for funcB to finish executing.

## Synchronous vs Asynchronous
- 제어권/결과 반환 시점의 관점 (기다리고 말고를 떠나서).
- "Are the control of execution and result returned at the same time or not?"
### Synchronous
- **Sequential execution of tasks.**
- The order of execution is predetermined.
  - i.e. the caller personally checks/receives the callee’s finished execution.
- The control of execution and "result" (indicating that it doesn't have a result yet) are returned at the same time.
### Asynchronous
- **Concurrent execution of tasks.**
  - i.e. other things can happen independently of the main program flow.
- The order of execution is indeterminate.
  - i.e. the caller does not actively care whether the callee has finished executing or not.
  - Hence, the need for some sort of queue.
- The control of execution is returned first. The result is returned later indirectly (callback queue).
- _**In relation to events, asynchronous programming is a way to deal with events independently of the main program flow.**_
- Example
  - `setTimeout` is encountered (top stack of the call stack).
  - `setTimeout` is taken from the call stack and sent to its corresponding browser API.
  - When the API is done, it sends it to the event queue.
  - When the main flow is ready, it receives the resolved `setTimeout` from the event queue.
### Example - when funcA calls funcB
- **Sync:** func A has to wait for func B to finish first before resuming execution.
  - i.e. func A waits for func B’s result.
- **Async:** func A does not have to wait for func B to finish first before resuming execution.
  - i.e. func A continues executing and later, when possible, receives func B’s result.
### Thread(s) and Sync/Async
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/threads-and-sync-async.png" alt="Thread(s) and Sync/Async" width="100%" />
</p>
#### Async vs Multithreading
- Async is about concurrent execution of _tasks_ without having to block the main thread of execution.
  - Async can be either multithreaded or singlethreaded.
- Multithreading simply refers to concurrent executions of _threads_.
  - It is not concerned with synchronity/asynchronity.

## Blocking/Non-Blocking vs Sync/Async
- Sync/Async is an abstract concept. Therefore, it can sometimes be the same as Blocking/Non-Blocking.
- In order of development.
### Blocking Sync
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/blocking-sync.png" alt="Blocking Sync" width="60%"/>
</p>

- The callee keeps hold of the caller's control of execution (blocking), and the caller waits to see the callee resolve (sync).
- Ex: waiting on user input, fetching from database.
### Non-Blocking Sync
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/non-blocking-sync.png" alt="Non-Blocking Async" width="60%"/>
</p>

- The callee immediately returns the caller's control of execution (non-blocking), and while resuming execution, the caller periodically checks if the callee is resolved (sync).
### Non-Blocking Async
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/non-blocking-async.png" alt="Non-Blocking Async" width="60%"/>
</p>

- The caller immediately returns the caller's control of execution (non-blocking), and while resuming execution, the caller does not check to see if the callee is resolved (async). Rather, the caller retrieves the resolved callback from the callback queue.
### Blocking Async
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/blocking-async.png" alt="Blocking Async" width="60%"/>
</p>

- The callee keeps hold of the caller's control of execution (blocking), and the caller does not check whether the callee is resolved (async). Rather, the caller retrieves the resolved callback from the callback queue.
- _This approach is usually not used as it doesn't offer much merit._

## Reference
[동기 vs 비동기 (feat. blocking vs non-blocking)](https://velog.io/@wonhee010/%EB%8F%99%EA%B8%B0vs%EB%B9%84%EB%8F%99%EA%B8%B0-feat.-blocking-vs-non-blocking)  
[[10분 테코톡] 🎧 우의 Block vs Non-Block & Sync vs Async - YouTube](https://www.youtube.com/watch?v=IdpkfygWIMk&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9C%ED%85%8C%ED%81%AC)  
[Asynchronous vs synchronous execution. What is the difference? - Stack Overflow](https://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-is-the-difference)  
[The Difference Between Asynchronous And Multi-Threading | Baeldung on Computer Science](https://www.baeldung.com/cs/async-vs-multi-threading)  
[What are the differences between blocking synchronous, nonblocking synchronous, blocking asynchronous, and nonblocking asynchronous? : learnprogramming](https://www.reddit.com/r/learnprogramming/comments/apkteq/what_are_the_differences_between_blocking/)  
[c# - What is the difference between asynchronous programming and multithreading? - Stack Overflow](https://stackoverflow.com/questions/34680985/what-is-the-difference-between-asynchronous-programming-and-multithreading)  
