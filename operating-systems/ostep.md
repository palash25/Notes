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
