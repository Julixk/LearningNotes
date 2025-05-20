# OpenMP

## 组成：
- 编译指导语句
- 库函数
- 环境变量

## 指导思想：
将工作划分为多个子任务分配给多个线程，从而实现多核并行处理单一的地址空间

## 编译指导语句的格式：
1. 
#pragma omp <directive> [clause[[,] clause]...]

directive部分：主要指令 —-> 用来指导多个CPU共享任务或指导多个CPU同步

clause部分：   选子句，给相应的指令参数


换行符：       必选项，位于被这个指令包围的结构块之前

2. 
编译指导语句以#pragma omp开始，后跟具体功能指令
常用指令如下：

parallel  ： 用在一个结构块之前，表示这段代码将被多个线程并行执行

for       ： 用于for循环语句之前，表示将循环计算任务分配到多个线程中并行执行，以实现任务分担，但必须保证每次循环之间无数据相关性

sections  ： 用在要被并行执行的代码段之前，用于实现多个结构块语句的任务分担，可并行执行的代码段各自用 section 指令标出

critical  ： 用在一段代码临界区之前，保证每次只有一个 OpenMP 线程进入

single    ： 用在并行域内，表示一段只被单个线程执行的代码

flush     ： 保证各个 OpenMP 线程的数据影像的一致性

barrier   ： 用于并行域内代码的线程同步，线程执行到 barrier 时要停下等待，直到所有线程都执行到 barrier 时才继续往下执行

4. 
子句，给相应的指令参数
常用子句如下：
private      | 指定一个或多个变量在每个线程中都有它自己的私有副本

shared       | 指定一个或多个变量为多个线程间的共享变量

default      | 用来指定并行域内的变量的使用方式，缺省是shared

firstprivate | 指定一个或多个变量在每个线程都有它自己的私有副本，并且私有变量要在进入并行域或任务分担域时，继承主线程中的同名变量的值作为初值

lastprivate  | 是用来指定将线程中的一个或多个私有变量的值在并行处理结束后复制到主线程中的同名变量中，负责拷贝的线程是for或sections任务分担中的最后一个线程

reduction    | 用来指定一个或多个变量是私有的，并且在并行处理结束后这些变量要执行指定的归约运算，并将结果返回给主线程同名变量

copyin       | 用来指定一个threadprivate类型的变量需要用主线程同名变量进行初始化 

## 头文件

** #include <omp.h> **


## 库函数

分类：
- 运行时环境函数
- 锁函数
- 时间函数分类

常用的 OpenMP 库函数如下：
- omp_in_parallel      |  判断当前是否在并行域中
- omp_set_num_threads  |  设置后续并行域中的线程数量
- omp_get_num_procs    |  返回计算系统中处理器的个数
- omp_get_num_threads  |  返回当前并行域中的线程数
- omp_get_thread_num   |  返回当前的线程号
- omp_get_max_threads  |  返回当前并行域中可用的最大线程数
- omp_get_dynamic      |  判断是否支持动态改变线程数量
- omp_set_dynamic      |  启用或关闭线程数量的动态改变
- omp_init_lock        |  初始化一个简单锁
- omp_set_lock         |  给一个简单锁上锁
- omp_unset_lock       |  给一个简单锁解锁
- omp_destroy_lock     |  关闭一个锁并释放内存
- omp_get_wtime        |  相对于某个任意参考时刻而言已经经历的时间 

## 变量作用域
共享变量：所有线程共同拥有的变量
私有变量：个别线程拥有的变量

全局变量
局部变量

private子句：

private(变量列表)


shared子句：

shared(变量列表)


default子句：

default(shared|none)


reduction子句：

reduction(运算符:变量列表)
