# Operating Systems
## These notes were made while studying operating systems from the OSTEP book.

## Part1: Virtualization

### Process API

- The OS uses a technique to run multiple concurrent process called time-sharing which enables virtualization.
- A process is just an API or abstraction for a running program and the system resources like memory that it can use.
- The process API has methods such as create, destroy, wait, status and others.
- Program code (or instructions) are permanently saved on the disk along with static data (if any). It is loaded into the memory (RAM) when a process needs to be created. (these programs are loaded lazily)
- After the process is loaded into the memory some amount of the memory is allocated for storing the program, static data and for the runtime stack, running it. The OS might also initialize some heap memory and I/O based initialization such as giving the process the 3 file descriptors (stdin, stdout, stderr).
- There are 3 basic process states: running, ready and blocked.
- There are special states like the zombie states which means that a process has exited but has not yet been cleaned up.
- A process keeps a track of several differnt details. Here is a basic process struct:

```C
// the registers xv6 will save and restore
// to stop and subsequently restart a process
struct context {
    int eip;
    int esp;
    int ebx;
    int ecx;
    int edx;
    int esi;
    int edi;
    int ebp;
};

// the different states a process can be in
enum proc_state { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE };

// the information xv6 tracks about each process
// including its register context and state
struct proc {
    char *mem; // Start of process memory
    uint sz; // Size of process memory
    char *kstack; // Bottom of kernel stack for this process
    enum proc_state state; // Process state
    int pid; // Process ID
    struct proc *parent; // Parent process
    void *chan; // If non-zero, sleeping on chan
    int killed; // If non-zero, have been killed
    struct file *ofile[NOFILE]; // Open files
    struct inode *cwd; // Current directory
    struct context context; // Switch here to run process
    struct trapframe *tf; // Trap frame for the current interrupt
};
```

### Mechanism

- OS aims to virtualize CPU resources while making sure that performance and control (over procs) is achieved.
> AISDE: System calls look similar to procedure calls because they are infact procedures defined in the c library. A call to e.g. `open()` calls the C code which implements a trap instruction (hand-coded assembly instructions that are provided by the hardware to trap into the kernel and return-from-trap) which transfers control to the kernel. After the kernel is done completing the task the result and the control both are transferred back the procedure (user program).

- Control over processes is achieved by having two process modes. The code which runs in `user mode` cannot do priveleged operations such as I/O whereas `kernel mode` allows the code (the OS) to do all sorts of priveledged 
- Whenever a user process wants to execute priveledged operations it calls the relevant syscall to do it. Syscalls act as a bridge b/w user and kernel mode. Syscalls are essentially an API for priveledged operations that the kernel exposes to the user programs.
- When toggling b/w proc modes the hardware must make sure to save all the necessary registers of the caller to be able to return after completion. For e.g.
> On x86, for example, the processor will push the program counter, flags, and a few other registers onto a per-process kernel stack; the return-fromtrap will pop these values off the stack and resume execution of the usermode program
- A trap table is used by the kernel to know which code to run in kernel mode. The trap table is initialized during system boot (which runs in kernel mode). For example, what code should run when a when a keyboard interrupt occurs. So during boot up the hardware is made to remember address of syscall handler. [Ref: Fig 6.2 Pg 71]
- This approach offers a certain protection to system resources as instead of referring to a certain piece of code to jump to the user programs request a certain service through syscalls. This is called Limited Direct Execution (LDE protocol).
- Since the two proc modes solve the priveledge the OS also needs to find a way to switch b/w processes and more importantly switch b/w a running user proc and itself. 
- There are two ways of doing this. A co-operative approach expects the control to be transferred back to OS if (1) the user program executes a syscall to transfer control back & (2) if it performs an illegal operation and the OS stops and terminates it. But this approach fails in cases in which neither of this happens (say an infinite loop)
- A non-cooperative approach uses a special timer interrupt (started by the OS during boot time) which is programmed to transfer the control back to the OS at specified intervals of time.
- After transferring the control the previous process can either be resumed or the OS can switch to a new process (decided by the scheduler). In case of a switch a piece of code known as context switch is executed (which saves a few registers etc)

##### Context Switch

- The first is when the timer interrupt occurs; in this
case, the user registers of the running process are implicitly saved by the hardware, using the kernel stack of that process.
- The second is when the OS decides to switch from A to B; in this case, the kernel registers are explicitly saved by the software (i.e., the OS), but this time into memory in
the process structure of the process. The latter action moves the system from running as if it just trapped into the kernel from A to as if it just trapped into the kernel from B

### Scheduling Policies

- Turnaround time of job `Tturnaround = Tcompletion − Tarrival`
- In FCFS if the longer (resource heavy) jobs are run prior to the shorter ones it can increase the TAT by a lot. This is known as the convoy effect whereas in SJF this effect is minimized to quite a large extent (if all procs arrive at the same time).
- Both these policies are non-preemptive and not used in modern schedulers
- SCTF a preemptive SJF can be used instead to run the jobs which have the least remaining time to complete. This leads to lesser avg TAT
- Another metric Response Time `Tresponse = Tfirstrun − Tarrival` when taken into account highlights the area where SCTF or SJF might not be suitable policies, the area being interactivity. If all the procs arrive at the same time according to the aforementioned algorithms the last proc will have a large enough response time to hamper the interactivity of the system.
- Round Robin greatly decreases response time for each process by running each proc for a specified time slice/quantum, although RR dissappoints when we take TAT into account. The length of the time slice is usually a multiple of the **timer interrupt**.
- Shorter time slices lead to better responsiveness but if they are too short they can lead to a much slower peformance due to an increased overhead of execessive context switching.
- To reduce the cost of context switching the duration of the time slice can be adjusted in such a way that a very small %age of the time slice is spent in context switching.