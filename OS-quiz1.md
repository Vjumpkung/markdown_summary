# สรุป OS 
## บทที่ 1 Introduction
### Objective
- อธิบายการจัดการพื้นฐานของ OS
- การทำงานของ user mode -> kernel mode

### OS คืออะไร
คำตอบคือ ก็ไม่รู้เหมือนกัน
- คำตอบคือก็เหมือนกับ 
  - รถ
  - เครื่องบิน
  - เครื่องพิมพ์
  - เครื่องซักผ้า
  - เตาปิ้งขนมปัง
  - คอมพิวเตอร์
  
**แต่** เราสามารถอธิบายหน้าที่คร่าวๆ ได้คือ เป็นตัวกลางระหว่าง user กับ hardware โดยมีเป้าหมายคือ 
  - ช่วย Human แก้ปัญหาในชีวิตประจำวันได้
  - ช่วยทำให้ Computer System ทำงานได้เต็มที่

### Computer System Structure

สามารถแบ่งได้เป็น 4 ส่วนคือ 
- Hardware
  - CPU, memory, I/O
- **Operating System** วิชานี้จะโฟกัสที่ตรงนี้
  - ควบคุมการใช้งานของ hardware สำหรับ application ต่างๆ
- Application Program
  - เป็นโปรแกรมที่จะใช้งานบน OS
- User
  - คน, เครื่องจักร หรือ computer เครื่องอื่นๆ 

![](https://media.discordapp.net/attachments/1131128255735410688/1131128305538580490/image0.jpg?width=1039&height=676)

### OS ทำหน้าที่อะไรบ้าง
- สำหรับ users ต้องการ **เร็ว** และ **แรง** แต่ไม่ได้สนใจในเรื่องของการจัดการ resource ที่ดี
- shared computer เช่น mainframe หรือ mini computer ต้องทำให้ประสิทธิภาพของ OS นั้นเสถียร สำหรับทุกคน 
- Workstation ก็ใช้ resource จาก server เช่นกัน
- Mobile จะโฟกัสไปที่ การใช้ touch screen และ การจัดการพลังงานที่ดี
- Embedded Computer ไม่จำเป็นต้องมี user interface 

### ความหมายของ OS (ต่อ)
- OS ไม่ได้มีนิยามเห็นด้วยตรงกัน
- คำว่า "หนึ่งโปรแกรมที่ run ทุกเครื่อง" มันก็คือ kernel ใน OS
- หรือจะแบ่งเป็นแบบนี้ก็ได้
  - System Program
  - Application Program 
- middleware คือโปรแกรมที่อยู่กึ่งกลางระหว่าง OS กับ Application อีก เช่น Database, Multimedia, Graphics

### Computer System Structure
![](https://media.discordapp.net/attachments/1131128255735410688/1131134844185870386/image0.jpg)

- การทำงานของ Computer System
  - I/O Devices สามารถทำงานพร้อมๆ กันได้
  - I/O Devices เป็นการนำ local buffer ไปยัง controller
  - Device Controller
    - มี local buffer ไว้เก็บข้อมูล
    - มี Driver เป็นของตัวเอง
  - เมื่อ CPU ทำงานเสร็จสิ้นจะเกิด **Interrupt** ขึ้น

### Interrupt
- Interrupt Transfer control -> Interrupt Service Routine จะทำงานผ่าน Interrupt Vector
- OS เป็น Interrupt Driven
- trap หรือ Exception สามารถเกิดได้จาก error หรือ user-request

Example
```
.
.
.
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
KeyboardInterrupt <-- จุดเกิด Interrupt
```

### I/O Structure
- สองอย่างที่ I/O ต้องทำ
  - หลัง I/O เริ่มทำงงาน ตัว control คืนค่าให้กับ user program เมื่อ I/O completion
    - รอ instruction ไปหยุด CPU จนกว่าจะเจอ Intrerrupt
    - รอ loop (หมายถึง memory access)
  - หลัง I/O เริ่มทำงงาน ตัว control คืนค่าให้กับ user program แบบไม่ต้องรอ I/O completion
    - System Call
    - Devices-Status table

### Storage Structure
![](https://media.discordapp.net/attachments/1131128255735410688/1131140816887357541/image0.jpg?width=1057&height=676)

### Operating System Operation
- Bootstrap Program -> Kernel Load -> system daemons -> Kernel Interrupt Drivem (HW and SW)

### Multiprogramming (Batch System)
- ใช้สิ่งที่เรียกว่า Job Scheduling ในการทำงาน

### Process , Memory, File-system Management
- Process Management
  - program ในขณะทำงานจะเป็นหน่วยย่อยของระบบ โดย Program เป็น Passive Entity และ Process เป็น Active Entity
  - Process Termination ต้องมีการ reclaim ทรัพยากรภายใน computer system
- Process Management Activities
  - Mechanisms ที่ทำมีดังนี้
    - Syncronization
    - Communication
    - Deadlock Handling
- Memory Management
  - execute โปรแกรมให้อยู่ได้ใน memory โดยขึ้นอยู่กับ OS ว่าจะให้ทำงานอย่างไร
    - ตรวจสอบ ว่าส่วนของไหนของ memory กำลังใช้อยู่หรือว่างอยู่
    - control process ว่าอันไหนอยู่หรือไป (Into and Out of Memory)
    - Allocating และ Deallocation `(malloc and free)`
- File-System Management
  - เป็นการจัดแต่ละส่วนของ file 
  - สร้าง/ลบ file และ directory

### Caching
- ใช้สำหรับ copy จาก storage ที่ช้ากว่าไป storage เร็วกว่า

### I/O Subsystem
- เป็นจุดประสงค์ที่ OS ซ่อนสิ่งที่ user มองไม่เห็น
- Memory management -> buffering, caching, spooling
- Driver เรียก hardware เฉพาะ
- เป็น Device-Driver interface ปกติ

### Protection And Security
- Protection
  - ทุกอย่างๆ ที่เกี่ยวกับการควบคุมสิทธิ์ ที่กำหนดโดย OS
- Security
  - การที่ป้องการจาก virus, malware
- OS จะมีการกำหนดสิทธิ์ของ user
  - UserID
  - GroupID

### Virtualization
- เป็นการที่ทำให้ Application จาก OS อื่นๆ สามารถทำงานได้
  - Emulation 
  - Virtualization

![](https://media.discordapp.net/attachments/1131128255735410688/1131150334434213888/image0.jpg?width=976&height=676)
<hr>


### Computer System Architecture

ข้ามคับเพราะว่าเป็นเรื่องที่เรียนมาแล้ว

### Computing Environments

- Traditional
  - คอม PC ทั่วไป mainframe
- Mobile
  - มือถือ smartphone (iOS Android)
- Client Server
- P2P
- Cloud Computing
  - Amazon EC2
- Real-time Embedded

### Kernel Data Structure
อ่านเพิ่มเติมในวิชา ADT ได้

- Linked List
  - Singly Linked List
  - Doubly Linked List
  - Circular Linked List
- Binary Search Tree
- Hash Function
- Bitmap

<hr>

## บทที่ 2 Operating-System Services

### Objective
- สามารถ identify services ที่ OS ให้มา
- สามารถอธิบาย System Calls ที่ OS ให้มาได้
- เปรียบเทียบ Monolihtic, Layered, Microkernel, Modular และ Hybrid
- อธิบายกระบวนการทำงานของการ boot OS
- ออกแบบ Linux Kernel Module

### OS-service 
- User Interface
- Program Execution
- I/O Operation
- File-System Manipulation
- Communication
- Error Detection
- Resource Allocation
- Logging
- Protection and Security

![](https://media.discordapp.net/attachments/1131128255735410688/1131170393143644170/image0.jpg?width=1305&height=676)

### Command Line Interpreter
- CLI เป็น การที่พิมพ์คำสั่งลงไปตรงๆ เลย
- CLI อยู่ใน kernel หรือ system program
- บางทีก็มีการตกแต่งเหมือนกัน (shells ex. explorer, control panel)

### GUI

![](https://www.zdnet.com/a/img/resize/a4f13f86a25b11a2bc8aeec027ce8384b047dcf2/2014/09/18/bcbcf400-3f0f-11e4-b6a0-d4ae52e95e57/guis-the-computing-revolution-that-turned-us-into-cranky-idiots.jpg?auto=webp&fit=crop&height=1200&width=1200)

- มี UI/UX ที่ดี
- ใช้งานผ่าน mouse, keyboard และ monitor
#### บาง OS มี GUI และ CLI ให้ใช้งานพร้อมกัน
- macOS
- Windows
- Linux

### System Calls
- เป็น Programming Interface ที่ OS ทำมาให้
- เขียนด้วย C, C++
- โดยปัจจุบัน โปรแกรมจะเรียก API มากกว่า System Calls ตรงๆ 
- API ที่พบเห็นได้ง่ายคือ Win32 API, POSIX API, Java API

![](https://media.discordapp.net/attachments/1131128255735410688/1131176598893244456/image0.jpg?width=901&height=676)

### Types of System Calls
- Process Control
  - create, terminate process
  - end, abort
  - load, execute
  - get/set attributes, set process attributes
  - wait for time
  - wait event, signal event
  - allocate and free memory
  - Dump memory if error
  - Debugger
  - Locks
- File Management
  - create file, delete file
  - open file, close file
  - read, write, reposition
  - get and set attributes
- Device Management
  - request device, release device
  - read, write, reposition
  - get device attributes, set device attributes
  - logically attach or detach devices
- Information Maintenance
  - get/set time or date
  - get/set system data
  - get/set process, file, or device attributes
- Communications
  - create, delete communication connection
  - send receive messages
  - shared memory model creation and gain access to memory regions
  - transfer status information
  - attach or detach remote devices
- Protection
  - control access to resources
  - get and set permission
  - allow and deny user access

**UNIX และ Windows ก็มี system calls ต่างกัน**

![](https://media.discordapp.net/attachments/1131128255735410688/1131178793059496038/image0.jpg?width=615&height=676)

### System Services
มีหน้าที่ดังนี้
- File Maniuipulation
  - create delete move copy rename print dump list and generally manipulate files and directories
- Status information sometimes stored in a file
  - Date, Time, Amount of available memory, disk space, number of users
  - Details of the system's resources
  - registry
- Programming language support
- Program loading and execution
- Communications
- Background services
  - Launch At Boot time
- Application Program
  - Run by users

**User จะเห็น OS ในส่วนของแค่ System Program จะไม่เห็น function System Calls**

### Linkers and Loaders
- source code ที่ complied แล้วเป็น relocatable object file
- Linker เป็นการรวมทุกอย่างเข้าเป็น executable file
- โปรแกรมจะอยู่ที่ secondary storage
- วิธีการใช้งาน คือ โปรแกรมจะถูก load ไปที่ memory แล้วทำงาน
- โปรแกรมในปัจจุบันไม่ได้ link library เข้ามาใน executable file แต่จะเป็น dynamic link library แทน 
  - เช่น DLL
  
![](https://media.discordapp.net/attachments/1131128255735410688/1131190116740775976/image0.jpg?width=745&height=676)

### ทำไม programs ถึงต้องการระบบที่เฉพาะเจาะจง

- Application ที่ complied แล้วจะสามารถ execute ได้แค่ระบบที่ complied ไว้เท่านั้น
- แต่ละ OS มี system call ที่ไม่เหมือนกัน
- แอปที่เขียนสำหรับหลากหลายระแบบ ได้แก่
  - interpreted language เช่น python, Ruby
  - ใช้ VM เช่น Java, .NET
- Application Binary Interface เป็นสิ่งที่เทียบเท่ากันกับ API ของ OS

### Design and Implementation

- ไม่มีสูตรสำเร็จในการทำ OS
- แต่ละ OS มีการออกแบบที่แตกต่างกัน
- เริ่มต้นด้วย การกำหนดเป้าหมายของ OS ก่อน
- hardware , รูปแบบของ system ก็ทำให้เป้าหมายเปลี่ยนได้เช่นกัน
- Users goals and System Goals
  - User goals หมายถึงเน้นทำระบบให้ user ใช้งานง่าย รวดเร็ว เสถียร และปลอดภัย
  - System goals หมายถึงออกแบบได้ง่าย ปรับปรุง ยืดหยุ่น error น้อย และ มีประสิทธิภาพ
- เป็นหน้าที่ของ software engineering

### Policy and Mechanism
- Policy : คือ การกำหนดว่าจะทำอะไรบ้าง
  - เช่น : Interrupt หลังจาก 100 วินาที
- Mechanism : คือ การทำงาน
  - เช่น : timer

### Implementation
- มีหลากหลายวิธี
  - assembly
  - Algol, PL/1
  - C, C++
- ปัจจุบันใช้หลากหลายภาษา
  - Lowest level : assembly
  - Main Body : C
  - System Program : C, C++, Scripting Language
- ถ้าใช้ High Level 
  - port ไปที่อื่นได้ง่าย แต่ช้า
- Emulation : เป็นวิธีที่ทำให้ OS ทำงานได้บน OS อื่นๆ ได้
  - แต่จะช้ากว่า native OS ที่ถูกออกแบบมาให้ทำงานบน hardware นั้นๆ

### OS Structure
- General-purpose OS เป็นโปรแกรมที่ขนาดใหญ่มาก
- แต่ละ OS มีการออกแบบที่แตกต่างกัน
  - simple structure - MS-DOS
  - more complex - UNIX
  - layered 
  - microkernel - Mach

### Layered Approach
- แบ่งเป็น layer ต่างๆ ที่มีความสำคัญต่างกัน
- layer 0 คือ hardware

### Microkernel
- Mach คือตัวอย่าง OS แบบ microkernel ที่ใช้ใน macOS

### System Boot
- เมื่อเปิดเครื่องมาจะเริ่มที่ memory location ที่เจาะจงไว้
- OS ต้องมีพื้นที่อยู่ใน hardware ถึงจะเริ่มได้
  - BIOS จะทำการตรวจสอบ hardware และทำการ load OS จาก disk มาไว้ใน memory
  - Boot Block
  - UEFI
- bootloader เช่น grub เพื่อโหลด kernel

### Operating-System Debugging
- เป็นการแก้บัค หรือ ตรวจสอบ bug
- performance tuning ก็นับเช่นกัน
  - trace listings
  - profiling
- core dump (เมื่อ application crash)
- crash dump (เมื่อ OS crash)

### Performance Tuning
- เปิด Task Manager, top ก็ถือว่าเป็นการตรวจสอบแล้ว
### Tracing
- เป็นการเก็บข้อมูลการทำงานของโปรแกรม เป็น step by step
- ตัวอย่าง tools
  - strace
  - gdb
  - perf
  - tcpdump