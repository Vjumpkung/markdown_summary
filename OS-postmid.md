## บทที่ 6 Synchronization Tools

### Race Condition

- เริ่มจากสร้าง child process ด้วย `fork()` แล้วพบว่าสร้าง pid เดียวกัน

### Critical Section Problem

- ป้องกันการเขียน อ่าน การทำงานที่มากกว่า 1 process 

![](https://media.discordapp.net/attachments/1131128255735410688/1139020987837464646/image0.jpg?width=671&height=671)

1. Mutual Exclusion - process ทำงานอยู่ process อื่นไม่สามารถเข้าถึงได้
2. progress - กระบวนการทำงานใน critical section ต้องทำงานให้จบโดยเร็ว ต้องมีวิธีเลือกให้ทำงานเสร็จโดยเร็วที่สุด
3. Bounded Waiting 

### Interrupt-based Solution
1. Entry Section : Disable Interrupts
2. Exit Section : Enablw Interrupts

  - ปัญหา ถ้า critical section run 1 hr
  - process ใน critical section รันไม่จบ
  - ถ้ามี 2 cpu ?

### Software Solution 1 

- atomic = cannot interrupt ex. load/store
- ใช้ตัวแปร turn (`int turn`)
  
![](https://media.discordapp.net/attachments/1131128255735410688/1139021463702225027/image0.jpg?width=774&height=671)

Correctness of the software solution

- progress requirement ?
- bounded-waiting requirement ?

### Peterson's Solution

- 2 process solution
- เพิ่มตัวแปร 
  - `int turn`
  - `booolean flag[2]`

- ตัวแปร flag ตรวจสอบว่าอยากเข้่ามั้ย

![](https://media.discordapp.net/attachments/1131128255735410688/1139022794080919572/image0.jpg?width=849&height=671)

correctness

- confirm ได้ว่าเป็น process ที่จะเข้าไปใน critical section แน่นอน

ปัญหา

- solution นี้ครอบคลุมแค่ 2 process ทำให้มีประโยชน์ที่จำกัด
- multithread ทำให้เกิด reorder process ได้

### Modern Arch. Example
![](https://media.discordapp.net/attachments/1131128255735410688/1139023968469254204/image0.jpg?width=617&height=671)

problem
- โดน reorder โดย cpu หรือ compiler

Peterson's solution มีปัญหาถ้าเกิดการ reorder -> ใช้ Memory Barrier

- Strongly ordered แก้ memory ทุกๆ process เห็นทั้งหมด
- Weakly ordered แก้ memory ทุกๆ process ไม่เห็นทันที
- ถ้าเจอคำสั่ง memory barrier OS จะต้อง load/store ให้หมดก่อน แล้วถึงจะทำคำสั่งต่อได้

example

![](https://media.discordapp.net/attachments/1131128255735410688/1139025741770657812/image0.jpg?width=757&height=544)

- thread 1 การันตีว่า `flag` โหลดทุกอย่างก่อนเข้า `x` 
- thread 2 ทำ x ก่อนถึงเขียน flag

---> นำไปสู่ระบบที่ stable ขึ้น

### Synchronization Hardware

- uniprocessor แค่หยุด interrupts 

### HW Inst.

#### test and set inst.

![](https://media.discordapp.net/attachments/1131128255735410688/1139026391753555998/image0.jpg?width=1048&height=671)

![](https://media.discordapp.net/attachments/1131128255735410688/1139026763805114409/image0.jpg?width=880&height=671)

#### compare and swap inst.

![](https://media.discordapp.net/attachments/1131128255735410688/1139027313909043310/image0.jpg?width=1083&height=671)

![](https://media.discordapp.net/attachments/1131128255735410688/1139027564619387060/image0.jpg?width=960&height=671)

### Atomic Variable

- ตัวแปรที่ไม่สามารถ interrupt ได้

![](https://media.discordapp.net/attachments/1131128255735410688/1139029210359419012/image0.jpg?width=1439&height=536)

### Mutex Locks

- เครื่องมือที่ช่วยจัดการ test and set inst. and compare and swap inst.
- โดยทำผ่าน boolean variable
- ทำงานผ่าน `accquire()` `release()`

![](https://media.discordapp.net/attachments/1131128255735410688/1139030036637290656/image0.jpg?width=873&height=642)

### Semaphore

- เครื่องมือที่มีความละเอียดสูงกว่า Mutex Locks little bit
- เป็น S integer variable
- `wait() signal()` เป็น atomic
- ![](https://cdn.discordapp.com/attachments/1131128255735410688/1139030572543512617/image0.jpg)
- counting semaphore - int domain varyu
- binary semaphore - 0 or 1

Example

![](https://media.discordapp.net/attachments/1131128255735410688/1139031110756618290/image0.jpg?width=851&height=671)

การ implementation

- ใช้ busy waiting
- ใส่ queue ลงไปใน semaphore

waiting queue

```c
typedef struct {
  int value;
  struct process *list;
} semaphore;

wait(semaphore *s){
  S->value--;
  if (S->value < 0){
    add this process to S->list;
    block();
  }
}

signal(semaphore *s){
  S->value++;
  if (S->value <= 0){
    remove a process P from S->list;
    wakeup(P)
  }
}
```

problems

![](https://media.discordapp.net/attachments/1131128255735410688/1139033376196677643/image0.jpg?width=1161&height=671)


### Monitors

- ![](https://media.discordapp.net/attachments/1131128255735410688/1139034196799668284/image0.jpg?width=689&height=671) 

### Condition Variable

- ![](https://media.discordapp.net/attachments/1131128255735410688/1139034950641922078/image0.jpg?width=798&height=671)

- มี P1 P2 มี S1 S2 ตามลำดับ โดย S1 มาก่อน S2
  - สร้าง monitor ที่มี 2  procedure S1 S2
  - สร้างตัวแปร x -> 0
  - boolean done
  - ![](https://media.discordapp.net/attachments/1131128255735410688/1139035364384845844/image0.jpg?width=590&height=671)

### Monitor Implementation Using Semaphores

![](https://media.discordapp.net/attachments/1131128255735410688/1139036007937867897/image0.jpg?width=973&height=671)

### Condition Variable

![](https://media.discordapp.net/attachments/1131128255735410688/1139036559740522496/image0.jpg?width=1056&height=671)

### Resuming Processes within a Monitor

- การเรียก process กลับคืนมา 
- FCFS ไม่ adequate

### Signal Resource Allocation

- ใช้ priority กับ specifies
- ![](https://media.discordapp.net/attachments/1131128255735410688/1139038175709384784/image0.jpg?width=480&height=671)

usage : 
  ```
    accquire
      ...
    release
  ```

incorrect use :
  ```
    acc-acc
    re-re
    omitting accquire() and/or release()
  ```

### Liveness

- process อาจรอจนแห้ง 
- รอแล้วค่อยไปต่อ (โดนลัดคิวไปตลอดไม่ได้)
- ต้องมีการตัดสินใจว่าใครจะไปต่อ
- Liveness เป็นกลุ่มของ propeties ที่ ensure ว่าจะทำให้ไปต่อได้

###

- Deadlock -> >= 2 process รอ infinite

![](https://media.discordapp.net/attachments/1131128255735410688/1139039228907507723/image0.jpg?width=1409&height=546)

- รอกันไปรอกันมา แล้วก็ไปต่อไม่ได้เพราว่า P0 รอ P1 

- Deadlock รูปแบบอื่น 
  - Starvation - indefinite blocking
  - Priority Inversion - low-priority โดน high-priority lock
  
## บทที่ 7 Synchronization Examples

### Classical Problems 

- Bounded-Buffer
  - n buffer
  - semaphore mutex = 1 , full = 0 , empty = n
  - producer ![](https://media.discordapp.net/attachments/1131128255735410688/1139050818750926849/image0.jpg?width=939&height=671)
  - consumer ![](https://media.discordapp.net/attachments/1131128255735410688/1139050903949807676/image0.jpg?width=1153&height=671)
- Readers and Writers
  - dataset shared หลายๆ processes
    - readers - read dataset
    - writers - read and write
  - problem
    - allow multiple writers
      - only one single wrtiter can access
    - shared data
      - `data set`
      - `semaphore rw_mutex = 1`
      - `semaphore mutex = 1`
      - `read_count = 0`
    - writers
      - ![](https://media.discordapp.net/attachments/1131128255735410688/1139052231069536426/image0.jpg?width=1100&height=606)
    - readers
      - ![](https://media.discordapp.net/attachments/1131128255735410688/1139052463090061322/IMG_2422.png?width=883&height=671)
    - ปัญหา
      - writers process never writes 
      - second reader-writer problem
- Dining-Philosophers
  - N philosophers นั่งโต๊ะจีนโดยมีข้าวอยู่ตรงกลาง โดยไม่สุงสิงกับใคร
  - แต่มีตะเกียบแค่ข้างเดียว โดยการใช้คือ หยิบ 2 ข้างเพื่อกินข้าว
  - ![](https://media.discordapp.net/attachments/1131128255735410688/1139053546961129472/image0.jpg?width=387&height=375)
  - implement เป็น
    - `bowl of rice (data set)`
    - `semaphore chopstick[5] = {1,1,1,1,1}`
  - ![](https://media.discordapp.net/attachments/1131128255735410688/1139054209648570378/image0.jpg?width=772&height=671)
  - ปัญหาคือ หยิบพร้อมกัน
  - ![](https://media.discordapp.net/attachments/1131128255735410688/1139054884293967993/image0.jpg?width=477&height=671)
  - Solution
    - ![](https://media.discordapp.net/attachments/1131128255735410688/1139055230777045072/IMG_2429.png?width=1252&height=671)
  - ไม่มี deadlock แต่ยังมี starvation ยังมีอยู่
    - `block | x | block`

### Kernel Synchronization Windows
- interrupt mask to protect access to global resource
- spinlock on multiprocessor
- Mutex Dispatcher Object
  - ![](https://media.discordapp.net/attachments/1131128255735410688/1139056109378883664/image0.jpg?width=1439&height=597)

### Linux Synchronization
- ก่อน kernel 2.6
  - ปิด interrupt
- หลัง 2.6 
  -  fully preemptive
- Linux
  - semaphores
  - atomic int
  - spinlock
  - reader-writer version of both
- single CPU แทนที่ spinlock ด้วย disable kernel preemption
- มี `atomic_t` = atomic integer
- ![](https://media.discordapp.net/attachments/1131128255735410688/1139056660376195122/image0.jpg?width=1439&height=350)

### POSIX synchronization
- POSIX api
  - mutex locks
    - `lock & unlock`
  - semaphorers
    - nameed and unmameed
    - `open wait closed` 
  - condition variable
    - `wait and signal`

### Java Synchronization
- Java rich set
  - Java monitors
    - single lock
    - ถ้า other threads ต้องการ ต้องปลด lock ก่อน
  - Reentant locks
    - คล้ายๆ mutex lock
    - `finally`
  - Semaphores
    - เป็น semaphores pipe เลย (Object)
  - Condition Variables
    - associated with an `ReentrantLock`
    - create condition variable using `newCndition()`
    - thread waits by calling the `await()`
    - signgal calling the `signal()` method

### Alternatice Approaches
- Transactional Memory 
  - ครอบคำว่า `atomic`
- OpenMP
  - `#pragma omp critical` เป็นการบอกว่า read เป็น critical section แล้วทำเป็น atomic ป้องกัน thread อื่นเข้ามาทำงานพร้อมกัน
- Functional Programming Languages
  - variable เป็น immutable
  - example golang, scala
  - ไม่มี stage
