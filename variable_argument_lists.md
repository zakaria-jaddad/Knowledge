# Variable Argument Lists

## The va_list Type

The `va_list` type is an array containing a single element of one structure
containing the necessary information to implement the `va_arg` macro.

In other C99 Manual, `va_arg` is an object type suitable for holding
information needed by `va_arg`, `va_start`, `va_copy` and `va_end` macros.

if access to the varying argument of the function is desired.

The C definition of `va_list` type is given in This example.

```c
typedef struct {
    unsigned int gp_offset;     // 4 bytes
    unsigned int fp_offset;     // 4 bytes
    void    *overflow_arg_area; // 8 bytes
    void    *reg_save_area;     // 8 bytes
} va_list[1];                   // size of this stucrt is 24 bytes.
```

## The va_start

The `va_start` macro shall be invoked before any access to unnamed argument

The `va_start` initializes the structure as follows:

- **reg_save_area**: The element points to the start of the register save area.
  reg_save_area saves the original state/value of the registers that have been used.

- **overflow_arg_erea**: This pointer is used to fetch arguments passed on the stack
  it is initialized with the address of the first argument passed on the stack,
  if any and then always update the point to the start of the next argument on the stack

- **gp_offset**: the element holds the offset in bytes from _reg_save_area_ to the place where the next available
  general purpose argument register is saved, in case all argument registers have been used, it's set to the value
  `48`, since there are 6 registers each register with the size of `8 bytes`

- **fp_offset**: the element holds the offset in bytes from _reg_save_area_ to the place where the next available
  floating point argument register is saved.
  In case all argument registers have been used. it's set to the value `304` `(6 * 8 + 16 * 16)`.

## WTF is a register

Registers are a type of computer memory built directly into the processor or CPU that is used to store
and manipulate data during the execution of instructions.

A register may hold an instruction, a storage address, or any kind of data.

Registers are performed based on three operations:

- Fetch is used to retrieve user-provided instructions that have been stored in the main memory.
  Registers are used to fetch these instructions.

- Decod is used to interpret the instruction.

- Execute operations.

### Types of Registers

- Status and control registers.
- General-purpose data registers
- Special purpose register.

### Size of registers

Now a days most computer support 64 bit processors, so registers can store 64 bits of data (8 bytes).

### General-purpose register

General purpose registers might be used with any instructions doing computation that's why, their called like that.

### register save area

register save area provides the space that is needed to save used registers state or value.

### floating point register

Floating point register is a special type of register used for storing floating-point numbers in a binary format.

## The va_arg

The `va_arg` macro expands to an expression that has the specified type and the value of the next argument in the call,
the parameter ap must have been initialized by the `va_start` of `va_vopy`.

Each call the `va-arg` modifies `ap` so that the value of successive argument are returned in turn,
`va_arg` returns the value of the requested type based on the giveen type and set ap to point to the next argument.

The `va_arg` macro access the next variadic function unnamed argument

## The va_start

The `va_start` macros shall be invoked before any access to unnamed arguments.
The unnamed parameter is an identifier ro the rest of the parameters (arguments) it's the one just before the `(...)`
in the function declaration.

## The va_end

The `va_end` macro facilitates a normal return from the function whose variable argument list was referred to by
`va_start` macro or some other `va` macros

The `va_end` macro may modify `ap` so that it's no longer usable, but as so far `va_end` does nothing

If `va_end` is no invoked before the return. the behavior is undefined, the `va_end` return no value.
