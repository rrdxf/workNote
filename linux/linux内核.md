### 进程管理

![1640741719056](C:\Users\xf\Desktop\学习笔记\assets\1640741719056.png)、

LDT：数据段，代码端

TSS：在进程运行的过程中，cpu需要知道进程的状态标识 

task_struct 

1. state 程序运行的状态
2. counter=counter/2+priority  时间片
3. priority 优先级

#### 分时技术进行多进程调度

内核态：不可抢占

用户态：可抢占

##### 进程的创建

1. l进程初始化在初始化的过程中，会进行0号进程的创建，0号进程是所有进程的父进程

   在0号进程中：

   1. 打开标准输入输出错误的控制台句柄
   2. 创建1号进程，出国创建成功则在一号进程，首先打开/etc/rc 执行SHELL程序 /bin/sh
   3. 0号进程不可能结束，他会在没有其他调用的时候，会执行for(;;)pasuse()；停止

2. 进程创建：fork(); 就是对0号进程或者当前进程的复制  结构体的复制，把task[0]对应的task_struct复制给新的task_struck

   对于栈堆 

   1. 在task链表中，找一个进程空位存放当前的进程
   2. 创建一个task_struct
   3. 设置task_struct

   fork 进程的创建是系统调用：

   ![1640742819057](C:\Users\xf\Desktop\学习笔记\assets\1640742819057.png)

##### 进程的调度

​	void schedule(void)进程调度函数

![1641045284484](C:\Users\xf\Desktop\学习笔记\linux\assets\1641045284484.png)

​	switch_to(next) 进程切换

​		将需要切换的进程赋值给当前指针

​		进行进程的上下文切换

​		上下文：程序运行是cpu的特殊寄存器，通用寄存器

void sleep_on（struct task_struct **p）

```c
 void sleep_on(struct task_struct **p)
 {
     struct task_struct *tmp;
     if (!p)   //若指针无效，则退出
         return;
     if (current == &(init_task.task)) //若当前任务是任务0则死机
         panic("task[0] trying to sleep");
     tmp = *p; //让tmp指向已经在等待队列上的任务
     *p = current; //将睡眠队列头的等待指针指向当前任务
     current->state = TASK_UNINTERRUPTIBLE;  //将当前任务置为不可中断的等待状态
     schedule();   //重新调度
// 只有当这个等待任务被唤醒时，调度程序才又返回到这里，则表示进程已被明确地唤醒。
     if (tmp)  
         tmp->state=0;  // 若在其前还存在等待的任务，则也将其置为就绪状态（唤醒）。

 }

```



1. 如果是0号进程，就返回
2. 

##### 进程状态

​	运行状态 #define TASK_RUNNING  可以被运行 就绪态 进程切换只有在运行状态

​	可中断状态 #define TASK_INITERRUPTIBLE 可以被信号中断 使其变成RUNNING

​	不可中断状态 #define TASK_UNINTERRUPTIBLR 只能被wakeupo唤醒，变为RUNNING

​	暂停状态	#define TASK_ZOMBIE 收到sigstop sigtstp sigttin 暂停

​	僵尸状态 #define TASK_STOPPED 进程停止运行了，但是父进程还没有将其清空 waitpid





