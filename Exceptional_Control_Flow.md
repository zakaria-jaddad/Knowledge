### Exceptional Control Flow

From the time you first apply power to a processor until the time you shut it off,
the program counter assumes a sequence of values, where each value is the address of some
corresponding instruction.

Each _transition_ is called the `flow of control` or `control flow`.

Systems must also be ablt to react to changes in system stat that are not captured by internal program variables
and are not related to the execution of the program.

Modern systems react to these situations by making abrupt changes in the control flow.

We refer to these abrup changes as **exceptional control flow (ECF)**
Exceptional control flow occurs at all levels of a computer system.
For example

- At the hardware level, events detected by the hardware trigger abrupt control transfers to
  exception handlers.
- At the operating systems level, the kernel transfers control from one user process to another
  via context switches.
- At the application level, a process can send a signal to another process that abruptly transfers
  control to a signal handler in the recipient

## Exceptions

An _exception_ is an **abrupt** change is the ocntrol flow in response to some change int the processor's
state.

The processor is executing some current instructions, when a significant change in the processor's state
occurs, The state is encoded in various bits and signals inside the processor.

The change in state is known as an _event_.

The event might be directly related to the execution of the current instruction.

For example

- A virtual memory page fault occurs
- An arithmetic overflow occurs
- An instruction attempts a divide by zero.

On the other hand, the event might be unrelated to the execution of the current instruction.
For example

- A system timer goes off
- An I/O request completes.

When the processor detects that the event has occurred, it makes and indirect procefure call the **exception**
, through a jump table called and _exception table_ to an operating system subroutine the **exception handler**
that is designed to process this particular kind of event.

When the exception handler is finished processing, one of three thing happens.

1. The handler returns control to the current instruction Icurr, the instruction that was executing when the event occurred.
2. The handler returns control to next instruction,the instruction that would have executed next had the exception not occurred.
3. The handler aborts the interrupted program.

Anatomy of an exception.
A change in the processor’s state _event_ triggers an abrupt control transfer
_an exception_ from the application program to an exception handler.
After it finishes processing, the handler either returns control to the interrupted program or aborts.

![Anatomy of Exception](./media/anatomy-of-exception.png)

### Exception Handling

![Exception Table](./media/exception-table.png)

The Exception Table has a number of entries each entry is represented with a uniq, nonnegative integer
called _exception number_ and each entry contains the address of the handler code for the exception _k_.

Some number are assigned by the designer of the processor, other number are assigned by the designers of the
operating system such: division by zero, page faults, memory access violations, break points, and arithmetic overflows, system calls and signals.

At system boot time **when the computer is reset or powered on**,
the operating system allocates and initializes a jump table called an **exception table**

At the run time, when the system is executing some program, the processor detects that an event has occurred
it determines the corresponding exception number in the exception table.

The processor then triggers the exception calling the entry of the handler.

The exception number is an index into the exception table, whose starting address is contained in a special _CPU_ register
called the _exception table base register._

![Generating the address of an exception handler](./media/exception-table-number.png)
Once the hardware triggers the exception, the rest of the work is done in software by the exception handler.

After the handler has processed the event, it optionally returns to the interrupted program by executing a special
“return from interrupt” instruction, which pops the appropriate state back into the processor’s control and data registers,
restores the state to user mode

Exceptions can be divided into four classes

- interrupts
- traps
- faults
- aborts

| Class     | Cause                         | Async/Sync | Return behavior                     |
| --------- | ----------------------------- | ---------- | ----------------------------------- |
| Interrupt | Signal from I/O device        | Async      | Always returns to next instruction  |
| Trap      | Intentional exception         | Sync       | Always returns to next instruction  |
| Fault     | Potentially recoverable error | Sync       | Might return to current instruction |
| Abort     | Nonrecoverable error          | Sync       | Never returns                       |

### Interrupts

Interrupts accur _asunchronously_ as a result of signals from I/O devices that are external to the processor.

They are asynchronous since they are not caused by the execution of any particular instruction.

Exception handlers for hardware intrerrupts are often called **interrupt handlers**

![anatomy of interrupts](./media/anatomy-of-interrupt.png)

After the current instruction finishes executing, the processor notices that the interrupt pin has gone high, reads the exception number from the system bus,
and then calls the appropriate interrupt handler.

When the handler returns, it returns control to the next instruction (i.e., the instruction that would have followed the current instruction in the control flow had the interrupt not occurred).

The effect is that the program continues executing as though the interrupt had never happened.
The remaining classes of exceptions `traps, faults, and aborts` occur synchronously as a result of executing the current instruction.
We refer to this instruction as the **faulting instruction**.

### Traps and System Calls

Traps are intentional exceptions generated by user programs when they want to interact with the kernel, in our case syscalls.

Traps are intentional exceptions that occur as a result of executing an instruction.
Like interrupt handlers, trap handlers return control to the next instruction.
The most important use of traps is to provide a procedure-like interface between user programs and the kernel known as a system call.

User programs often need to request services from the kernel such as reading a file (read), creating a new process (fork), loading a new program (execve), or terminating the current process (exit).
To allow controlled access to such kernel services, processors provide a special “syscall n” instruction that user programs can execute when they want to request service n.
Executing the syscall instruction causes a trap to an exception handler that decodes the argument and calls the appropriate kernel routine.
From a programmer’s perspective, a system call is identical to a

![trap handling](./media/trap-handling.png)
