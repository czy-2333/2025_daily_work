# OS

## 1 进程控制

进程是操作系统中程序执行的一个实例，是资源分配和调度的基本单位，每个进程都有一个独有的进程ID **`PID`**

一个进程不仅仅是代码的运行，还包括了其所需的资源和运行状态，且每个进程由操作系统独立管理和调度，保证它们能够并行运行



特点：

-   独立性： 每个进程都有自己独立的地址空间（包括代码段、数据段、堆和栈等），因此一个进程的崩溃或错误不会直接影响其他进程
-   资源分配单位： 操作系统为每个进程分配所需的资源（CPU、内存、文件等）
-   并发性： 在单核系统中，进程通过时间片轮转实现并发执行；在多核系统中，进程可以同时在多个核心上并行执行
-   进程状态： 进程有不同的状态，如：**就绪**（等待CPU调度）、**运行**（CPU正在执行进程）、**阻塞**（等待资源或事件发生）等



注意：==进程不是线程==

**进程**：是系统资源分配的单位，拥有独立的内存空间，进程间的切换代价较大

**线程**：是进程的执行单元，一个进程可以包含多个线程，线程共享进程的资源（如内存），切换开销较小





操作进程的头文件有很多，例如 `<sys/types.h>`，`<unistd.h>`，`<fcntl.h>`

### 1.1 <sys/types.h＞

`<sys/types.h>` 头文件定义了一些与系统相关的类型，通常用于系统编程



**注意**：

`<sys/types.h>` 常常不需要显式包含，因为很多情况下它已经隐式包含在其他头文件中

当你使用 `<unistd.h>` 或 `<sys/wait.h>` 时，系统会自动包含 `<sys/types.h>`，因为这些头文件依赖其中定义的数据类型



`<sys/types.h>` 主要包含的数据类型有：

*   ==pid_t==：表示进程ID，用于标识系统中的一个进程

*   `uid_t`，`gid_t`：表示用户ID和组ID，通常用于控制文件访问权限或用户管理

*   `off_t`：用于表示文件中的偏移量，常在文件操作中使用

*   `mode_t`：表示文件的权限或模式，比如读写权限

*   `dev_t`：表示设备号，通常用于描述设备文件





### 1.2 <unistd.h>

`<unistd.h>` 是 Unix 系统中最常见的头文件之一，主要提供了一些 POSIX（可移植操作系统接口）标准的系统调用函数，用于与操作系统底层进行交互

它包含了文件操作、进程控制、管道等功能





#### 1.2.1 fork
- **原型**：
  ```c
  pid_t fork(void);
  ```
- **参数**：无
- **返回值**：
  - **父进程**：返回子进程的 PID（正整数）
  - **子进程**：返回 `0`
  - **失败**：==返回 -1==，并设置 `errno`
  
- **作用**：创建一个新进程（子进程），它是父进程的副本。子进程继承了父进程的所有数据、文件描述符等

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    pid_t pid = fork();
    if (pid == 0)
        printf("这是子进程\n");
    else if (pid > 0)
        printf("这是父进程，子进程的PID是：%d\n", pid);
    else
        perror("fork失败");
    return 0;
}
```



#### 1.2.2 exec

`<unistd.h>` 中包含多种 `exec` 函数族，用于替换当前进程映像，执行另一个程序

常见的函数包括 `execl`、`execv`、`execle`、`execve` 等，它们的作用相同，但参数形式不同

1.   execl

*   **原型**：

```c
int execl(const char *path, const char *arg, ...);
```

- **参数**：
  - `path`：要执行的程序的路径
  - `arg`：要传递给程序的参数列表（参数以 `NULL` 结束）

- **返回值**：成功时`execvp` 不会返回，它会用新的程序替换当前进程；失败时返回 `-1`，并设置 `errno`

- **作用**：用指定的程序替换当前进程映像，程序路径由 `path` 指定，传递的参数为一组字符串

2.   execvp

*   **原型**：

```c
int execvp(const char *file, char *const argv[]);
```

*   **参数**：

    *   `file`：要执行的程序名称，可以是程序的文件名，也可以是完整路径或相对地址（如果只是文件名，`execvp` 会从 `PATH` 环境变量中查找该文件）

        ​			`argv[0]` 存储的可执行文件的地址，传入参数需要从 `argv[1]` 开始存入

    *   `argv`：是一个参数数组，`argv[0]` 通常是程序名，后面的元素是该程序运行时所需的命令行参数（这个数组必须以 `NULL` 结束）

*   **返回值**：成功时`execvp` 不会返回，它会用新的程序替换当前进程；失败时返回 `-1`，并设置 `errno`

    ### 2. **使用示例**





**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    execl("/bin/ls", "ls", "-l", NULL);
    // 如果execl成功执行，下面的代码不会运行
    perror("execl失败");
    return 1;
}
```



#### 1.2.3 getpid 和 getppid

- **原型**：
  ```c
  pid_t getpid(void);
  pid_t getppid(void);
  ```
  
- **返回值**：
  - `getpid`：返回当前进程的进程 ID
  - `getppid`：返回当前进程的父进程 ID

- **作用**：获取当前进程和父进程的进程 ID

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    printf("当前进程的PID: %d\n", getpid());
    printf("父进程的PID: %d\n", getppid());
    return 0;
}
```



#### 1.2.4 sleep

- **原型**：
  ```c
  unsigned int sleep(unsigned int seconds);
  ```
- **参数**：
  - `seconds`：要休眠的秒数
  
- **返回值**：如果 `sleep` 正常结束，返回 0；如果被信号打断，返回剩余的未休眠时间

- **作用**：让进程休眠指定的秒数

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    printf("程序开始...\n");
    sleep(5);  // 休眠5秒
    printf("程序结束...\n");
    return 0;
}
```



#### 1.2.5 access

- **原型**：
  ```c
  int access(const char *pathname, int mode);
  ```
- **参数**：
  
  - `pathname`：文件路径
  - `mode`：检查文件权限，可以是 `F_OK`（文件是否存在）、`R_OK`（可读）、`W_OK`（可写）、`X_OK`（可执行）
  
- **返回值**：成功返回 `0`，失败返回 `-1` 并设置 `errno`

- **作用**：检查当前进程对文件的访问权限

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    if (access("test.txt", F_OK) == 0)
        printf("文件存在\n");
    else
        printf("文件不存在\n");

    return 0;
}
```





#### 1.2.6 getcwd

- **原型**：
  ```c
  char *getcwd(char *buf, size_t size);
  ```
- **参数**：
  - `buf`：用于存储当前工作目录的缓冲区
  - `size`：缓冲区的大小
  
- **返回值**：成功时返回当前工作目录的指针，失败时返回 `NULL`，并设置 `errno`

- **作用**：获取当前进程的工作目录，并将其存储到 `buf` 中

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    char cwd[1024];
    if (getcwd(cwd, sizeof(cwd)) != NULL)
        printf("当前工作目录: %s\n", cwd);
    else
        perror("getcwd失败");
    return 0;
}
```



#### 1.2.7 chdir

- **原型**：
  ```c
  int chdir(const char *path);
  ```
- **参数**：
  - `path`：要切换到的目录路径
  
- **返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`

- **作用**：更改当前工作目录

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    if (chdir("/tmp") == 0)
        printf("工作目录切换到 /tmp\n");
    else
        perror("chdir失败");
    return 0;
}
```







#### 1.2.8 案例：自定义shell

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

#define MAX_CMD_LENGTH 1024

int main()
{
    char command[MAX_CMD_LENGTH];
    pid_t pid;
    int status;

    while (1)
    {
        printf("myshell> ");

        if (fgets(command, sizeof(command), stdin) == NULL)
        {
            perror("fgets failed");
            continue;
        }

        command[strcspn(command, "\n")] = 0;

        if (strcmp(command, "exit") == 0)
            break;

        pid = fork();
        if (pid < 0)
        {
            perror("fork failed");
            continue;
        }

        else if (pid == 0)
        {
            char *args[] = {"/bin/sh", "-c", command, NULL};
            execvp(args[0], args);
            /*
            	参数1 /bin/sh 系统命令解释器（也称为Shell）
            	参数2 /bin/sh -c 调用 /bin/sh 下的 -c 指令,将之后的参数由地址转换为命令
            */
            perror("exec failed");
            exit(1);
        }
        else
            waitpid(pid, &status, 0);
    }

    return 0;
}

```









### 1.3 <sys/wait.h>

`<sys/wait.h>` 头文件主要提供了一些与进程终止和等待相关的函数和宏，用于父进程**等待子进程**结束，处理子进程的退出状态，以及控制进程的终止行为等



#### 1.3.1 wait
- **原型**：
  
  ```c
  pid_t wait(int *status);
  ```
- **参数**：
  
  - `status`：一个指向整数的指针，用于存储子进程的终止状态，如果不关心子进程的退出状态，可以将其设置为 `NULL`
  
- **返回值**：
  
  - 成功时返回终止的子进程的 PID，失败时返回 `-1` 并设置 `errno`
  
- **作用**：父进程调用 `wait` 函数会阻塞，直到它的某个子进程结束。`status` 参数会存储子进程的退出状态

**例**：

```c
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    pid_t pid = fork();  // 创建子进程
    if (pid == 0)
    {
        // 子进程
        printf("子进程运行中...\n");
        sleep(2);  // 模拟子进程任务
        exit(0);   // 子进程退出
    }
    else
    {
        // 父进程
        int status;
        wait(&status);  // 等待子进程结束
        if (WIFEXITED(status))
            printf("子进程正常退出，退出状态：%d\n", WEXITSTATUS(status));
    }
    return 0;
}

/*
	子进程运行中...
	子进程正常退出，退出状态：0
*/
```






#### 1.3.2 waitpid
- **原型**：
  
  ```c
  pid_t waitpid(pid_t pid, int *status, int options);
  ```
  
- **参数**：
  - `pid`：指定要等待的子进程的 PID。如果 `pid` 为 `-1`，表示等待任意子进程；如果为正数，则等待指定 PID 的子进程
  - `status`：与 `wait` 函数中的 `status` 类似，用于存储子进程的退出状态
  - `options`：可以是 `0`（默认行为），也可以是 `WNOHANG`（非阻塞模式），或者其他选项（如 `WUNTRACED`，用于获取停止的子进程）


  | 宏        | 说明                                                         |
  | --------- | ------------------------------------------------------------ |
  | WNOHANG   | 如果pid指定的子进程没有结束，则waitpid()函数立即返回0，而不是阻塞在这个函数上　　　　　　　　　　　　　　　　　　　　　　等待如果结束了，则返回该子进程的进程号 |
  | WUNTRACED | 如果子进程进入暂停状态，则马上返回                           |

- **返回值**：
  - 成功时返回终止的子进程的 PID，失败时返回 `-1` 并设置 `errno`。如果使用 `WNOHANG`，且没有子进程终止，返回 `0`
  
- **作用**：`waitpid` 比 `wait` 提供了更灵活的控制，可以选择特定子进程进行等待，并且可以设置选项以非阻塞方式运行

**例**：

```c
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    pid_t pid = fork();  // 创建子进程
    if (pid == 0)
    {
        // 子进程
        printf("子进程运行中...\n");
        sleep(2);  // 模拟子进程任务
        exit(0);   // 子进程退出
    }
    else
    {
        // 父进程
        int status;
        while (waitpid(pid, &status, WNOHANG) == 0)
        {
            printf("父进程等待子进程结束...\n");
            sleep(1);  // 每1秒检查一次
        }
        if (WIFEXITED(status))
            printf("子进程正常退出，退出状态：%d\n", WEXITSTATUS(status));
    }
    return 0;
}

/*
    子进程运行中...
    父进程等待子进程结束...
    父进程等待子进程结束...
    子进程正常退出，退出状态：0
*/
```




#### 1.3.3 wait3
- **原型**：
  
  ```c
  pid_t wait3(int *status, int options, struct rusage *rusage);
  ```
- **参数**：
  
  - `status`：用于存储子进程的终止状态
  - `options`：可以是 `0` 或与 `waitpid` 中的选项类似（如 `WNOHANG`）
  - `rusage`：指向 `struct rusage` 类型的指针，用于存储子进程的资源使用情况。如果不需要此信息，可以传递 `NULL`
  
- **返回值**：成功时返回终止的子进程的 PID，失败时返回 `-1` 并设置 `errno`

- **作用**：类似于 `waitpid`，但可以收集子进程的资源使用情况



#### 1.3.4 wait4
- **原型**：
  
  ```c
  pid_t wait4(pid_t pid, int *status, int options, struct rusage *rusage);
  ```
- **参数**：
  
  - `pid`：要等待的子进程 PID（与 `waitpid` 类似）
  - `status`：用于存储子进程的终止状态
  - `options`：与 `waitpid` 的选项类似
  - `rusage`：用于存储子进程的资源使用信息（与 `wait3` 类似）
  
- **返回值**：成功时返回终止的子进程的 PID，失败时返回 `-1` 并设置 `errno`

- **作用**：类似 `wait3`，但提供更多控制功能，允许等待特定的子进程并获取资源使用信息



#### 1.3.5 waitid

- **原型**：

    ```c
    int waitid(idtype_t idtype, id_t id, siginfo_t *infop, int options);
    ```

- **参数**：

    - **`idtype`**：指定 `id` 的类型。可以是以下类型之一：

        -   `P_PID`：等待具有特定 `id` 的进程。
        -   `P_PGID`：等待具有特定进程组 ID 的进程
        -   `P_ALL`：等待任何子进程
        
    - **`id`**：指定的进程 ID 或进程组 ID（如果 `idtype` 是 `P_ALL`，则该值被忽略）
    
    -  **`infop`**：指向 `siginfo_t` 结构体的指针，用于存储有关终止进程的详细信息
    
        ​			`siginfo_t` 包含了子进程的终止原因、信号信息等，需要提供有效的指针
    
    - **`options`**：一个或多个选项，控制等待行为。可以使用以下选项

- **返回值**：成功时返回 `0`；失败时返回 `-1`，并设置 `errno`



`siginfo_t` 结构体：`infop` 参数指向的 `siginfo_t` 结构体，包含进程的状态信息，比如退出原因、退出信号等

`siginfo_t` 结构体的一些常见字段包括：

*   `si_pid`：子进程的 PID

-   `si_status`：进程的退出状态或收到的信号
-   `si_code`：描述退出状态的代码，指示退出原因是正常退出、信号终止还是其他

- **作用**：相比于其他等待函数，waitid函数可以等待特定进程，获取丰富的信息且拥有更强的控制等待能力，但问题是==我不会用==





#### 1.3.6 宏

1. `WIFEXITED(status)`

- **作用**：检查子进程是否正常退出
- **参数**：`status` 是 `wait` 或 `waitpid` 的返回状态值
- **返回值**：如果子进程正常退出，返回 `非零值`；否则返回 `0`



2. `WEXITSTATUS(status)`

- **作用**：获取子进程的退出状态（即 `exit()` 调用的参数）
- **参数**：`status` 是 `wait` 或 `waitpid` 的返回状态值
- **返回值**：子进程的退出状态，通常是一个整数值



3. `WIFSIGNALED(status)`

- **作用**：检查子进程是否因信号而终止
- **参数**：`status` 是 `wait` 或 `waitpid` 的返回状态值
- **返回值**：如果子进程因信号终止，返回 `非零值`；否则返回 `0`



4. `WTERMSIG(status)`

- **作用**：获取导致子进程终止的信号编号
- **参数**：`status` 是 `wait` 或 `waitpid` 的返回状态值
- **返回值**：导致子进程终止的信号编号



5. `WIFSTOPPED(status)`

- **作用**：检查子进程是否被停止（例如，由于接收到 `SIGSTOP`）
- **参数**：`status` 是 `wait` 或 `waitpid` 的返回状态值
- **返回值**：如果子进程被停止，返回 `非零值`；否则返回 `0`



**例：**

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <unistd.h>

int main()
{
    pid_t pid;
    int status;

    // 创建子进程
    pid = fork();

    if (pid == -1)		// 创建进程失败
    {
        perror("fork failed");
        exit(EXIT_FAILURE);
        //使用 exit() 函数返回
        // EXIT_FAILURE 1 表示没有成功的执行一个程序
        // EXIT_SUCCESS 0 表示成功地执行一个程序
    }

    if (pid == 0)
    {
        // 子进程
        printf("子进程：正在执行...\n");
        sleep(2);  // 模拟子进程任务
        printf("子进程：任务完成，退出中...\n");
        exit(5);   // 使用 exit(5) 模拟返回状态码 5
    }
    else
    {
        // 父进程
        printf("父进程：等待子进程结束...\n");

        // 使用 waitpid 等待子进程结束
        pid_t wpid = waitpid(pid, &status, 0);

        if (wpid == -1)		//返回失败
        {
            perror("waitpid failed");
            exit(EXIT_FAILURE);
        }

        if (WIFEXITED(status))			//检查子进程是否正常退出
            printf("父进程：子进程正常退出，退出状态：%d\n", WEXITSTATUS(status));
        
        else if (WIFSIGNALED(status))	//检查子进程是否被信号终止
            printf("父进程：子进程被信号终止，信号编号：%d\n", WTERMSIG(status));
        
        else if (WIFSTOPPED(status))	//检查子进程是否暂停
            printf("父进程：子进程暂停，暂停信号编号：%d\n", WSTOPSIG(status));
    }

    return 0;
}
```







## 2 文件操作

## 3 多线程编程

### 3.1 <pthread.h>

`<pthread.h>` 头文件用于POSIX线程（即POSIX Threads，简称pthread）库的支持，提供多线程编程的API

在多线程编程中，开发者可以使用该库中的函数来创建、管理和控制线程。



#### 3.1.0 变量类型

1.   `pthread_t`

**类型**：用于表示线程ID，通常是一个不透明类型，可以通过 `pthread_create` 函数生成线程时获得

**作用**：用于标识和管理线程，在创建线程时，线程ID会存储到该类型的变量中

​	之后可以通过该变量来操作线程，例如等待线程结束（`pthread_join`）等



2.   `pthread_mutex_t`

**类型**：用于表示互斥锁（mutex）

**作用**：互斥锁用于在线程之间同步对共享资源的访问

​		互斥锁可以确保同一时刻只有一个线程进入临界区，从而避免数据竞争



3.   `pthread_cond_t`

**类型**：用于表示条件变量

**作用**：条件变量允许线程在等待某个条件发生时进入阻塞状态，直到条件满足时被其他线程唤醒

​	与互斥锁一起使用，可以实现更灵活的线程同步机制





#### 3.1.1 pthread_create
**作用**：用于创建新线程

**原型**：

```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```

**参数**：

- `pthread_t *thread`：线程ID的指针，创建的线程ID会存储在此变量中。
- `const pthread_attr_t *attr`：线程属性指针，`NULL` 表示使用默认属性。
- `void *(*start_routine)(void *)`：线程执行的函数指针，这个函数必须接受一个 `void *` 类型的参数并返回一个 `void *` 类型的值。
- `void *arg`：传递给 `start_routine` 的参数。

**返回值**：成功时返回0，失败时返回错误码。



**例**：

```c
#include <pthread.h>
#include <stdio.h>

void* thread_func(void* arg)
//void* 表示一个通用的指针类型,和c++中的auto相似
{
    printf("线程正在运行\n");
    return NULL;
}

int main()
{
    pthread_t thread;
    int result = pthread_create(&thread, NULL, thread_func, NULL);
    if (result == 0)
    {
        printf("线程创建成功\n");
        pthread_join(thread, NULL);  // 等待线程结束
    }
    else
        printf("线程创建失败\n");
    return 0;
}
```



#### 3.1.2 pthread_join

**作用**：用于等待指定的线程终止，并回收其资源

**原型**：

```c
int pthread_join(pthread_t thread, void **retval);
```

**参数**：
- `pthread_t thread`：要等待的线程的ID
- `void **retval`：线程退出时返回的值的指针。如果不需要返回值，可以传 `NULL`

**返回值**：成功时返回0，失败时返回错误码



**例**：

```c
pthread_t thread;
pthread_create(&thread, NULL, thread_func, NULL);
pthread_join(thread, NULL);  // 主线程等待子线程完成
```



#### 3.1.3 pthread_exit

**作用**：用于终止当前线程，并返回一个退出状态

**原型**：

```c
void pthread_exit(void *retval);
```

**参数**：
- `void *retval`：线程的返回值，可以在 `pthread_join` 中获得。如果不需要返回值，可以传 `NULL`

**例**：

```c
void* thread_func(void* arg)
{
    pthread_exit(NULL);  // 线程终止
}
```



#### 3.1.4 pthread_mutex_init

**作用**：初始化互斥锁（mutex）

**原型**：

```c
int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);
```

**参数**：
- `pthread_mutex_t *mutex`：指向互斥锁的指针
- `const pthread_mutexattr_t *attr`：互斥锁的属性，通常传 `NULL`，表示使用默认属性

**返回值**：成功时返回0，失败时返回错误码

**例**：

```c
pthread_mutex_t mutex;
pthread_mutex_init(&mutex, NULL);
```



#### 3.1.5 pthread_mutex_lock

**作用**：加锁互斥锁，若锁已经被其他线程占用，则调用线程会阻塞，直到锁被释放

**原型**：

```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
```

**参数**：
- `pthread_mutex_t *mutex`：要加锁的互斥锁

**返回值**：成功时返回0，失败时返回错误码

**例**：

```c
pthread_mutex_lock(&mutex);  // 线程进入临界区
```



#### 3.1.6 pthread_mutex_unlock

**作用**：解锁互斥锁，允许其他线程获得锁

**原型**：

```c
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

**参数**：
- `pthread_mutex_t *mutex`：要解锁的互斥锁

**返回值**：成功时返回0，失败时返回错误码

**例**：

```c
pthread_mutex_unlock(&mutex);  // 线程退出临界区
```



#### 3.1.7 pthread_cond_wait

**作用**：使线程等待一个条件变量，同时释放互斥锁，直到条件满足被唤醒

**原型**：

```c
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
```

**参数**：
- `pthread_cond_t *cond`：条件变量
- `pthread_mutex_t *mutex`：互斥锁，等待时会自动释放，唤醒后会自动加锁

**返回值**：成功时返回0，失败时返回错误码

**例**：

```c
pthread_cond_wait(&cond, &mutex);
```



#### 3.1.8 pthread_cond_signal

**作用**：唤醒一个等待条件变量的线程

**函数原型**：

```c
int pthread_cond_signal(pthread_cond_t *cond);
```

**参数**：
- `pthread_cond_t *cond`：要发送信号的条件变量

**返回值**：成功时返回0，失败时返回错误码

**例**：

```c
pthread_cond_signal(&cond);  // 唤醒等待该条件变量的一个线程
```



**例**：

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

pthread_mutex_t mutex;             	// 互斥锁
pthread_cond_t cond;               	// 条件变量
int buffer = 0;                    	// 数据缓冲区
int buffer_full = 0;               	// 标志缓冲区是否有数据

// 消费者线程函数
void* consumer(void* arg)
{
    pthread_mutex_lock(&mutex);    	// 加锁
    while (buffer_full == 0)		// 如果缓冲区为空，等待条件变量
    {
        printf("消费者等待数据...\n");
        pthread_cond_wait(&cond, &mutex);   // 等待信号，自动释放锁
    }
    printf("消费者获取数据: %d\n", buffer);
    buffer_full = 0;               	// 标记缓冲区为空
    pthread_mutex_unlock(&mutex);  	// 解锁
    return NULL;
}

// 生产者线程函数
void* producer(void* arg)
{
    sleep(1);                      	// 模拟生产数据的延迟
    pthread_mutex_lock(&mutex);     // 加锁
    buffer = 42;                   	// 生产数据
    buffer_full = 1;               	// 标记缓冲区已满
    printf("生产者生产数据: %d\n", buffer);
    pthread_cond_signal(&cond);     // 唤醒等待条件的消费者
    pthread_mutex_unlock(&mutex);   // 解锁
    return NULL;
}

int main()
{
    pthread_t producer_thread, consumer_thread;

    // 初始化互斥锁和条件变量
    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    // 创建生产者和消费者线程
    pthread_create(&producer_thread, NULL, producer, NULL);
    pthread_create(&consumer_thread, NULL, consumer, NULL);

    // 等待两个线程完成
    pthread_join(producer_thread, NULL);
    pthread_join(consumer_thread, NULL);

    // 销毁互斥锁和条件变量
    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    return 0;
}

/*
	消费者等待数据...
	生产者生成数据: 100
	消费者消费数据: 100
*/
```











### 3.2 <semaphore.h>

`<semaphore.h>` 头文件主要用于处理**信号量**，信号量是一种用于线程间同步的机制

信号量可以控制同时访问共享资源的线程数量，常用于解决并发编程中的资源竞争问题






####  3.2.0 sem_t
- **类型**：信号量类型，定义了一个信号量变量

- **作用**：信号量对象，用于管理共享资源的访问

    ​		`sem_t` 类型变量通过相关函数（如 `sem_init`）进行初始化，并通过其他信号量函数进行操作




#### 3.2.1 sem_init
- **原型**：
  
  ```c
  int sem_init(sem_t *sem, int pshared, unsigned int value);
  ```
- **参数**：
  
  - `sem`：指向要初始化的信号量变量。
  - `pshared`：如果为0，则表示该信号量用于**线程**之间的同步；如果不为0，表示信号量可以在多个**进程**之间共享
  - `value`：信号量的初始值，通常为正整数，表示可以同时访问共享资源的最大线程数
- **返回值**：成功时返回 `0`，失败时返回 `-1`，并设置 `errno` 来指示错误原因
- **作用**：初始化信号量，使其可用于线程或进程间同步

**例**：

```c
sem_t semaphore;
sem_init(&semaphore, 0, 3);  // 初始化信号量，允许最多3个线程同时访问资源
```



#### 3.2.2 sem_wait

- **原型**：
  ```c
  int sem_wait(sem_t *sem);
  ```
  
- **参数**：
  - `sem`：指向信号量的指针。
  
- **返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`

- **作用**：如果信号量的值大于0，线程可以继续执行C ；如果信号量的值为0，调用线程会阻塞，直到信号量的值大于0（即有其他线程释放信号量）

    ​		判断结束后，信号量的值减1

**例**：

```c
sem_wait(&semaphore);  // 等待信号量，若信号量值为0，线程将阻塞
// 访问共享资源
```



#### 3.2.3 sem_post

- **原型**：
  
  ```c
  int sem_post(sem_t *sem);
  ```
  
- **参数**：
  - `sem`：指向信号量的指针。
  
- **返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`。

- **作用**：信号量的值加1，释放信号量，如果有等待的线程，此操作会唤醒其中一个等待线程

**例**：

```c
// 访问共享资源完成
sem_post(&semaphore);  // 释放信号量
```



#### 3.2.4 sem_destroy

- **原型**：
  
  ```c
  int sem_destroy(sem_t *sem);
  ```
- **参数**：
  - `sem`：指向要销毁的信号量变量。
- **返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`。
- **作用**：销毁信号量，释放与该信号量相关的所有资源，在程序结束或不再需要信号量时调用

**例**：

```c
sem_destroy(&semaphore);  // 销毁信号量
```



#### 3.2.5 sem_getvalue

- **原型**：
  ```c
  int sem_getvalue(sem_t *sem, int *sval);
  ```
- **参数**：
  - `sem`：指向信号量的指针。
  - `sval`：指向保存信号量当前值的整数指针。
- **返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`
- **作用**：获取信号量的当前值。如果信号量当前值大于0，表示资源可用线程数；如果为0，则表示资源已被占满，线程可能正在等待

**例**：

```c
int value;
sem_getvalue(&semaphore, &value);  // 获取信号量的当前值
printf("信号量的当前值为: %d\n", value);
```





**例**：

```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define BUFFER_SIZE 5

sem_t empty_slots;  	// 空缓冲区的信号量
sem_t full_slots;   	// 满缓冲区的信号量
pthread_mutex_t mutex;  // 互斥锁保护缓冲区
int buffer[BUFFER_SIZE];
int count = 0;  		// 计数缓冲区中的数据

// 生产者线程
void* producer(void* arg)
{
    int item;
    for (int i = 0; i < 10; i++)
    {
        item = i + 1;
        sem_wait(&empty_slots);  		// 等待空缓冲区
        pthread_mutex_lock(&mutex);  	// 加锁，保护缓冲区

        buffer[count++] = item;  		// 向缓冲区添加数据
        printf("生产者生产: %d\n", item);

        pthread_mutex_unlock(&mutex);  	// 解锁
        sem_post(&full_slots);  		// 通知有新的满缓冲区
        sleep(1);  						// 模拟生产耗时
    }
    return NULL;
}

// 消费者线程
void* consumer(void* arg)
{
    int item;
    for (int i = 0; i < 10; i++)
    {
        sem_wait(&full_slots);  		// 等待满缓冲区
        pthread_mutex_lock(&mutex); 	// 加锁，保护缓冲区

        item = buffer[--count];  		// 从缓冲区消费数据
        printf("消费者消费: %d\n", item);

        pthread_mutex_unlock(&mutex);  	// 解锁
        sem_post(&empty_slots); 	 	// 通知有新的空缓冲区
        sleep(2);  						// 模拟消费耗时
    }
    return NULL;
}

int main()
{
    pthread_t prod_thread, cons_thread;

    // 初始化信号量
    sem_init(&empty_slots, 0, BUFFER_SIZE);  // 缓冲区初始为空
    sem_init(&full_slots, 0, 0);             // 初始没有满的缓冲区

    // 初始化互斥锁
    pthread_mutex_init(&mutex, NULL);

    // 创建生产者和消费者线程
    pthread_create(&prod_thread, NULL, producer, NULL);
    pthread_create(&cons_thread, NULL, consumer, NULL);

    // 等待线程结束
    pthread_join(prod_thread, NULL);
    pthread_join(cons_thread, NULL);

    // 销毁信号量和互斥锁
    sem_destroy(&empty_slots);
    sem_destroy(&full_slots);
    pthread_mutex_destroy(&mutex);

    return 0;
}

/*
	生产者生产: 1
	消费者消费: 1
	生产者生产: 2
	消费者消费: 2
	生产者生产: 3
	生产者生产: 4
	消费者消费: 3
*/
```





## 4 进程通信

### 4.1 <signal.h>

`<signal.h>` 是一个用于处理信号的标准 C 语言头文件，主要用于进程之间的通信、异常情况的处理和控制流程的管理

它提供了一些与信号相关的函数、宏和数据类型

`<signal.h>` 是一个用于处理信号的标准 C 语言头文件，主要用于进程之间的通信、异常情况的处理和控制流程的管理。它提供了一些与信号相关的函数、宏和数据类型。以下是该头文件中一些重要的函数和宏的解释：

#### 4.1.1 signal

*   **原型**：

```c
void (*signal(int signum, void (*handler)(int)))(int);
```
*   **作用**：为特定信号 `signum` 设置处理程序 `handler`

    当进程接收到这个信号时，会调用指定的处理函数

*   **参数**：
    *   `signum`：信号编号（例如 `SIGINT`、`SIGTERM` 等），表示要捕获的信号
    *   `handler`：指向信号处理函数的指针。处理函数的参数是信号编号，返回类型为 `void`
    *   特殊的 `handler` 值包括：
        *   `SIG_DFL`：表示使用信号的默认处理程序
        *   `SIG_IGN`：表示忽略该信号
*   **返回值**：返回上一个信号处理函数的地址，或者在出错时返回 `SIG_ERR`



**例**：

```c
void my_handler(int signum) {
    printf("Signal %d caught!\n", signum);
}

int main() {
    signal(SIGINT, my_handler); // 捕获 Ctrl+C 信号
    while(1);
}
```



#### 4.1.2 raise

*	**原型**：

```c
int raise(int signum);
```
*   **作用**：向当前进程发送信号 `signum`，模拟产生某个信号
    
*   **参数**：`signum`：信号编号，表示要发送的信号

*   **返回值**：成功返回 `0`，失败返回非零值。

**例**：

```c
raise(SIGINT); // 向当前进程发送 SIGINT 信号
```



#### 4.1.3 kill

*   **原型**：

```c
int kill(pid_t pid, int sig);
```
*   **作用**：向指定进程或进程组发送信号 `sig`

*   **参数**：

    *   `pid`：

        *   大于 0 表示向特定进程发送信号

        *   等于 0 表示向调用进程所属的进程组发送信号

        *   小于 -1 表示向进程组发送信号，其进程组 ID 为 `-pid`
    *   `sig`：要发送的信号编号

*   **返回值**：成功返回 `0`，失败返回 `-1`，并设置 `errno`

**例**：

```c
kill(1234, SIGTERM); // 向 PID 为 1234 的进程发送 SIGTERM 信号
```



#### 4.1.4 sigaction

*   **原型**：

```c
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
```
*   **作用**：更为灵活地控制信号处理程序，代替 `signal` 函数。可以设置信号处理函数以及其他信号处理选项

*   **参数**：
    *   `signum`：要处理的信号编号
    *   `act`：新的信号处理方式
    *   `oldact`：如果不为 `NULL`，则保存旧的信号处理方式

`sigaction` 函数使用的结构体 `struct sigaction` 定义如下：

```c
struct sigaction
{
    void (*sa_handler)(int);       // 信号处理函数指针
    void (*sa_sigaction)(int, siginfo_t *, void *); // 信号处理函数，支持更详细信息
    sigset_t sa_mask;              // 在信号处理函数执行期间屏蔽的信号集
    int sa_flags;                  // 控制信号行为的标志位
    void (*sa_restorer)(void);     // 已弃用
};
```

*   **返回值**：成功返回 `0`，失败返回 `-1` 并设置 `errno`。

**例**：

```c
struct sigaction act;
act.sa_handler = my_handler;
sigemptyset(&act.sa_mask);
act.sa_flags = 0;
sigaction(SIGINT, &act, NULL); // 设置 SIGINT 信号的处理程序
```



#### 4.1.5 常用的信号

1.   `SIGINT`：程序终止（Ctrl+C）
2.   `SIGTERM`：程序终止请求
3.   `SIGKILL`：强制终止程序，无法捕获或忽略
4.   `SIGSTOP`：暂停程序执行，无法捕获或忽略
5.   `SIGCONT`：继续暂停的程序。

这些函数和信号为程序提供了丰富的信号处理能力，用于管理进程间通信、错误处理和异常控制







### 4.2 管道通信

通过 `<unistd.h>` 头文件中的函数创建连接进程的管道用于通信



#### 4.1.1 pipe

- **原型**：

    ```c
    int pipe(int pipefd[2]);
    ```

- **参数**：

    - `pipefd`：一个长度为 2 的整型数组，`pipefd[0]` 用于读取，`pipefd[1]` 用于写入

- **返回值**：成功返回 `0`，失败返回 `-1` 并设置 `errno`

- **作用**：让一个二维数组变成一个用于进程间通信的管道，管道提供了一种单向数据流，可以在父子进程或其他相关进程间传递数据

**例**：

```c
#include <unistd.h>
#include <stdio.h>

int main()
{
    int pipefd[2];
    char buffer[10];
    
    if (pipe(pipefd) == -1)
    {
        perror("pipe失败");
        return 1;
    }

    if (fork() == 0)
    {
        // 子进程：向管道写入数据
        close(pipefd[0]);  // 关闭读取端
        write(pipefd[1], "Hello", 5);
        close(pipefd[1]);  // 关闭写入端
    }
    else
    {
        // 父进程：从管道读取数据
        close(pipefd[1]);  // 关闭写入端
        read(pipefd[0], buffer, 5);
        printf("父进程从管道读取：%s\n", buffer);
        close(pipefd[0]);  // 关闭读取端
    }

    return 0;
}
```





#### 4.2.2 close

*   **原型**：

```c
int close(int fd);
```

*   **作用**：

    *   `close` 函数用于关闭文件描述符 `fd`，终止进程对该文件描述符的访问

    *   在多进程编程中，父进程和子进程之间通常会通过管道（`pipe`）、文件、套接字等方式进行通信

        使用完这些资源后，调用 `close` 可以释放文件描述符，防止资源耗尽，避免资源泄漏



*   **参数**：

    `fd`：待关闭的文件描述符（它可以是一个文件、管道、套接字等的描述符，表示需要关闭的资源）



*   **返回值**：

    *   成功时返回 `0`

    *   失败时返回 `-1`，并设置 `errno` 来指明具体的错误原因

        例如，可能的错误包括：

        *   `EBADF`：非法的文件描述符，即 `fd` 不是一个有效的文件描述符
        *   `EINTR`：`close` 调用被信号中断，需要重新尝试。



**例**：

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main()
{
    int pipefd[2];
    pid_t pid;

    // 创建管道
    if (pipe(pipefd) == -1)
    {
        perror("pipe");
        return 1;
    }

    pid = fork();
    if (pid == -1)
    {
        perror("fork");
        return 1;
    }

    if (pid == 0)
    {
        close(pipefd[1]); // 子进程关闭写端
        
        char buffer[128];
        read(pipefd[0], buffer, sizeof(buffer)); // 从管道读取数据
        printf("Child process received: %s\n", buffer);
        
        close(pipefd[0]); // 关闭读端
    }
    else
    {
        close(pipefd[0]); // 父进程关闭读端

        char msg[] = "Hello from parent!";
        write(pipefd[1], msg, sizeof(msg)); // 向管道写入数据

        close(pipefd[1]); // 关闭写端
    }

    return 0;
}
```



#### 4.2.3 write 和 read

- **原型**：

    ```c
    ssize_t write(int fd, const void *buf, size_t count);
    ssize_t read(int fd, void *buf, size_t count);
    ```

- **参数**：

    - `fd`：文件描述符，表示要写入或读取的文件或地址
    - `buf`：指向要写入或读取的数据的缓冲区
    - `count`：要写入或读取的字节数

- **返回值**：返回实际写入或读取的字节数，失败时返回 `-1` 并设置 `errno`

- **作用**：`write` 用于向文件或设备写入数据，`read` 用于从文件或设备读取数据

**例**：

```c
#include <unistd.h>
#include <stdio.h>
#include <fcntl.h>

int main()
{
    char buffer[20];
    int fd = open("test.txt", O_RDONLY);
    if (fd < 0)
    {
        perror("open失败");
        return 1;
    }

    ssize_t bytes_read =

	read(fd, buffer, sizeof(buffer));
    if (bytes_read >= 0)
    {
        buffer[bytes_read] = '\0';  // 确保字符串以 NULL 结尾
        printf("读取的数据：%s\n", buffer);
    }
    else
        perror("read失败");

    close(fd);
    return 0;
}
```







### 4.3 <sys/msg.h>

`<sys/msg.h>` 是一个用于处理 `System V` 消息队列的头文件，主要提供了与消息队列相关的函数、结构体和常量

消息队列是一种进程间通信的机制

#### 4.3.1 msgget
*   **原型**：

```c
int msgget(key_t key, int msgflg);
```

*   **参数**：
    *   `key`：消息队列的唯一标识符。通常通过 `ftok()` 函数生成
    *   `msgflg`：标志位，用于控制消息队列的创建和访问权限
        *   `IPC_CREAT`：如果消息队列不存在，则创建消息队列
        *   `IPC_EXCL`：与 `IPC_CREAT` 一起使用，如果队列已存在则返回错误
        *   `IPC_NOWAIT`：操作不能立即完成时不等待
        *   权限标志：如 `0666` 代表消息队列的权限
*   **返回值**：
    *   成功时返回消息队列的标识符 (正整数)
    *   失败时返回 `-1` 并设置 `errno`
*   **作用**：用于创建一个新的消息队列或获取一个现有的消息队列







#### 4.3.2 msgsnd
*   **原型**：

```c
int msgsnd(int msqid, const void *msgp, size_t msgsz, int msgflg);
```

*   **参数**：

  *   `msqid`：由 `msgget()` 返回的消息队列标识符

  *   `msgp`：指向消息结构体的指针，该结构体必须包含消息类型和数据

      			消息结构体通常为自定义的，至少包含 `long mtype`

      `msgsz`：消息正文的大小（不包括消息类型 `mtype`）

  *	`msgflg`：控制发送行为的标志

  - `IPC_NOWAIT`：若消息队列已满，则不等待，立即返回错误

*   **返回值**：
    *   成功时返回 `0`
    *   失败时返回 `-1` 并设置 `errno`

*   **作用**：用于将消息发送到指定的消息队列



### 4.4 <sys/ipc.h>

`<sys/ipc.h>` 是一个标准头文件，主要用于在 Unix 和 Linux 系统中进行进程间通信（IPC，Inter-Process Communication）

该头文件定义了一些与 IPC 机制相关的结构体、常量和函数原型

最常见的 IPC 机制包括消息队列、信号量、共享内存等

#### 4.4.1 ftok

*   **原型**：

```c
key_t ftok(const char *pathname, int proj_id);
```

*   **参数**：
    
    *   `pathname`：指定一个现有文件的路径名（该文件必须存在，并且调用进程对该文件有读取权限，==该文件地址也可以不存在，原因未知==）
    *   `proj_id`：一个项目标识符，通常是一个字符或整数。该值用来区分同一个文件的不同 IPC 键
    
*   **返回值**：
    *   成功时返回生成的 IPC 键（类型为 `key_t`）
    *   失败时返回 `(key_t)-1`，并设置 `errno` 以指示错误

*   **作用**：

    `ftok` 用于根据指定的文件路径和项目 ID 生成一个唯一的 IPC 键

    这个键通常用于消息队列、共享内存或信号量的创建

    `ftok` 的目的是让不同的进程可以通过同一个文件和项目 ID 来生成相同的 IPC 键，从而实现 IPC 的识别和共享



**例**：

```c
key_t key = ftok("/tmp/somefile", 'A');
if (key == (key_t)-1)
{
    perror("ftok");
    exit(1);
}
```



注意常见的错误处理：
- 文件不存在或不可访问
- `proj_id` 非法或不符合规定



#### 4.4.2 类型定义

`<sys/ipc.h>` 中还定义了一些与 IPC 机制相关的常量和类型。最重要的是：

1.   常量：（可以在 `msgget` 函数中充当参数）

- `IPC_CREAT`：用于创建一个新的 IPC 资源（如消息队列、信号量等），如果资源已经存在，则不会创建新的
- `IPC_EXCL`：与 `IPC_CREAT` 一起使用，如果 IPC 资源已经存在，则会返回错误
- `IPC_NOWAIT`：用于非阻塞操作，如果资源不可用，调用函数将不会等待，而是立即返回

2.   类型：

- `key_t`：这是 IPC 键的类型，`ftok` 函数返回的值就是 `key_t` 类型
- `IPC_PRIVATE`：这是一个特殊的 `key_t` 值，表示创建一个私有的 IPC 资源，只能由创建者进程使用



**发送者**：

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>
#include <stdlib.h>

// 自定义消息结构
struct my_msgbuf
{
    long mtype;
    char mtext[100];
};

int main()
{
    key_t key;
    int msgid;
    struct my_msgbuf buf;

    // 生成消息队列的唯一键值
    if ((key = ftok("msg_queue_file", 65)) == -1)
    {
        perror("ftok");
        exit(1);
    }

    // 创建消息队列
    if ((msgid = msgget(key, 0666 | IPC_CREAT)) == -1)
    {
        perror("msgget");
        exit(1);
    }

    // 设置消息类型和消息内容
    buf.mtype = 1;  // 消息类型为1
    strcpy(buf.mtext, "Hello, this is a test message!");

    // 发送消息
    if (msgsnd(msgid, &buf, sizeof(buf.mtext), 0) == -1)
    {
        perror("msgsnd");
        exit(1);
    }

    printf("Message sent: %s\n", buf.mtext);

    return 0;
}
```



**接收者**：

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <stdlib.h>

// 自定义消息结构
struct my_msgbuf
{
    long mtype;
    char mtext[100];
};

int main()
{
    key_t key;
    int msgid;
    struct my_msgbuf buf;

    // 生成消息队列的唯一键值
    if ((key = ftok("msg_queue_file", 65)) == -1)
    {
        perror("ftok");
        exit(1);
    }

    // 获取现有的消息队列
    if ((msgid = msgget(key, 0666)) == -1)
    {
        perror("msgget");
        exit(1);
    }

    // 接收类型为1的消息
    if (msgrcv(msgid, &buf, sizeof(buf.mtext), 1, 0) == -1)
    {
        perror("msgrcv");
        exit(1);
    }

    printf("Message received: %s\n", buf.mtext);

    // 删除消息队列
    if (msgctl(msgid, IPC_RMID, NULL) == -1)
    {
        perror("msgctl");
        exit(1);
    }

    return 0;
}
```





### 4.5 <sys/shm.h>

`<sys/shm.h>` 是一个用于共享内存机制的头文件，提供了共享内存（shared memory）相关的函数和数据结构

共享内存是进程间通信（IPC）的一种方式，可以让多个进程访问同一块内存区域



#### 4.5.1 shmget

*   **原型**：

   ```c
   int shmget(key_t key, size_t size, int shmflg);
   ```

*   **参数**：
    
    *   `key`：用于标识共享内存的键值
    
        			通常通过 `ftok()` 函数生成。如果想创建匿名共享内存，可以设置为 `IPC_PRIVATE`
    
    *   `size`：共享内存段的大小（字节为单位）
    
    *   `shmflg`：
        *   `IPC_CREAT`（如果共享内存不存在则创建）
        *   `IPC_EXCL`（与 `IPC_CREAT` 一起使用时，表示如果共享内存已经存在则返回错误），以及权限标志（如 `0666`，表示所有用户可读写）
    
*    **返回值**：成功时返回共享内存标识符（非负整数），失败时返回 `-1` 并设置 `errno`

*	**作用**：创建一个新的共享内存段，或者返回已经存在的共享内存段的标识符



#### 4.5.2 shmat

*   **原型**：

   ```c
   void *shmat(int shmid, const void *shmaddr, int shmflg);
   ```

*	**参数**：
     - `shmid`：共享内存段的标识符，由 `shmget()` 返回
     - `shmaddr`：指定要将共享内存段附加到的地址，通常设为 `NULL`，表示由系统选择地址
     - `shmflg`：标志位，可以是 `SHM_RDONLY`（只读）或 `0`（读写模式）

*	**返回值**：成功时返回指向共享内存段的指针，失败时返回 `(void *)-1` 并设置 `errno`
     
*	**作用**：将共享内存段映射到调用进程的地址空间，以便进程可以访问该内存



#### 4.5.3 shmdt

*   **原型**：

   ```c
   int shmdt(const void *shmaddr);
   ```

*	**参数**：`shmaddr`：指向先前由 `shmat()` 返回的共享内存地址
     
*	**返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`
     
*    **作用**：将共享内存段从进程的地址空间中移除，进程不再访问该共享内存



#### 4.5.4 shmctl

*   **原型**：

   ```c
   int shmctl(int shmid, int cmd, struct shmid_ds *buf);
   ```

*	**参数**：
   
   - `shmid`：共享内存段的标识符
   - `cmd`：控制命令，可以是：
     - `IPC_RMID`：删除共享内存段
     - `IPC_STAT`：获取共享内存段的 `shmid_ds` 结构信息
     - `IPC_SET`：设置共享内存段的 `shmid_ds` 结构
   - `buf`：指向 `shmid_ds` 结构的指针，用于存储或设置共享内存段的信息，可以传 `NULL`
   
*	**返回值**：成功时返回 `0`，失败时返回 `-1` 并设置 `errno`
     
*    **作用**：对共享内存段进行控制，例如删除共享内存或修改其权限等



**例**：

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <string.h>
#include <unistd.h>

int main()
{
    // 创建唯一的 key
    key_t key = ftok("shmfile", 65);

    // 创建共享内存段
    int shmid = shmget(key, 1024, 0666 | IPC_CREAT);

    // 将共享内存段附加到进程地址空间
    char *str = (char *)shmat(shmid, NULL, 0);

    if (fork() == 0)
    {
        // 子进程读取数据
        printf("Data read from memory：%s\n", str);

        // 分离共享内存段
        shmdt(str);
    }
    else
    {
        // 父进程写入数据
        strcpy(str, "Hello, Shared Memory!");

        // 等待子进程结束
        wait(NULL);

        // 分离共享内存段
        shmdt(str);

        // 删除共享内存段
        shmctl(shmid, IPC_RMID, NULL);
    }

    return 0;
}
```





## 5 进程调度

## 6 内存管理