# สรุป OS Quiz 2

### Operations ใน processes
- creation
  - parent จะ create children process โดยจะทำต่อๆ ลงมาเป็นต้นไม้
  - process จะถูกจัดการด้วย pid (process identifier)
  - วิธีการแบ่ง resource มีให้เลือกดังนี้
    - parent กับ children shared ทั้งหมด
    - children shared เป็น subset (บางส่วน)
    - แยกจากกันเลย
  - รูปแบบการ execute
    - แบบพร้อมกัน
    - แบบรอ child terminate ก่อน
  - Address Space
    - child duplicate จาก parent
    - child มีโปรแกรมโหลดเข้าไป
  - UNIX example
    - fork() สร้าง process
    - exec() ใช้ต่อจาก fork()
    - wait() ใช้รอ children process terminate

![](https://media.discordapp.net/attachments/1131128255735410688/1138719563094171668/image0.jpg?width=899&height=671)

- termination
  - บาง OS ไม่ allow ให้ child ทำงานต่อเมื่อ parent ถูก terminate โดย children ต้องจบการทำงานด้วย
    - cascading termination -> All Terminated
    - Terminated จัดการโดย OS
  - parent process อาจรอการ termination ของ child process โดยใช้คำสั่ง `wait()` โดยจะ return status information และ pid ของ terminated process
    - `pid = wait(&status)`
  - ถ้าไม่มี parent รออยู่ (ไม่ได้เรียก `wait()`) จะเรียกว่า **zombie** process
  - ถ้่า parent terminated โดยไม่เรียก `wait()` จะเรียกว่า **orphan** process

### Android Process Importance Hierarchy
- ลำดับความสำคัญของ preocess ใน Android
  - Foreground process
  - Visible process
  - Service process
  - Background process
  - Empty process
- โดย Android จะ terminated จากความสำคัญน้อยไปหามาก

### Google Chrome
- แบ่ง processes ออกเป็น 3 ส่วน
  - Browser
  - Renderer
    - รันใน sandbox เพื่อป้องกันช่องโหว่
  - Plug-in

### Interprocess Communication
- เหตุผล
  - การแชร์ข้อมูล
  - speedup เรื่องการสื่อสาร
  - ความยืดหยุ่น (modularity)
  - convenience
- แบ่งออกเป็น 2 รูปแบบดังนี้ 
  - shared memory
  - message passing

![](https://media.discordapp.net/attachments/1131128255735410688/1138724140229070919/IMG_2368.png?width=905&height=671)

### Producer-Consumer Problem

- producer ให้ข้อมูลกับ consumer 
- มี 2 variation
  - unbounded-buffer - producer ไม่มีการรอ consumer จะรอเมื่อ buffer หมด
  - bounded-buffer - producer จะรอเมื่อ buffer เต็ม และ consumer จะรอเมื่อ buffer หมด

### IPC-Shared Memory
- หมายถึงการแขร์ memory เพื่อใช้ในการ communicate ซึ่งกันและกัน 
- communication จะจัดการโดย user โดยจะ allow ให้เกิดการ synchronize กัน

### การ Filling all the buffers

- กำหนดให้ consumer-producer problem เติมทุกๆ buffers
- สามารถทำได้โดยกำหนด counter = 0 ขึ้นมา โดยมีเงื่อไขดังนี้
  - counter เพิ่มขึ้นเมื่อสร้าง buffer
  - counter ลดลงเมื่อกิน buffer

### Race Condition
- จะกล่าวถึงในบทที่ 6

### IPC - Message Passing
- เป็นการสื่อสารกันโดยไม่ได้แชร์ตัวแปรกัน
- มี 2 operation
  - `send(msg)`
  - `recieve(msg)`
  - `msg` เป็น fixed หรือ variable ก็ได้
- ถ้า P และ Q ต้องการสื่อสารกันทำได้ดังนี้
  - สร้าง communation link ระหว่างกัน

### รูปแบบการ implement Communication Link
-  Physical
   -  Shared Memory
   -  Hardware Bus Network
- Logical
  - Direct or Indirect
  - Sync or Async
  - Automatic or Explict 

### Direct Communation
- process ต้องระบุชื่อในการส่ง

### Undirect Communication
- process ไม่ต้องระบุชื่อ แต่จะระบุเป็น ports โดยใช้ uid (unique id)
- จะสามารถคุยกันได้เมื่อ share mailbox

### Synchronization
- Blocking
  - blocking send
  - blocking receive
- Non-Blocking
  - non-blocking send
  - non-blocking recieve 
- ***ถ้่า send กับ recieve block พร้อมกันจะเรียกว่า rendezvous

### Buffering
- zero capacity - ไม่มีข้อความบน link 
- bounded capacity - ข้อความจำกัด n ข้อความ
- unbounded capacity - ไม่จำกัดข้อความ

### Pipes
- เป็นทางเชื่อมระหว่าง 2 processes สำหรับการสื่อสารกัน
  - ordinary pipes - มีการใช้ parent-child ในการสื่อสารกัน
  - named pipes - ไม่มี parent-child relationship

### Ordinary Pipes
- allow producer-consumer style communication
- unidirectional
- producer writes to one end
- comsumer reads from the other end
- ใช้ parent-child
- windows เรียกว่า anonymous pipes
  
### Named Pipes
- แข็งแกร่งกว่า ordinary pipes
- birectional communication
- หลาย processes ใช้ named pipe สำหรับการสื่อสารกัน
- UNIX and Windows มีทั้งคู่

### Client-Server Communication
- socket
- remote procedure calls

### Socket
- socket เป็น endpoint สำหรับการ communication
- ประกอบด้วย ip:port 
- `127.0.0.1` เป็น special ip address 

### Remote Procedure Calls
- process สื่อสารกับ server โดยตรง
  - ใช้ระบบ ports ในการแยกกันและกัน
  - client-side marshalles the parameters (ควบคุม parameters)
  - server-side รับ msg, unpacked, marshalls แล้วก็ performs procedure 
- Data จะถูกเสนอในรูปแบบ External Data Representation (XDL) 
  - Big-Endian , Little-Endian
- OS ให้ rendezvous หรือ matchmaker service สำหรับการเชื่อมต่อ client กับ server

![](https://4.bp.blogspot.com/_IEmaCFe3y9g/SO3GGEF4UkI/AAAAAAAAAAc/z7waF2Lwg0s/s400/lb.GIF)

ภาพ big endian กับ little endian

## บทที่ 4 Threads & Concurrency

ภาพเปรียบเทียบระหว่าง single-thread กับ multi-thread process

![](https://media.discordapp.net/attachments/1131128255735410688/1138740265520222269/image0.jpg?width=1002&height=671)

ภาพ server multithread architecture

![](https://media.discordapp.net/attachments/1131128255735410688/1138740709415997461/image0.jpg?width=1439&height=545)

### Multicore Programming

ประกอบไปด้วยวิธีการดังนี้
- แบ่ง activities
- Balance
- Data Splitting
- Data Dependency
- Testing and Debugging

**Parallelism** หมายถึง system ที่สามารถ perform ได้มากกว่า 1 task อย่างต่อเนื่อง

**concurrency** เป็นการรองรับมากกว่า 1 task สำหรับการสรัาง preocess

Parallelism vs Concurrency

![](https://media.discordapp.net/attachments/1131128255735410688/1138741764769972224/image0.jpg?width=1211&height=671)

รูปแบบของการ parallel
- Data Parallelism
- Task Parallelism

![](https://media.discordapp.net/attachments/1131128255735410688/1138742252110352384/image0.jpg?width=997&height=671)

### Amdahl's Law

![](https://media.discordapp.net/attachments/1131128255735410688/1138742608529739776/image0.jpg?width=1439&height=349)

- เป็นการตรวจสอบว่าการเพิ่มจำนวน cores สำหรับ application ที่มีทั้ง serial และ parallel จะมี speedup เท่าใด

### User Threads and Kernel Threads

- User Threads
  - เป็นการจัดการที่อยู่ในระดับ user
  - Examples
    - POSIX
    - Windows 
    - Java
- Kernel Threads
  - kernel support
  - Examples
    - Windows
    - Linux
    - macOS
    - iOS
    - Android

### Multithreading Models
- Many to 1
  - หลายๆ user threads map ไปยัง single kernel thread
  - ถ้า thread นึง blocking จะเกิดทุก block ทั้งหมด
  - multiple threads ไม่ได้รันในรูปแบบ parallel
  
  ![](https://media.discordapp.net/attachments/1131128255735410688/1138744112925921330/image0.jpg?width=927&height=544)

- 1 to 1
  - แต่ละ user thread map ไปยัง kernel thread
  - มีความต่อเนื่อกว่า Many to 1
  - แต่จำนวน threads จะเกิด overhead ได้

  ![](https://media.discordapp.net/attachments/1131128255735410688/1138744443252506674/image0.jpg?width=897&height=525)

- Many to Many
  - หลาย user map ไปยัง หลายๆ kernel
  - OS สามารถจัดการจำนวน kernel threads ได้
  
![](https://media.discordapp.net/attachments/1131128255735410688/1138744909235507291/image0.jpg?width=937&height=594)

- Two-level model
  - คล้ายๆ Many to Many แต่ว่า allow user thread bound ไปยัง kernel thread หลายๆ แบบได้

### Thread Libraries
- เป็น API ให้ programmer สำหรับสร้างและจัดการ thread
- วิธี implement มีอยู่ 2 แบบ 
  - Lib สำหรับ user-space
  - kernel-level ที่ supported สำหรับ OS

- Exmaple
  - Pthreads
    - มีทั้ง user-level, kernel-level
    - เป็น inteface ไม่ใช่การ implement
  - Java Threads
    - ใช้กับ JVM
    - สร้างได้จาก 
      - Extending Thread Class
      - สร้าง runnable interface

### Implicit Threading
- ได้ัรับความนิยมเพิ่มขึ้น ตามจำนวนของ thread ที่เพิ่มขึ้น , ความถูกต้อง และ explicit threads
- การจัดการ threads ส่วนมากจัดการโดย compilers and runtime lib มากกว่า programmer
- Five methods explored
  - Thread Pools
  - Fork-Join
  - OpenMP
  - Grand Central Dispatch
  - Intel Threading Building Block

### Signal Handling
- ใช้ใน UNIX system สำหรับการดัก event ที่ error
- signal handler ใช้สำหรับ process signal ที่
  - 1. signal จาก particular event
  - 2. signal ที่ส่งมาจาก process
  - 3. signal ที่ handle 1 ใน 2 handlers 
    - default
    - user-defined
  - ทุกๆ signal มี default signal ที่ kernel runs ในขณะที่ handle signal
    - user สามารถแทนที่ config signal เดิมได้ 
    - single thread = single delivery
    - Multi thread signal
      - ส่งไปยัง thread ที่ต้องการ signal applies
      - ส่งไปทุกๆ thread
      - ส่งไปยังบาง thread
      - ส่งไปยัง thread เฉพาะที่รับ signal สำหรับการ process

### Thread Cancellation
- เป็นการหยุดการทำงานก่อนเสร็จ
- เรียกว่า target thread
- มี 2 วิธี
  - Async ทันที
  - Defer ตรวจสอบเป็นช่วงๆ 
- การที่จะ cancel thread ได้ต้องให้ thread มีสถานะนั้นด้วย ตามตารางด้านล่าง 

![](https://media.discordapp.net/attachments/1131128255735410688/1138752602520170537/image0.jpg?width=1199&height=276)

### Thread-Local Storage
- เป็นการที่ทำให้ thread มี copy ของตัวเองได้
- มีประโยชน์เมื่อ ไม่มีการควบคุม thread creation process 
- ต่างจาก local variable
  - local variable จะปรากฎต่อเมื่อ single function invocation
  - TLS ปรากฎข้าม function ได้
- คล้ายๆ static 
  - TLS unique ในแต่ละ thread

### Scheduler Activations
- ทั้ง Many to Many และ Two-level models ต้องมีการ communication เพื่อจัดการจำนวน thread ที่ application ได้ทำการจองไป
- โดยปกติจะใช้ data structure ที่เรียกว่า Lightweight process (LWP)
  - โดยจะเป็น virtual processor บน process ไหนที่สามารถ run ได้
  - แต่ละ LWP จะติดอยู่กับ kernel thread
- scheduler activations จะให้ upcalls เป็น mechanism การสื่อสาร จาก kernel ไปยัง upcall handler ใน thread library
- commination จะ allow application ให้ maintain จำนวน threads ที่ถูกต้อง

## บทที่ 5 CPU scheduling

### CPU scheduler
- CPU scheduler เลือก process ใน ready queue แล้วจอง CPU core ไปยัง 1 ใน ทั้งหมด
- CPU scheduling เลือกได้ตามนี้
  - 1.Switch from running to waiting
  - 2.Switch from running to ready 
  - 3.Switch from waiting to ready
  - 4.Terminate
- 1 กับ 4 สำหรับ process ใหม่ โดยไม่มีทางเลือกอื่น
- 2 กับ 3 สามารถเลือกได้

### Preemptive and Nonpreemptive Scheduling
**preemptive = เกี่ยวกับการจอง**
- nonpreemptive อยู่ใน 1 กับ 4
- ที่เหลือเป็น preemptive

- preemptive สามารถนำไปสู่ race condition ได้ เมื่อ data share among several processes

### Dispatcher
- ให้การควบคุมกับ CPU โดย CPU จะเลือก scheduler ที่นำไปสู่
  - switch context
  - switch to user mode
  - jumping into proper location เพื่อ restart
- Dispatch latency
  - เวลาหน่วงระหว่าง stop one process -> start another running

### Scheduling Criteria
- CPU utilization
- Throughput
- Turnaround time
- Waiting time - เวลาที่รอใน ready queue
- Response time

เป้าหมาย algorithm

- MAX CPU utilization
- MAX Throughput
- MIN Turnaround time
- MIN Waiting time
- MIN Response time


### First Come First Served

![](https://media.discordapp.net/attachments/1131128255735410688/1138758802448060466/IMG_2378.png?width=1031&height=671)

![](https://media.discordapp.net/attachments/1131128255735410688/1138758994815623199/image0.jpg?width=1439&height=668)

- เกิด convoy effect หมายถึง short process อยู่หลัง long process
  - คิดว่ามาจาก one CPU-bound และ I/O bound process

### Shortest Job First (SJF) Scheduling
- เลือก process ที่สั้นสุดก่อน
- preemptive version เรียกว่า shortest-remaining-time-first
- SJF เป็น optimal

![](https://media.discordapp.net/attachments/1131128255735410688/1138759913389178980/image0.jpg?width=1206&height=671)

### Determining Length of NEXT cpu burst
- ใช้ exponential averaging

![](https://media.discordapp.net/attachments/1131128255735410688/1138760261017272391/image0.jpg?width=1133&height=530)

### Shortest Remaining Time (SRT) First Scheduling

- Preemptive version ของ SJN
- เมื่อมี process ใหม่มา จะมีการตัดสินใจว่าจะเลือก process ไหนโดยใช้ SJN Algorithm
- SRT > SJN

![](https://media.discordapp.net/attachments/1131128255735410688/1138761030583980092/image0.jpg?width=1002&height=671)

### Round Robin

- แบ่งเวลาให้กับแต่ละ process เป็นช่วงๆ เรียกว่า time quantum `(q)`

- timmer interrupt จะเกิดทุกๆ quantum เพื่อไปยัง next process
- performance
  - q large -> FIFO (FCFS)
  - q small -> RR

![](https://media.discordapp.net/attachments/1131128255735410688/1138761686350843944/image0.jpg?width=1010&height=671)


### Priority Scheduling

- เพิ่ม priority ในแต่ละ process
- smallest int = highest priority
- ปัญหาคือ process priority ต่ำอาจไม่ได้รับการประมวลผลอีกเลย
- แก้โดยการเพิ่ม priority เมื่อรอนานขึ้นเรื่อยๆ 

![](https://media.discordapp.net/attachments/1131128255735410688/1138762309850902608/image0.jpg?width=1031&height=671)

![](https://media.discordapp.net/attachments/1131128255735410688/1138762539623260201/image0.jpg?width=894&height=671)

### Multilevel Queue

- เพิ่มจำนวน ready queue
- มี scheduling algorithm สำหรับแต่ละ queue
- ตัวอย่างเช่น สร้าง multilevel queue จาก priority , priority ตาม รูปแบบ process

![](https://media.discordapp.net/attachments/1131128255735410688/1138763026795860048/image0.jpg?width=656&height=671)

![](https://media.discordapp.net/attachments/1131128255735410688/1138763284363890718/image0.jpg?width=1166&height=671)


### Multilevel Feedback Queue

- process สามารถขยับไปมาระหว่าง queue ได้
- aging สามารถสร้างได้ด้วยวิธีนี้เช่นกัน
- Example

![](https://media.discordapp.net/attachments/1131128255735410688/1138763814859448360/image0.jpg?width=1206&height=671)

### Thread Scheduling

- user-level and kernel-level มี distinction
- เมื่อ threads support -> threads scheduling ไม่ใช่ process
- many to one , many to many models , thread lib จัดการ user-level threads to run on LWP
- Kernel Thread Scheduled onto available CPU เรียกว่า system-contention-scope (SCS)

### Multi-Processor Scheduling

- CPU scheduling จะซับซ้อนขึ้น เมื่อมี CPU หลายๆ ตัว
- สามารถเป็นไปได้ตามนี้
  - Multicore CPUs
  - Multithreads cores
  - NUMA systems
  - Heterogenous multiprocessing
  - SMP คือ แต่ละ processor มี self scheduling ขงอตัวเอง
    - โดยทุกๆ thread จะอยู่ใน ready queue
    - หรือแต่ละ processor มี private queue of threads

### Multicore Processors

- เป็น trend ปัจจุบัน
- เร็วและกินไฟน้อยกว่า
- multiple threads ก็เติบโตเช่นกัน
  - ได้เปรียบโดย memory stall แล้วให้สลับไปยัง thread อื่นๆ ก่อนเลย

![](https://media.discordapp.net/attachments/1131128255735410688/1138766049530085396/image0.jpg?width=1313&height=352)

### Multithreaded Multicore system

- 1 core > 1 threads
- ถ้า 1 thread เกิด stall ให้สลับไป another thread

![](https://media.discordapp.net/attachments/1131128255735410688/1138766337984970822/image0.jpg?width=1417&height=378)

- โดย intel เรียกว่า hyperthreading
- Two levels of scheduling
  - 1. OS เลือกว่า software threadd ไหนให้รันบน logical cpu
  - 2. ให้แต่ละ core เลือกว่า แต่ละ thread ไหนเลือกรันบน physical core

Load Balancing
- เป็นการ control workload ให้มีประสิทธิภาพ
- Push migration ใช้ตรวจสอบว่าเกิด overload มั้ยถ้าจริงให้ผลักภาระไปยัง cpu อื่นๆ
- pull migration idle processors ดึง waiting task มาจาก busy processors

### Processor Affinity
 
- Soft Affinity - PS พยายามจะให้ thread รันบน processor เดียวกัน แต่ไม่มีการันตี
- Hard Affinity - allow ให้ process เจาะจง processor ที่จะ run on

### Real-Time CPU scheduling

RT = real time

- Soft RT ให้ task ที่มี priority สูงสุดทำงาน ทันที แต่ไม่มีการันตีว่า task ไหนจะทำงานต่อไป
- Hard RT - execute by deadline

![](https://media.discordapp.net/attachments/1131128255735410688/1138767988829454346/image0.jpg?width=1148&height=671)

### Interrupt Latency

![](https://media.discordapp.net/attachments/1131128255735410688/1138768354144960512/image0.jpg?width=742&height=671)

### Dispatch Latency

- เป็น conflict phase ของ dispatch latency

1. preemption of any process running in kernel mode
2. ปล่อยโดย low-priority process of resource ที่ต้องการโดย high-priority processes
  
![](https://media.discordapp.net/attachments/1131128255735410688/1138768772145094706/image0.jpg?width=1223&height=671)

### Priority-based Scheduling

- hard real-time เจอกับ deadlines
- process มี character ใหม่โดยมี constant interval
  - processing time **t** deadline **d** period **p**
  - 0 <=t <= d <= p
  - rate periodic task = 1/p

![](https://media.discordapp.net/attachments/1131128255735410688/1138769495155019886/image0.jpg?width=1174&height=359)

### Rate Monotonic Scheduling

- เวลาสั้นกว่า = priority สูงกว่า
- เวลายาวกว่า = priority ต่ำกว่า
- แต่มีปัญหาคือ miss deadlines ได้

### Earliest Deadline First Scheduling (EDF)

- earlier -> higher
- later -> lower

![](https://media.discordapp.net/attachments/1131128255735410688/1138770051709800509/image0.jpg?width=1439&height=267)

### Proportional Share Scheduling

- T shared เป็นการจองทุกๆ process ใน system
- Application รับ N โดย N < T
- แต่ละ application จะได้ N/T เป็นเวลา processor รวม

### ตัวอย่าง OS

- Linux 
  - มี priority 2 แบบ คือ time-sharing , real-time
  - priority ใหญ่สุดจะได้ q ที่ใหญ่กว่า
  - ใช้ runqueue data structure สำหรับแต่ละ cpu
  - version หลัง 2.6.23
    - quantum คำนวณจาก nice value โดยอยู่ในช่วง -20 -> +19
    - lower value = higher priority
  - มีการใช้ scheduling domain
- Windows
  - ถ้าไม่มี run-able thread จะรัน idle thread
  - WIN32 API เป็นตัวจัดการ priority class โดยจะบอกว่าจะต้องจัดการ process ยังไง
  - ถ้า quantum หมดอายุ ให้ทำการ ลด priority

![](https://media.discordapp.net/attachments/1131128255735410688/1138771369845338183/image0.jpg?width=1413&height=631)

- Solaris
  - แต่ละ class มี algorithm จัดการ scheduling ของตัวเอง
  - จะรันจนกว่า 1. blocks 2. time slice 3. preempted by higher priority thread

### Queueing Models

- ใช้อธิบายการมาของ process และ CPU และ I/O bursts 
  - โดยปกติเป็น expo จะบรรยายโดยเป็น mean
  - ประมวลผล throughput, utilization, waiting time, etc.
- computer system อธิบาย network of server โดยแต่ละ queue ของ waiting process
  - ทราบถึง arrival reates และ service reates
  - ประมวลผล utilization, ค่า mean ของ ความยาว queue , wait time avg. etc

### Simulations
- เนื่องจาก model queue มันจำกัด 
- simulation ที่แม่นยำกว่า
  - clock
  - programmed model
  - statistics
  - data to drive simulation 
    - RNG
    - Math
    - Tapes Record

Implementation
  - limited accuracy
  - High cost High risk
  - Environment Vary