# MPI 并行编程技术 - 点对点通信

## 一、点对点通信简介

MPI（Message Passing Interface）中的点对点通信是指两个进程之间直接进行消息传递的方式。一个进程发送消息，另一个进程接收消息。点对点通信是 MPI 中最基本的通信方式，构建了更复杂通信模式的基础。

## 二、通信函数

### 1. 阻塞通信

* `MPI_Send(buffer, count, datatype, dest, tag, comm)`

  * 向指定进程发送消息，直到消息被复制到系统缓冲区或接收进程接收消息后，函数才返回。
* `MPI_Recv(buffer, count, datatype, source, tag, comm, status)`

  * 从指定进程接收消息，直到接收到消息后，函数才返回。

### 2. 非阻塞通信

* `MPI_Isend(buffer, count, datatype, dest, tag, comm, request)`

  * 发起一个非阻塞发送操作，立即返回，发送操作在后台进行。
* `MPI_Irecv(buffer, count, datatype, source, tag, comm, request)`

  * 发起一个非阻塞接收操作，立即返回，接收操作在后台进行。

非阻塞通信需要配合 `MPI_Wait` 或 `MPI_Test` 等函数来确保通信完成。

## 三、通信模式

MPI 提供了多种发送模式，以满足不同的通信需求：

* **标准模式（Standard Mode）**：使用 `MPI_Send`，系统决定是否缓存消息。
* **缓冲模式（Buffered Mode）**：使用 `MPI_Bsend`，需要用户提供缓冲区，适用于发送大量数据。
* **同步模式（Synchronous Mode）**：使用 `MPI_Ssend`，发送操作直到接收进程开始接收消息时才返回。
* **就绪模式（Ready Mode）**：使用 `MPI_Rsend`，要求接收操作已经被调用，适用于性能优化。

## 四、参数说明

* `buffer`：数据缓冲区，存放发送或接收的数据。
* `count`：发送或接收的数据项数量。
* `datatype`：数据类型，如 `MPI_INT`、`MPI_FLOAT` 等。
* `dest` / `source`：目标或源进程的编号（rank）。
* `tag`：消息标签，用于标识消息。
* `comm`：通信器，通常为 `MPI_COMM_WORLD`。
* `status`：接收操作的状态信息结构。
* `request`：非阻塞通信的请求句柄。

## 五、避免通信死锁

通信死锁是指两个或多个进程在等待彼此的消息，导致程序无法继续执行。常见的死锁场景包括：

* 两个进程都先调用 `MPI_Send`，然后调用 `MPI_Recv`，如果系统没有足够的缓冲区，可能导致死锁。
* 使用阻塞通信时，发送和接收操作的顺序不当。

避免死锁的方法：

* 使用非阻塞通信，如 `MPI_Isend` 和 `MPI_Irecv`，并配合 `MPI_Wait` 或 `MPI_Test`。
* 使用同步发送模式 `MPI_Ssend`，确保接收方已经准备好接收消息。
* 调整发送和接收操作的顺序，确保不会相互等待。

## 六、示例代码

以下是一个简单的点对点通信示例：

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    int rank, size, data;
    MPI_Init(&argc, &argv); // 初始化MPI环境
    MPI_Comm_rank(MPI_COMM_WORLD, &rank); // 获取当前进程编号
    MPI_Comm_size(MPI_COMM_WORLD, &size); // 获取总进程数

    if (rank == 0) {
        data = 100;
        MPI_Send(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD); // 发送数据到进程1
        printf("Process 0 sent data %d to process 1\n", data);
    } else if (rank == 1) {
        MPI_Recv(&data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE); // 从进程0接收数据
        printf("Process 1 received data %d from process 0\n", data);
    }

    MPI_Finalize(); // 结束MPI环境
    return 0;
}
```

