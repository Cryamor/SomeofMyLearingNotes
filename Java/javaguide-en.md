## Computer Network

## OS

OS is the program to manage computer hardware and software resources.

### Kernel Mode & User Mode

**User Mode**: access user program data, low permission, system call interface

**Kernel Mode**: access almost any resources, very high authority, underlying, more cost

**Reason** for dividing: 

- Security & Stability: dangerous operations(clock, memory allocation, IO) can only be used by Kernel

- Efficiency & Performance: if only 1 Kernel, all programs share system resources, leading to collision & competition

**Switching**:

- Trap: system call

- Interrupt: outside from CPU

- Exception

### Process & Thread

Thread is lightweight Process, several threads share process resources and work simultaneously.

for JVM, Threads share the process's **Heap** and **String Constant Pool**

each thread has its own **VM Stack**, **Native Method Stack** and **Program Counter Register**.

**Why**：low switching cost, concurrency

#### Thread Synchronize

1. **Mutex**: e.g. `synchronized` in Java

2. ****Read-Write Lock****: several read at the same time, but only one writes

3. **Semaphore**: control maximum of threads accessing resource at the same time

4. **Barrier**: wait until all threads reach one point, e.g. `CyclicBarrier` in Java

5. **Event**: Wait & Notify

#### Process State

![](assets/2024-04-16-19-30-08-image.png)

In Unix/Linux, a child process is created via `fork()`,  when child call `exit()`, resources released, but PCB still exists

**Zombie Process**: child terminated, but parent still running, and parent process has not called `wait()` or `waitpid()` to obtain the status of child, releasing the resources occupied by child.

```bash
ps -A -ostat,ppid,pid,cmd |grep -e '^[Zz]'
```

**Orphaned Process**: parent terminated, but child still running. To avoid occupying resources, OS sets orphan's parent as *init*(pid = 1) to release resources.

#### Process Communication

1. **Pipes**: between parent-child or sibling processes

2. **Named Pipes**: FIFO, any 2 processes

3. **Signal**

4. **Message Queuing**: FIFO, store in kernel, overcome: small information amount, only unformatted bytes and buffer size limitation.

5. **Semaphores**

6. **Shared memory**

7. **Sockets**: between client and server

#### Process Schedule Algorithm

1. **FCFS，First Come, First Served**

2. **SJF，Shortest Job First**

3. **RR，Round-Robin**

4. **MFQ，Multi-level Feedback Queue**

5. **Priority**

#### Deadlock

Necessary Condition:

1. Mutex

2. Possession and waiting

3. Non-preempt

4. Circular waiting

Solve Deadlock:

1. Prevention: Restrict requests
   
   1. **Static allocation strategy**(Condition 2): get all needed resources before exec, very low efficiency
   
   2. **Hierarchical allocation strategy**(Condition 4): can only apply for higher-layer resources 

2. Avoidance: Advance prediction, Banker Algorithm

3. Detection

4. Lift: free from a deadlock

### Memory

#### Page Switching Algorithm

1. FIFO

2. OPT, Optimal

3. LRU, Least Recently Used

4. LFU, Least Frequently Used

5. Clock

### File & Disk

#### Disk Schedule Algorithm

1. **First-Come First-Served，FCFS**

2. **Shortest Seek Time First，SSTF** (Shortest Service Time First，SSTF)

3. **SCAN**

4. **Circular Scan，C-SCAN**

5. **LOOK**: when no request on moving direction, change 

6. **C-LOOK**

## Java

## Database

### SQL

#### Data Type

**VARCHAR**(100) costs more memory than VARCHAR(10), for always alloc fixed memory (as defined)

**TEXT & BLOB** are not recommended, for:

1. no default value

2. low searching efficiency

3. temporary table can only be created on disk, not in memory

4. cannot create an index directly, need to specify the prefix length

**DATETIME & TIMESTAMP**：

- DATETIME：
  - 8(5~8) Bytes 
  - not time zone relevant, fast
  - 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59.499999
- Timestamp：
  - 4(4~7) Bytes 
  - time zone relevant, slow
  - 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07.499999
- String: no APIs, more cost, don't use

**NULL & ''**

1. `SELECT NULL=NULL` returns false, but `DISTINCT/GROUP BY/ORDER` BY considers equal

2. `''` no space, NULL needs space

3. use `IS (NOT) NULL`, not comparison operators

4. `COUNT *` counts NULL, `COUNT (columnname)` doesn't count NULL

#### DB Engine

plugin architecture, table-based, default InnoDB (MyISAM before 5.5)

## Distributed

## 
