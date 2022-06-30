# Thread States and Lifecycle

## Learning Goals

- Explain the various thread states.
- Explain a simplified thread lifecycle.

## Introduction

A thread goes through different states from its creation to termination. These
state changes can be caused by the programmer’s code or by the operating
system’s events. We’ll first take a look at the different states a thread can be
in and then look at a simplified lifecycle.

## Thread States

Thread states are represented by the `Thread.State` enum and a current thread’s
state can be examined using the `getState` instance method on a thread.

There are six possible values of thread state:

- `NEW`: a new thread instance has just been created but not started yet.
- `RUNNABLE`: the thread is being run by the JVM but it’s waiting for OS
  resources.
- `BLOCKED`: a thread is said to be blocked if it’s waiting for a monitor lock
  (discussed later).
- `WAITING`: the thread is waiting for another thread indefinitely (`join`
  without a timeout).
- `TIMED_WAITING`: the thread is waiting for another thread for the specified
  amount of time (`Thread.sleep`, `join` with a timeout defined).
- `TERMINATED`: a thread is said to have terminated when the `run` method has
  finished execution or an uncaught exception is thrown. Once terminated, a
  thread cannot go back to a runnable state.

Let’s look at some of these states in an example.

```java
class Example {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
            System.out.println("hi from the thread!");
        });

        System.out.println(t.getState());
        t.start();
        System.out.println(t.getState());
        System.out.println(t.getState());
        System.out.println(t.getState());
        t.join();
        System.out.println(t.getState());
    }
}
```

```java
NEW
RUNNABLE
RUNNABLE
TIMED_WAITING
hi from the thread!
TERMINATED
```

## Thread Lifecycle

The states in the `Thread.State`are from the JVM’s point of view. The actual
states are more complex. For example, the `RUNNABLE` state can describe a thread
that is ready to run or is already running. Here’s a simplified diagram of a
thread lifecycle:

![Thread lifecycle](https://curriculum-content.s3.amazonaws.com/java-mod-5/thread-lifecycle.png)

When a thread is initialized, it is in the `NEW` state. When the thread is
started it goes into the `RUNNING` state waiting for the thread scheduler to
allocate it resources for executing its instructions. The thread switches
between a “ready to run” state and a “running’.

The waiting state means that a thread’s execution is paused while it waits for
another thread or an I/O operation. A thread must be moved from this state to a
“ready to run” state to continue execution.

## Conclusion

We’ve learned about the various states of a thread and their lifecycle. As we
learn how to synchronize data and create safe, concurrent programs, this idea
about different thread states will be helpful in understanding more advanced
concepts.
