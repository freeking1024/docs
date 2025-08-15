Java 中的两种核心同步机制：`synchronized` 和 `ReentrantLock`。它们都是可重入锁（Reentrant Lock），用于实现线程间的互斥和同步，但它们在实现、灵活性和使用方式上有着显著的区别。

---

### 1. `synchronized` 关键字

`synchronized` 是 Java 语言内置的关键字，用于实现同步。它可以修饰**实例方法**、**静态方法**或**代码块**。

#### 1.1 基本用法

*   **修饰实例方法**：
    ```java
    public synchronized void method() {
        // 方法体
    }
    ```
    *   **锁对象**：当前实例对象 `this`。
    *   **作用**：同一时刻，只有一个线程能进入该实例的这个方法。

*   **修饰静态方法**：
    ```java
    public static synchronized void staticMethod() {
        // 方法体
    }
    ```
    *   **锁对象**：该类的 `Class` 对象（`MyClass.class`）。
    *   **作用**：同一时刻，只有一个线程能进入该类的所有实例的这个静态方法。

*   **修饰代码块**：
    ```java
    public void method() {
        synchronized (this) { // 也可以是其他对象
            // 同步代码块
        }
    }
    ```
    *   **锁对象**：括号内的对象（如 `this`, `lockObj` 等）。
    *   **作用**：同一时刻，只有一个线程能进入以该对象为锁的同步代码块。

#### 1.2 特点

*   **内置（Intrinsic）**：是 JVM 内置的锁机制，由 JVM 直接支持。
*   **自动获取与释放**：
    *   进入 `synchronized` 方法或代码块时，自动获取锁。
    *   退出方法或代码块（正常返回或抛出异常）时，**自动释放锁**。这是 `synchronized` 最大的优点之一，避免了死锁风险（因忘记释放锁）。
*   **可重入性**：同一个线程可以多次获取同一个对象的锁。JVM 会记录锁的持有线程和持有计数。每次获取锁，计数加1；每次释放锁，计数减1；计数为0时，锁被完全释放。
*   **非公平锁**：`synchronized` 实现的是**非公平锁**。当锁被释放时，等待队列中的线程不按 FIFO 顺序竞争锁，新来的线程可能直接抢到锁，导致等待线程“饥饿”。
*   **不可中断**：线程在等待 `synchronized` 锁时，无法被其他线程通过 `Thread.interrupt()` 中断。它会一直阻塞，直到获得锁。
*   **不支持超时**：无法设置尝试获取锁的超时时间。要么立即获得，要么一直等待。
*   **不支持条件变量（Condition）**：`synchronized` 配合 `wait()`, `notify()`, `notifyAll()` 实现线程间的等待/通知，但这些方法是 `Object` 类的，不够灵活。`ReentrantLock` 提供了更强大的 `Condition` 接口。
*   **性能**：在 JDK 1.6 之后，`synchronized` 经历了大量优化（偏向锁、轻量级锁、自旋锁、锁消除、锁粗化），性能已经非常接近甚至在某些场景下优于 `ReentrantLock`。

---

### 2. `ReentrantLock` 类

`ReentrantLock` 是 `java.util.concurrent.locks` 包下的一个类，实现了 `Lock` 接口。它是一个显式的、可编程的锁。

#### 2.1 基本用法

```java
import java.util.concurrent.locks.ReentrantLock;

public class Example {
    private final ReentrantLock lock = new ReentrantLock();

    public void method() {
        lock.lock(); // 获取锁
        try {
            // 需要同步的代码
        } finally {
            lock.unlock(); // 释放锁（必须在 finally 块中）
        }
    }
}
```

#### 2.2 特点

*   **显式（Explicit）**：需要显式地调用 `lock()` 和 `unlock()` 方法。
*   **手动获取与释放**：
    *   必须在 `try` 块之前调用 `lock()`。
    *   必须在 `finally` 块中调用 `unlock()`。这是**强制要求**，否则一旦 `try` 块中抛出异常，锁将永远不会被释放，导致死锁。
*   **可重入性**：与 `synchronized` 一样，同一个线程可以多次获取同一个 `ReentrantLock` 实例。内部有持有线程和计数器。
*   **公平性可选**：
    *   `ReentrantLock()`：创建**非公平锁**（默认）。
    *   `ReentrantLock(boolean fair)`：`fair=true` 创建**公平锁**。公平锁会按照线程等待的 FIFO 顺序分配锁，减少“饥饿”，但性能开销更大。
*   **可中断**：提供了 `lockInterruptibly()` 方法。线程在等待锁时，可以被其他线程通过 `Thread.interrupt()` 中断，从而放弃等待并抛出 `InterruptedException`。
*   **支持超时**：提供了 `tryLock()` 和 `tryLock(long timeout, TimeUnit unit)` 方法。
    *   `tryLock()`：立即尝试获取锁，成功返回 `true`，失败返回 `false`。
    *   `tryLock(timeout, unit)`：在指定时间内尝试获取锁，成功返回 `true`，超时返回 `false`。
*   **支持多个条件变量（Condition）**：`ReentrantLock` 可以通过 `newCondition()` 方法创建多个 `Condition` 对象。这使得可以实现更复杂的线程间协调，比如“生产者-消费者”模式中，可以有多个不同的等待队列（如 `notFull`, `notEmpty`）。
*   **更丰富的监控方法**：提供了 `getHoldCount()`, `getQueueLength()`, `isHeldByCurrentThread()`, `hasQueuedThreads()` 等方法，便于监控锁的状态。

---

### 3. `synchronized` vs `ReentrantLock` 对比总结

| 特性 | `synchronized` | `ReentrantLock` |
| :--- | :--- | :--- |
| **实现方式** | JVM 内置关键字 | `java.util.concurrent.locks` 包下的类 |
| **获取/释放** | 自动（进入/退出代码块或方法） | 显式（`lock()` / `unlock()`，必须在 `finally` 中释放） |
| **可重入性** | 是 | 是 |
| **公平性** | 非公平（不可变） | 可选（默认非公平，可构造公平锁） |
| **可中断** | 否 | 是（`lockInterruptibly()`） |
| **超时获取** | 否 | 是（`tryLock()` / `tryLock(timeout)`） |
| **条件变量** | `wait()`/`notify()`/`notifyAll()`（每个对象一个） | `Condition`（一个 `Lock` 可创建多个 `Condition`） |
| **锁绑定** | 对象或 `Class` | `ReentrantLock` 实例 |
| **性能** | JDK 1.6+ 优化后很好 | 通常略好或相当，公平锁开销大 |
| **代码简洁性** | 简洁 | 稍繁琐（需 `try-finally`） |
| **灵活性** | 低 | 高 |

---

### 4. 如何选择？

*   **优先使用 `synchronized`**：
    *   在大多数情况下，`synchronized` 是首选。它足够简单、安全（自动释放锁）、性能优秀，且代码简洁。
    *   如果你的需求只是简单的互斥同步，`synchronized` 完全能满足。

*   **考虑使用 `ReentrantLock`**：
    *   **需要公平锁**：必须保证线程等待的公平性。
    *   **需要可中断的锁等待**：线程在等待锁时需要能够响应中断。
    *   **需要尝试获取锁（超时）**：需要避免无限等待，或者实现“超时放弃”的逻辑。
    *   **需要多个条件变量**：需要实现复杂的线程间协调，比如多个不同的等待/通知场景。

---

### 5. 总结

*   `synchronized` 和 `ReentrantLock` 都是强大的可重入互斥锁。
*   `synchronized` 是**简洁、安全、内置**的选择，适合大多数场景。
*   `ReentrantLock` 是**灵活、功能丰富、显式**的选择，提供了 `synchronized` 所不具备的高级特性（公平性、可中断、超时、多条件变量）。
*   选择哪个主要取决于你的具体需求。在没有特殊需求时，优先使用 `synchronized`。