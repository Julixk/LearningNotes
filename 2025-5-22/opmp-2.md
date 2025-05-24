# OpenMP 常用指令&私有变量声明&规约操作

## 常用指令详解

### for循环并行：parallel for

**#pragma omp for**

parallel指令要初始化才能并行

数据竞争

在parallel区域由两个以上的线程对相同的共享变量同时进行写操作引起

子句collapse

嵌套循环中，collapse可以使代码在不嵌套并行的情况下对多重循环进行并行

```c 
#pragma omp parallel for collapse(2) shared (a,b,c,M,N,P) private(j)
for(int i = 0; i < M; i++)
    for(int k = 0; k < P; k++)
        for(int j = 0; j < N; j++)
            c[i,k] = c[i,k] + a[i,j] * b[j + k];
```

### 分段并行

分段并行(sections)主要用于非循环的代码并行。

若不同子任务之间不存在相互依赖的关系，可以将它们分配给不同的线程去执行。

若不同子任务之间存在相互依赖的关系,那么分段并行就很难实现。

```c
#pragma omp sections [private firstprivate lastprivate reduction nowait]
{
    #pragma omp section
    结构块
    #pragma omp section
    结构块
}

```

### Single

- 需要注意的是，在single后面会有一个隐含的栅障，因此在single部分执行期间，其他线程处于空闲状态。

- single只能由一个线程来执行，但是并不要求主线程。

- 不执行的其余线程会在single结束后同步，如果指令存在nowait，则不执行single的其余线程可以直接越过single结构向下执行。

```c
#pragma omp single [private firstprivate copyprivate nowait]
{
    结构块
}
```

### copyprivate

也是常用的一个子句，将线程里的私有变量在结构结束时广播给同一线程组的其余线程。


## 私有变量的声明

### 局部变量
- private

在定义并行区域内的私有变量的时候，private 子句会创建一个并行区域外原始变量的私有副本，并且和原始变量没有任何关联。

- firstprivate

如果想让私有变量继承原始变量的值，可以通过 firstprivate 子句来实现。

值得注意的是，firstprivate 并非使用初始变量，而是生成一个跟原始变量数值一样的副本。

- lastprivate

在并行区域内，当完成对子线程私有变量的计算后，lastprivate 可以将它的值传递给并行区域外的原始变量。

- copyprivate
### 全局变量
- threadprivate
- copyprivate

## 规约操作

累加求和、求差、求积等运算操作被称为规约操作
