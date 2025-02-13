- [多进程](#多进程)
- [进程间通信](#进程间通信)
- [进程间同步](#进程间同步)
- [多线程](#多线程)

## 多进程
```cpp
// 父进程返回子进程的PID（>0）
// 子进程返回0
// 失败返回-1
#include <unistd.h>
#include <iostream>

int main() {
    pid_t pid = fork();
    if (pid == -1) {
        std::cerr << "Fork failed\n";
        return 1;
    } else if (pid == 0) {
        std::cout << "Child process, PID: " << getpid() << std::endl;
    } else {
        std::cout << "Parent process, Child PID: " << pid << std::endl;
    }
    return 0;
}




// 使用exec执行新程序，替换当前程序
#include <unistd.h>

int main() {
    execl("/bin/ls", "ls", "-l", nullptr); // 替换为执行 ls -l
    return 0; // 仅在 exec 失败时执行
}
```
## 进程间通信
```cpp
// 进程间通信方式有：信号，套接字，管道，共享内存，文件

// 1. 匿名管道（Pipe）
#include <unistd.h>
#include <string.h>
#include <iostream>

int main() {
    int fd[2];
    if (pipe(fd) == -1) {
        std::cerr << "Pipe failed\n";
        return 1;
    }

    pid_t pid = fork();
    if (pid == 0) { // 子进程读数据
        close(fd[1]); // 关闭写端
        char buf[100];
        read(fd[0], buf, sizeof(buf));
        std::cout << "Child received: " << buf << std::endl;
        close(fd[0]);
    } else { // 父进程写数据
        close(fd[0]); // 关闭读端
        const char* msg = "Hello from parent!";
        write(fd[1], msg, strlen(msg) + 1);
        close(fd[1]);
    }
    return 0;
}




// 2. 共享内存（Shared Memory）
#include <sys/mman.h>
#include <sys/wait.h>
#include <unistd.h>
#include <iostream>

int main() {
    int size = 4096;
    void* shm = mmap(nullptr, size, PROT_READ | PROT_WRITE, 
                     MAP_SHARED | MAP_ANONYMOUS, -1, 0);
    if (shm == MAP_FAILED) {
        std::cerr << "mmap failed\n";
        return 1;
    }

    pid_t pid = fork();
    if (pid == 0) {
        // 子进程读取数据
        std::cout << "Child read: " << static_cast<char*>(shm) << std::endl;
        munmap(shm, size);
    } else {
        // 父进程写入数据
        sprintf(static_cast<char*>(shm), "Hello via shared memory!");
        wait(nullptr); // 等待子进程结束
        munmap(shm, size);
    }
    return 0;
}




// 3. 信号（Signal）
#include <signal.h>
#include <unistd.h>
#include <iostream>

void handler(int sig) {
    std::cout << "Received signal: " << sig << std::endl;
}

int main() {
    signal(SIGUSR1, handler); // 注册信号处理函数
    pid_t pid = fork();
    if (pid == 0) {
        // 子进程向父进程发送信号
        kill(getppid(), SIGUSR1);
    } else {
        pause(); // 等待信号
    }
    return 0;
}
```
## 进程间同步
```cpp
// 1. 信号量（Semaphore）
#include <fcntl.h>
#include <sys/stat.h>
#include <semaphore.h>
#include <iostream>
#include <unistd.h>

int main() {
    sem_t* sem = sem_open("/my_semaphore", O_CREAT, 0644, 1); // 初始值1
    if (sem == SEM_FAILED) {
        std::cerr << "Semaphore open failed\n";
        return 1;
    }

    pid_t pid = fork();
    if (pid == 0) {
        sem_wait(sem); // 获取信号量
        std::cout << "Child entered critical section\n";
        sleep(2);
        sem_post(sem); // 释放信号量
    } else {
        sem_wait(sem);
        std::cout << "Parent entered critical section\n";
        sleep(2);
        sem_post(sem);
        sem_unlink("/my_semaphore");
    }
    return 0;
}
```
## 多线程
C++11标准中引入了5个头文件来支持多线程编程，如下图所示：
![](images/cplusplus_201.png)
```
两种方式利用并发提高性能

1. 易并行（embarrassingly parallel）算法：
一个线程执行算法的一部分，另一个线程执行算法的另一个部分

2. 数据并行（data parallelism）：
每个线程在不同的数据部分上执行相同的操作
```
## 示例
```cpp
// 实现循环打印1，2
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
using namespace std;

mutex m;
condition_variable cv;
int count = 1;

void one() {
    for (int i = 0; i < 100; ++i) {
        unique_lock<mutex> lk(m);
        cv.wait(lk, []{ return count % 2 == 1; }); // 等待奇数
        cout << 1 << endl;
        ++count;
        lk.unlock();
        cv.notify_one();
    }
}

void two() {
    for (int i = 0; i < 100; ++i) {
        unique_lock<mutex> lk(m);
        cv.wait(lk, []{ return count % 2 == 0; }); // 等待偶数
        cout << 2 << endl;
        ++count;
        lk.unlock();
        cv.notify_one();
    }
}

int main() {
    thread t1(one);
    thread t2(two);
    t1.join();
    t2.join();
}

```

```cpp
// std::this_thread::get_id()获得当前线程的id
// std::thread::hardware_concurrency()返回真正可以并发运行的线程数，如果无法获得此信息，
// 该函数可能返回0
// t.detach()函数分离线程
// t.join()函数阻塞主线程，直到t线程完成
// t.joinable()返回true或false
#include <iostream>
#include <thread>

void hello() {
    std::cout << "Hello from thread!" << std::endl;
}

int main() {
    std::thread t(hello);  // 创建一个线程并启动
    t.join();  // 等待线程结束
}

// 传递参数
void hello(int i, string s) { }
std::thread t(hello, 3, "world");

// std::thread可以与任何可调用类型一起工作
class backgound_task {
pubic:
	void operator()() const {
		do_something();
	}
};
int main() {
	background_task f;
	std::thread my_thread(f);
}

// 移交所有权
std::thread t1(some_function);
std::thread t2 = std::move(t1);





// 互斥锁
// std::lock_guard类在构造时会自动锁定传入的互斥锁，在析构时自动解锁
// std::lock可以一次性锁定两个或多个互斥锁而无死锁风险，std::lock(mtx1, mtx2);
// std::adopt_lock参数表明互斥锁已经被锁住，不要试图在构造函数中对互斥锁上锁
// std::lock_guard<std::mutex> lock(mtx, std::adopt_lock);

// std::scoped_lock结合了std::lock可以锁住多个互斥锁和std::lock_guard自动锁定和解锁的特点，属于c++17新特性，std::scoped_lock guard(mtx1, mtx2);

// std::unique_lock提供了lock()、try_lock()和unlcok()成员函数。这些函数会更新std::unique_lock内部的标志位，可以调用owns_lock()成员函数来查询这个标志位
// 除非需要转移锁的所有权或者执行其他需要std::unique_lock的操作，否则，最好还是使用std::scoped_lock

// c++14提供std::shared_timed_mutex，c++17提供std::shared_mutex

// 线程休眠，std::this_thread::sleep_for(std::chrono::milliseconds(100));
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;

void print_hello(int id) {
    std::lock_guard<std::mutex> lock(mtx);  // 自动加锁，作用域结束时自动释放锁
    std::cout << "Hello from thread " << id << std::endl;
}

int main() {
    std::thread t1(print_hello, 1);
    std::thread t2(print_hello, 2);
    
    t1.join();
    t2.join();
    
    return 0;
}




// 条件变量
// std::condition_variable c;
// c.wait(lock)，c.notify_one()，c.notify_all()
// std::condition_variable仅限于与std::mutex一起使用
// std::condition_variable_any更加通用，可以与任何满足互斥锁基本条件的对象一起使用
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void print_id(int id) {
    std::unique_lock<std::mutex> lock(mtx);
    while (!ready) {
        cv.wait(lock);  // 等待条件满足
    }
    std::cout << "Thread " << id << '\n';
}

void go() {
    std::unique_lock<std::mutex> lock(mtx);
    ready = true;
    cv.notify_all();  // 通知所有等待的线程
}

int main() {
    std::thread threads[10];
    for (int i = 0; i < 10; ++i) {
        threads[i] = std::thread(print_id, i);
    }
    
    std::cout << "10 threads ready to race...\n";
    go();  // 激活线程
    
    for (auto& t : threads) {
        t.join();
    }
    
    return 0;
}




// 原子操作
#include <iostream>
#include <thread>
#include <atomic>

std::atomic<int> counter(0);

void increase() {
    for (int i = 0; i < 1000; ++i) {
        counter.fetch_add(1, std::memory_order_relaxed);  // 原子递增
    }
}

int main() {
    std::thread t1(increase);
    std::thread t2(increase);
    
    t1.join();
    t2.join();
    
    std::cout << "Counter: " << counter.load() << std::endl;  // 获取原子变量的值
    return 0;
}




// 通过std::async和std::future实现异步执行
// c++提供std::future和std::shared_future
// std::promise
#include <iostream>
#include <future>

int square(int x) {
    return x * x;
}

int main() {
    std::future<int> result = std::async(std::launch::async, square, 5);
    std::cout << "Square of 5 is: " << result.get() << std::endl;  // 获取返回结果
    return 0;
}
```