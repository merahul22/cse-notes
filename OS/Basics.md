### **What is an Operating System (OS)?**

An **Operating System** is system software that acts as an interface between hardware and software. It manages all resources of a computing device, controls the execution of programs, and ensures efficient system operation. Popular OS examples include **Windows, Linux, macOS, Android, and iOS**.
![[Introduction-of-OS-660.webp]]
### **Uses of an Operating System**

- **Platform for applications:** Runs application programs.
    
- **Resource management:** Manages I/O devices, memory, CPU, etc.
    
- **Multitasking:** Supports running multiple applications simultaneously.
    
- **Security:** Provides protection through authentication and access control.
    
- **Memory and file management**
### **Functions of an Operating System**

- **Resource & Process Management:** Allocates memory, CPU, and manages processes.
    
- **Memory & File Management:** Controls usage and access of memory and files.
    
- **Security & Job Accounting:** Ensures secure operations and tracks resource usage.
    
- **Device & Network Management:** Interfaces with hardware and supports network tasks.
    
- **User Interface:** Offers GUI or CLI for user interaction.
    
- **Backup, Recovery & Virtualization**
    
- **Performance Monitoring, System Calls, and Error Handling**
### **Objectives of Operating Systems**

- Make computer usage **convenient and efficient**
    
- Provide a **user-friendly interface**
    
- Ensure **fair resource sharing**
    
- Enable **easy access to hardware resources**
    
- Manage and monitor resource usage

### **Types of Operating Systems**

1. **Batch OS** – Processes jobs in batches without user interaction.
    
2. **Time-Sharing OS** – Multiple users share resources simultaneously.
    
3. **Distributed OS** – Manages a network of computers as one system.
    
4. **Network OS** – Provides networking features and resource management.
    
5. **Real-Time OS** – Quick response for critical applications (e.g., robotics).
    
6. **Multiprocessing OS** – Supports multiple CPUs.
    
7. **Single-User OS** – One user at a time (e.g., Windows).
    
8. **Multi-User OS** – Multiple users simultaneously (e.g., Unix).
    
9. **Embedded OS** – For devices with limited resources (e.g., Android, iOS).
    
10. **Cluster OS** – Coordinates multiple systems for high performance.
### **Choosing an Operating System**

Key factors to consider:

- **Price:** Free (Linux) vs Paid (Windows, macOS)
    
- **Ease of use:** User-friendliness
    
- **Compatibility:** Application and hardware support
    
- **Security:** Built-in protection features
### **Examples of Operating Systems**

- **Windows** – GUI-based OS for PCs
    
- **Linux** – Open-source OS for servers and desktops
    
- **macOS** – Apple's OS for desktops/laptops
    
- **Android** – Google's mobile OS
    
- **iOS** – Apple’s OS for mobile devices

# ### **Functions of an Operating System (OS)**

An Operating System serves as an interface between the user and computer hardware, aiming to provide a convenient, secure, and efficient computing environment.
### **Core Functions:**

1. **Process Management**
    ![[Process-Managementos.webp]]
    - Manages all running programs (processes).
        
    - Handles **scheduling**, **synchronization**, **deadlock prevention**, and **inter-process communication (IPC)**.
        
    - Ensures fair CPU usage and smooth multitasking.
        
2. **Memory Management**
    ![[Memory-Managementos.webp]]
    - Manages allocation and deallocation of main and secondary memory.
        
    - Supports **virtual memory**, **paging**, **segmentation**, and **fragmentation management**.
        
    - Ensures memory protection among processes.
        
3. **File System Management**
    ![[File-System-Managementos.webp]]
    - Organizes, stores, and retrieves data via files and directories.
        
    - Supports **file operations** (create, read, write, delete), **file access methods**, and **permissions**.
        
    - Manages file attributes and different file types (text, binary, executable).
        
4. **Device Management (I/O System)**![[Device-Managementos.webp]]
    
    - Controls hardware devices using **device drivers**.
        
    - Implements **buffering**, **caching**, and **spooling** for efficient I/O operations.
        
    - Coordinates communication between CPU and peripheral devices.
        
5. **Protection and Security**![[Protection-and-Securityos.webp]]
    
    - Provides **access control**, **user authentication**, and **resource protection**.
        
    - Defends against **malware** and **unauthorized access** through internal and external security mechanisms.
        

---

### **Additional Responsibilities:**

- **System Performance Monitoring**
    
    - Tracks system response times and identifies performance bottlenecks.
        
- **Job Accounting**
    
    - Logs system usage for auditing, billing, and optimization.
        
- **Error Detection and Debugging**
    
    - Generates diagnostic data like dumps and error messages to maintain system reliability.
Here’s a **clean, structured summary** of the **8 main types of Operating Systems**, based on purpose and functionality:

---

## 🧠 **Types of Operating Systems**

Operating Systems (OS) manage hardware and software resources and are categorized by **functionality** into the following 8 types:

---

### 1. **Batch Operating System**

- **Definition**: Executes batches of jobs without user interaction. Jobs with similar needs are grouped.
    ![[types1-(1)os.webp]][[]]
- **Advantages**:
    
    - Shared by multiple users
        
    - Efficient for repetitive tasks
        
    - Minimal idle time
        
- **Disadvantages**:
    
    - Poor CPU utilization during I/O
        
    - Failure of one job delays others
        
    - High response time
        
- **Examples**: Payroll systems, bank statements
    

---

### 2. **Multi-Programming Operating System**
![[Types-of-OS-03-660os.webp]]
- **Definition**: Multiple programs reside in memory, sharing CPU time to optimize resource use.
    
- **Advantage**: Better CPU utilization and reduced response time
    

#### ⮞ **Time-Sharing (Multitasking) OS**
![[Types-of-OS-01os.webp]]
- **Definition**: Type of multiprogramming where each process gets CPU time (quantum) in turn
- **Advantages**:
    
    - Equal CPU opportunity for tasks
        
    - Efficient resource sharing
        
    - Improved user experience
        
- **Disadvantages**:
    
    - Complex and less secure
        
    - Higher overhead
        
- **Examples**:
    
    - IBM VM/CMS
        
    - TSO (IBM System/360)
        
    - Windows Terminal Services
        

---

### 3. **Multi-Processing Operating System**
![[Types-of-OS-02-660os.webp]]
- **Definition**: Uses multiple CPUs for parallel processing
    
- **Advantages**:
    
    - Higher throughput
        
    - Fault tolerance with multiple processors

### 4. **Multi-User Operating System**
![[Capture2210os.png]]
- **Definition**: Allows multiple users to access a single system simultaneously (via interleaving or multiple processors)
    
- **Note**: Often overlaps with time-sharing systems
    

---

### 5. **Distributed Operating System**
![[Types-of-OS-04os.webp]]
- **Definition**: Manages a group of independent, networked computers as a single system
    
- **Advantages**:
    
    - Fault-tolerant and scalable
        
    - High-speed data exchange
        
    - Load distribution
        
- **Disadvantages**:
    
    - Complex and costly
        
    - Network failure affects communication
        
    - Security and consistency challenges
        
- **Example**: LOCUS
    

---

### 6. **Network Operating System (NOS)**

- **Definition**: Installed on servers to manage networked computers
    ![[types-7-(1)os.webp]]b
- **Advantages**:
    
    - Centralized security and management
        
    - Remote access
        
    - Easy upgrades
        
- **Disadvantages**:
    
    - Expensive servers
        
    - Dependence on central server
        
    - Frequent maintenance
        
- **Examples**:
    
    - Windows Server 2003/2008
        
    - UNIX, Linux
        
    - Mac OS X, Novell NetWare, BSD
        

---

### 7. **Real-Time Operating System (RTOS)**![[Types8os.webp]]

- **Definition**: Processes data and responds instantly within strict time constraints
    
- **Types**:
    
    - **Hard RTOS**: No delay tolerated (e.g., airbags)
        
    - **Soft RTOS**: Small delays acceptable
        
- **Advantages**:
    
    - Fast and error-free processing
        
    - Efficient memory allocation
        
    - Ideal for embedded systems
        
- **Disadvantages**:
    
    - Handles fewer tasks
        
    - Complex algorithms
        
    - Needs high-performance hardware
        
- **Examples**:
    
    - Missile control systems
        
    - Medical imaging
        
    - Robots, Air traffic systems
        

---

### 8. **Mobile Operating System**

- **Definition**: Designed for smartphones and tablets (e.g., Android, iOS)
    
- **Advantages**:
    
    - User-friendly
        
    - Extensive app ecosystems
        
    - Connectivity and regular updates
        
- **Disadvantages**:
    
    - Battery constraints
        
    - Security risks
        
    - Device fragmentation (especially in Android)
        
    - Limited hardware resources
        
## **Need for Operating System**

1. **Platform for Application Programs**
    
    - Acts as a platform to run application software.
        
    - Interface between user and hardware.
        
2. **Manages Input/Output Devices**
    
    - Controls and allocates I/O resources (keyboard, monitor, printer).
        
3. **Multitasking**
    
    - Allows multiple programs to run simultaneously.
        
    - Uses shared memory for inter-process communication.
        
4. **Software Management**
    
    - Controls, executes, and manages various software applications.
        
5. **Memory Control**
    
    - Allocates and deallocates main memory to processes.
        
6. **File Management**
    
    - Manages data stored as files.
        
    - Helps in accessing and organizing files efficiently.
        
7. **Security**
    
    - Ensures system and data protection using authentication and authorization.
        

---

## 🔹 **Functions of an Operating System**

1. **Processor Management**
    
    - Allocates CPU time to different processes (Scheduling).
        
    - Techniques:
        
        - **Shortest Job First (SJF)**
            
        - **Round Robin**
            
        - **Priority-Based Scheduling**
            
    - **Context Switching**: Saves and loads process states during multitasking.
        
2. **Device Management**
    
    - Manages hardware communication using:
        
        - **Buffering**: Temporary storage of I/O data.
            
        - **Spooling**: Queueing tasks for shared devices (e.g., printer).
            
3. **Memory Management**
    
    - Allocates memory to processes using:
        
        - **Partitioning**: Fixed or variable memory blocks.
            
        - **Virtual Memory**: Allows execution of programs larger than main memory.
            
4. **File Management**
    
    - Manages files using:
        
        - **FAT (File Allocation Table)** or **inode** (Linux).
            
    - Handles file creation, editing, memory allocation, and access permissions.
        

---

## 🔹 **Operating System Services**

1. **Program Execution**
    
    - Loads programs into memory and executes them.
        
2. **I/O Operations**
    
    - Controls input/output device communication.
        
3. **Communication**
    
    - Allows inter-process communication using message passing.
        

---

## 🔹 **Types of Operating Systems**

| Type               | Description                                                     | Example                          |
| ------------------ | --------------------------------------------------------------- | -------------------------------- |
| **Batch OS**       | Groups similar jobs into batches. No direct user interaction.   | Payroll systems, bank processing |
| **Time-Shared OS** | Allows multiple tasks to share CPU time simultaneously.         | Multitasking environments        |
| **Distributed OS** | Multiple CPUs with distributed tasks, no shared memory/clock.   | LOCUS                            |
| **Network OS**     | Allows resource sharing over a network (files, printers, etc.). | LAN server systems               |