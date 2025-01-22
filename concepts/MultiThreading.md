# Multithreading in Java

## Overview
Multithreading in Java allows a program to execute multiple threads simultaneously, enabling parallel execution and efficient CPU utilization. A thread is the smallest unit of a process, and multithreading enables multitasking within a single program.

---

## Topics

### 1. Basics of Threads
- **Definition**: A thread is a lightweight process. Each thread runs in the same process and shares resources like memory and file handles.
- **Thread Lifecycle**:
    - New
    - Runnable
    - Running
    - Blocked/Waiting
    - Terminated

#### Example
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " - Count: " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                System.out.println("Thread interrupted: " + e.getMessage());
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.setName("Thread-1");
        t2.setName("Thread-2");

        t1.start(); // Starts a new thread
        t2.start();
    }
}
```

This example demonstrates two threads running concurrently, each printing its progress and pausing briefly.

#### Interview Questions
1. **What is the difference between a process and a thread?**
    - A process is an independent execution unit with its own memory space, while a thread is a lightweight sub-process that shares memory and resources of the process.
2. **What are the different states of a thread?**
    - New, Runnable, Running, Blocked/Waiting, and Terminated.

---

### 2. Thread Creation
- **Using Thread class**:
  Extend the `Thread` class and override the `run` method.
- **Using Runnable interface**:
  Implement the `Runnable` interface and pass it to a `Thread` object.

#### Example
```java
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println(Thread.currentThread().getName() + " - Task " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyRunnable(), "Worker-1");
        Thread t2 = new Thread(new MyRunnable(), "Worker-2");

        t1.start();
        t2.start();
    }
}
```

This example highlights the separation of task logic (in `MyRunnable`) from the thread mechanism.

#### Interview Questions
1. **Which approach is better for thread creation: extending `Thread` or implementing `Runnable`?**
    - Implementing `Runnable` is better as it allows the class to inherit from other classes and provides better separation of thread logic.
2. **Why is `Runnable` preferred in some cases?**
    - It is preferred because it promotes better design by separating the task from the thread mechanism.

---

### 3. Thread Synchronization
- **Why Synchronization?** To avoid inconsistent behavior when multiple threads access shared resources.
- **Synchronized Methods**: Synchronize a method to allow only one thread to execute it at a time.
- **Synchronized Blocks**: Synchronize a block of code for more fine-grained control.

#### Example
```java
class SharedResource {
    synchronized void printNumbers(int n) {
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + " - " + n * i);
            try {
                Thread.sleep(400);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class MyThread extends Thread {
    SharedResource sr;

    MyThread(SharedResource sr) {
        this.sr = sr;
    }

    public void run() {
        sr.printNumbers(5);
    }
}

public class Main {
    public static void main(String[] args) {
        SharedResource obj = new SharedResource();

        MyThread t1 = new MyThread(obj);
        MyThread t2 = new MyThread(obj);

        t1.start();
        t2.start();
    }
}
```

This ensures only one thread accesses the `printNumbers` method at a time.

#### Interview Questions
1. **What is the purpose of thread synchronization?**
    - It prevents concurrent access to critical sections, avoiding data inconsistency.
2. **Explain the difference between synchronized methods and synchronized blocks.**
    - Synchronized methods lock the entire object, while synchronized blocks lock only a specific section of code.
3. **What is a reentrant lock?**
    - A lock that allows a thread to reacquire it multiple times without blocking itself.

---

### 4. Race Conditions
- **Definition**: Occurs when multiple threads access shared data and try to change it simultaneously, leading to unpredictable outcomes.

#### Example of Race Condition
```java
class Counter {
    private int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Final count: " + counter.getCount());
    }
}
```

#### Handling Race Conditions
1. Use synchronized blocks or methods.
2. Use `java.util.concurrent.locks.ReentrantLock`.
3. Use `Atomic` classes from `java.util.concurrent.atomic`.

#### Fixed Example
```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

#### Interview Questions
1. **What is a race condition?**
    - It is a situation where the behavior of a program depends on the sequence or timing of thread execution.
2. **How can you prevent race conditions in Java?**
    - Use synchronization, locks, or atomic classes to ensure thread safety.
3. **What are `Atomic` classes?**
    - They are classes in `java.util.concurrent.atomic` that provide low-level atomic operations for thread safety.

---

### 5. Deadlocks
- **Definition**: A deadlock occurs when two or more threads are waiting for each other to release resources, preventing all involved threads from proceeding.

#### Example of Deadlock
```java
class Resource {
    void methodA(Resource resource) {
        synchronized (this) {
            System.out.println(Thread.currentThread().getName() + " locked resource A");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (resource) {
                System.out.println(Thread.currentThread().getName() + " locked resource B");
            }
        }
    }

    void methodB(Resource resource) {
        synchronized (this) {
            System.out.println(Thread.currentThread().getName() + " locked resource B");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (resource) {
                System.out.println(Thread.currentThread().getName() + " locked resource A");
            }
        }
    }
}

public class DeadlockExample {
    public static void main(String[] args) {
        Resource res1 = new Resource();
        Resource res2 = new Resource();

        Thread t1 = new Thread(() -> res1.methodA(res2), "Thread-1");
        Thread t2 = new Thread(() -> res2.methodB(res1), "Thread-2");

        t1.start();
        t2.start();
    }
}
```

#### Avoiding Deadlocks
1. **Lock Ordering**: Always acquire locks in a consistent order.
2. **Timeouts**: Use time-bound locks like `tryLock()` from `ReentrantLock`.
3. **Deadlock Detection**: Use tools or algorithms to detect potential deadlocks.
4. **Avoid Nested Locks**: Minimize the use of nested synchronized blocks.

#### Extended Example with Deadlock Prevention
```java
import java.util.concurrent.locks.ReentrantLock;

class Resource {
    private final ReentrantLock lock = new ReentrantLock();

    void safeMethod(Resource other) {
        boolean acquiredThis = lock.tryLock();
        boolean acquiredOther = other.lock.tryLock();

        if (acquiredThis && acquiredOther) {
            try {
                System.out.println(Thread.currentThread().getName() + " acquired both locks");
            } finally {
                other.lock.unlock();
                lock.unlock();
            }
        } else {
            if (acquiredThis) lock.unlock();
            if (acquiredOther) other.lock.unlock();
            System.out.println(Thread.currentThread().getName() + " could not acquire locks");
        }
    }
}

public class DeadlockPrevention {
    public static void main(String[] args) {
        Resource res1 = new Resource();
        Resource res2 = new Resource();

        Thread t1 = new Thread(() -> res1.safeMethod(res2), "Thread-1");
        Thread t2 = new Thread(() -> res2.safeMethod(res1), "Thread-2");

        t1.start();
        t2.start();
    }
}
```

This example demonstrates the use of `tryLock()` to avoid deadlocks by only acquiring locks if both can be acquired.

#### Interview Questions
1. **What is a deadlock?**
    - A situation where two or more threads are waiting indefinitely for each other to release locks.
2. **How can you prevent deadlocks?**
    - Use consistent lock ordering, timeouts, and avoid nested locks.
3. **What is the role of `ReentrantLock` in deadlock prevention?**
    - `ReentrantLock` provides more flexibility, such as timeout-based lock acquisition and interruptible locks.

---

### 6. Advanced and Tricky Interview Questions
1. **What happens if a thread throws an uncaught exception?**
    - The thread terminates, and the exception is propagated to the `UncaughtExceptionHandler` if one is set.
2. **How does Java handle thread priorities?**
    - Thread priorities are hints to the scheduler and may not be strictly enforced.
3. **Explain the difference between `volatile` and `synchronized`.**
    - `volatile` ensures visibility of changes to variables across threads but does not provide atomicity, while `synchronized` ensures both visibility and atomicity.
4. **Can you synchronize a static method?**
    - Yes, it locks the class object instead of an instance.
5. **How do you implement a producer-consumer pattern in Java?**
    - Use a shared queue with `wait()` and `notify()` or use `BlockingQueue` from `java.util.concurrent`.
6. **Explain Fork/Join Framework in Java.**
    - It is designed for parallel processing by recursively breaking down tasks into smaller subtasks using `ForkJoinPool`.
7. **What is a ThreadLocal variable?**
    - It provides thread-local storage, ensuring each thread has its own isolated copy of a variable.
8. **Can you explain the concept of work-stealing?**
    - In the Fork/Join framework, idle threads can "steal" tasks from the queues of busy threads to optimize resource utilization.

---

## Conclusion
Multithreading in Java is a powerful feature that can significantly improve application performance when used correctly. However, improper handling of threads can lead to issues like race conditions, deadlocks, and reduced maintainability. A solid understanding of the concepts and best practices is essential for building robust multithreaded applications.
