# Multithreading and Concurrency in Java

## 1. Overview & Core Concepts
Multithreading enables concurrent execution of two or more parts of a program to maximize CPU utilization. In Java, concurrency is managed via the `java.lang.Thread` class, the `java.lang.Runnable` interface, and the `java.util.concurrent` package (introduced in Java 5 and enhanced in Java 8).

### Key Terms
* **Process**: An independent execution environment with its own memory space.
* **Thread**: The smallest unit of execution within a process; threads share the same heap memory space.
* **Concurrency**: Managing multiple tasks at the same time (interleaved execution on a single core or parallel on multi-core).
* **Parallelism**: Executing multiple tasks simultaneously across multiple CPU cores.

---

## 2. Creating Threads: `Runnable` vs `Callable`

| Feature | `Runnable` | `Callable` |
| :--- | :--- | :--- |
| **Return Value** | Does not return a result (`void`) | Returns a result via `Future<V>` |
| **Exception Handling** | Cannot throw checked exceptions | Can throw checked exceptions (`throws Exception`) |
| **Method Name** | `run()` | `call()` |

```java
import java.util.concurrent.*;

public class ThreadCreationExample {
    public static void main(String[] args) throws Exception {
        // 1. Using Runnable (Lambda expression)
        Runnable runnableTask = () -> System.out.println("Running via Runnable thread");
        new Thread(runnableTask).start();

        // 2. Using Callable with ExecutorService
        ExecutorService executor = Executors.newSingleThreadExecutor();
        Callable<String> callableTask = () -> {
            Thread.sleep(1000);
            return "Result from Callable task";
        };

        Future<String> futureResult = executor.submit(callableTask);
        System.out.println("Doing other work...");
        
        // blocking call to get result
        System.out.println(futureResult.get()); 
        executor.shutdown();
    }
}
```

---

## 3. Thread Lifecycle and States
A Java thread goes through 6 distinct states defined in `Thread.State`:

1. **NEW**: Thread created but not yet started (`start()` not called).
2. **RUNNABLE**: Executing in the Java virtual machine (ready or running).
3. **BLOCKED**: Waiting to acquire an intrinsic monitor lock to enter a synchronized block/method.
4. **WAITING**: Waiting indefinitely for another thread to perform a particular action (e.g., `Object.wait()`, `Thread.join()`).
5. **TIMED_WAITING**: Waiting for a specified period (e.g., `Thread.sleep(long)`, `Object.wait(long)`).
6. **TERMINATED**: Exited execution successfully or due to an unhandled exception.

---

## 4. Synchronization and Thread Safety
When multiple threads access shared mutable state concurrently, race conditions occur. Java uses monitors and locks to enforce thread safety.

### `synchronized` Keyword
Ensures mutual exclusion—only one thread can execute a synchronized block/method on the same object monitor at a time.

```java
public class Counter {
    private int count = 0;

    // Synchronized method
    public synchronized void increment() {
        count++;
    }

    // Synchronized block (finer-grained locking)
    public void incrementBlock() {
        synchronized (this) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}
```

### Explicit Locks (`ReentrantLock`)
Provides more flexibility than `synchronized` (e.g., try-lock, fair locks, interruptible locks).

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class CustomLockExample {
    private final Lock lock = new ReentrantLock();
    private int count = 0;

    public void performTask() {
        lock.lock(); // acquire lock
        try {
            count++;
        } finally {
            lock.unlock(); // always release in finally block
        }
    }
}
```

---

## 5. The `volatile` Keyword
The `volatile` keyword guarantees **visibility** of changes to variables across threads. When a variable is declared `volatile`, writes to it are flushed immediately to main memory, and reads bypass thread-local CPU caches.

* *Note*: `volatile` ensures visibility and prevents instruction reordering, but **does not** guarantee atomicity (e.g., `count++` is read-modify-write and requires synchronization or `AtomicInteger`).

---

## 6. Executor Service and Thread Pools
Managing individual threads manually is expensive. `ExecutorService` abstracts thread creation and management via thread pools.

```java
import java.util.concurrent.*;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        // Create a fixed thread pool with 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 5; i++) {
            int taskId = i;
            executor.submit(() -> {
                System.out.println("Task " + taskId + " executed by " + Thread.currentThread().getName());
            });
        }

        // Graceful shutdown
        executor.shutdown();
    }
}
```

### Common Thread Pool Types
* `newFixedThreadPool(int n)`: Reuses a fixed number of threads operating off a shared unbounded queue.
* `newCachedThreadPool()`: Creates threads as needed, but reuses previously constructed threads when available (good for short-lived asynchronous tasks).
* `newSingleThreadExecutor()`: Operates a single worker thread executing tasks sequentially.
* `newScheduledThreadPool(int corePoolSize)`: Supports delayed or periodic execution of tasks.

---

## 7. Introduction to `CompletableFuture` (Java 8)
`CompletableFuture` represents a future result of an asynchronous computation, supporting non-blocking fluent composition pipelines.

```java
import java.util.concurrent.*;

public class CompletableFutureExample {
    public static void main(String[] args) throws Exception {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            // Asynchronous task
            return "Hello from";
        }).thenApply(result -> {
            // Transform result
            return result + " CompletableFuture!";
        });

        // Block and retrieve result
        System.out.println(future.get()); // Hello from CompletableFuture!
    }
}
```

---

## 8. Concurrent Collections
Standard collections (`ArrayList`, `HashMap`) are not thread-safe and throw `ConcurrentModificationException` during concurrent mutations. The `java.util.concurrent` package provides thread-safe alternatives:

* **`ConcurrentHashMap`**: Uses fine-grained locking (bucket-level locking / CAS operations) instead of locking the entire map like `Hashtable` or `Collections.synchronizedMap`.
* **`CopyOnWriteArrayList`**: Creates a fresh cloned copy of the underlying array upon every mutation (add/remove). Ideal for read-heavy, write-rare scenarios.
* **`BlockingQueue`**: Thread-safe queue supporting producer-consumer patterns by blocking threads when attempting to dequeue from an empty queue or enqueue to a full queue.

---

## 9. Tricky Interview Questions & Answers

### Q1. What is the difference between `wait()` and `sleep()`?
* **`Thread.sleep()`**: A static method that pauses current thread execution for a specified duration **without releasing acquired monitor locks**.
* **`Object.wait()`**: Must be called within a synchronized context. It pauses execution **and releases the monitor lock** on the object, allowing other threads to enter synchronized blocks until `notify()` or `notifyAll()` is called.

### Q2. Why should you avoid `Executors.newCachedThreadPool()` in production high-load applications?
`newCachedThreadPool()` creates an unbounded number of threads (`Integer.MAX_VALUE`). Under heavy traffic, if tasks take time to complete, it can spawn thousands of threads, causing severe memory consumption, CPU thrashing, and eventually an `OutOfMemoryError`.

### Q3. How does `ConcurrentHashMap` achieve high concurrency in Java 8?
Unlike Java 7 (which used segment-based locking), Java 8 `ConcurrentHashMap` utilizes lock-free mechanisms using **CAS (Compare-And-Swap)** operations and synchronized blocks restricted strictly to individual hash bins (buckets) only when treeification or collision resolution occurs, significantly reducing contention.

### Q4. What is a Deadlock and how can you prevent it?
A deadlock occurs when two or more threads are blocked forever, waiting for each other to release locks. 
* **Prevention Strategies**: 
  1. Lock ordering (always acquire locks in a consistent global order).
  2. Lock timeout using `Lock.tryLock()`.
  3. Keep synchronized blocks small and avoid nested locks where possible.

---

## 10. Quick Revision
* **`Runnable` vs `Callable`**: `Runnable` returns `void` and no checked exceptions; `Callable` returns `Future<V>` and permits checked exceptions.
* **Thread States**: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED.
* **`synchronized` vs `ReentrantLock`**: `synchronized` is implicit and block-scoped; `ReentrantLock` offers explicit lock control, try-lock, and fairness policies.
* **`volatile`**: Guarantees variable memory visibility and stops instruction reordering, but *not* atomicity.
* **Executor Pools**: Prefer `ThreadPoolExecutor` or bounded pools over raw threads or unbounded cached pools.
* **Concurrent Maps**: Use `ConcurrentHashMap` for lock-free read performance and fine-grained updates.
