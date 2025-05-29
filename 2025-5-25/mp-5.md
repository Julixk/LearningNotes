# OpenMP并行编程技术 - 第五节：数据安全

## 一、数据竞争与数据安全的概念

### 什么是数据竞争（Data Race）
- 当多个线程**同时访问同一内存位置**，且至少有一个是**写操作**，且线程间**没有同步机制**时，就会产生数据竞争。
- 数据竞争会导致**程序行为不可预测**，产生**随机错误**。

### 数据共享与私有
- **共享变量（Shared）**：多个线程可读写同一份数据。
- **私有变量（Private）**：每个线程拥有该变量的一个副本，线程间互不干扰。

## 二、OpenMP中的变量共享控制

### 默认共享方式
- OpenMP 默认将大多数变量设为 **shared**，除非在 `parallel` 语句块中另行声明。

### 显式指定共享方式
使用 `shared()` 或 `private()` 指定变量的共享或私有属性：
```c
#pragma omp parallel shared(a, b) private(i)
````

### `firstprivate` 和 `lastprivate`

* **firstprivate**：私有变量的副本从主线程中复制初始化。
* **lastprivate**：在线程退出时将最后一次迭代的值写回主线程。

示例：

```c
#pragma omp parallel for firstprivate(x)
for (int i = 0; i < n; i++) { ... }
```

## 三、常见同步机制

### 1. 临界区（Critical Section）

```c
#pragma omp critical
{
    // 只允许一个线程进入此区域
}
```

### 2. 原子操作（Atomic）

* 用于对单一内存位置的简单读/写操作：

```c
#pragma omp atomic
sum += i;
```

### 3. 屏障（Barrier）

* 等待所有线程完成再往下执行：

```c
#pragma omp barrier
```

### 4. 单一执行（Single）

* 只由一个线程执行代码块：

```c
#pragma omp single
{
    // 仅一个线程执行
}
```

### 5. Master

* 仅主线程执行，不会阻塞其他线程：

```c
#pragma omp master
```

## 四、数据安全建议与最佳实践

* 避免使用全局变量或静态变量。
* 使用 `private` 避免变量被共享造成数据竞争。
* 尽量使用 `reduction`、`atomic` 等结构来安全地操作共享数据。
* 使用调试工具（如 Intel Inspector）检测数据竞争。

---

📌 **总结**：
OpenMP并行编程中，数据安全是关键。正确管理变量的共享和私有性，合理使用同步机制，可以有效避免数据竞争问题。
