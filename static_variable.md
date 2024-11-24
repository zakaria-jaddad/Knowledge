# Static variable

## What is Static variable

a static variable is a variable that has been allocated `statically`,
meaning that it's lifetime is the entire run of the program.

`global` and `local` refer to scope, not lifetime.

In many languages `global variable` are always static, but in some languages they are dynamic.

`static memory allocation` is the allocatiobn of memory at `compile time`,
before the associated program is executed.

Unlike `dynamic memory allocation` or `automatic memory allocation` where memory is allocated as
required at `run time`.

As far as i know a `static variable` is allocated at the compile time even if you use recursive call's still new value.
might be wrong

## Addressing

The absolute address addressing mode only can be used with static variables,
thought those are the only kinds of variables whose location is know by the `compiler` at compile time.

When the program or a library is loaded into memory `static variables` are stored in
`data segment` of the program address space (if initialized) or the `BSS segment` (if uninitialized).

And are stored in corresponding sections of `object files` priop to loading.

### What is the absolute address addressing mode

addressing modes define how the machine languages instructions in that
architecture identify the operands (object of an oberation) of each instruction.

An addressing mode specifies how to calculate the effective `memory address` of an operand by using information
held in `registers` or constants contained within machine instruction or elsewher.

### What is a data segment of the program

a data segment is a portion of an object file or the corresponding `address space` of a program contains initialized
static variable, That is both `global variables` and `static local variables`.

The size of the this segment is determined by the size of the value in the programs's source code,
and _does not change at run time_

This is in contrast to the read-only `data segment` which contains static constants rather than variables.

it also contrast to the `code segment` also known as the text segment, which is read-only on many architectures.

Uninitialized data, both variables and constants is instead in the `BSS segment .bss`.

`BSS segment`: _block starting symbol_ is the portion of an `object file` executable or assembly language code that contains
`statically allocated variables` that are declared but have not been assigned a value yet.
it is often referred to as the _bss serction_ or _bss segment_.

## Scope

A static local variable is different form local variable as `static local variable` is initialized only once
no matter how many times the function in which it resides is called and it's value is retained and accessible
through many calls to the function in which it is declared. _what i did when i used it in recursive calls_

### What is `internal linkage` in `C`

The scope of a global variable can be restricted to the file containing it's declaration by prefixing the declaration
by prefixing the declaration with the keyword `static`, Such variables are said to have internal linkage.

### What is `external linkage` in `C`

A global variable has external linkage by default. it's scope can be extended to files other than containing it by giving
a matching `extern` declaration in the other file.

```c
void f(int i);
extern const int max = 10;
int n = 0;
int main()
{
    int a;
    //...
    f(a);
    //...
    f(a);
    //...
}
```

1. the signature of the function `f` declares `f` as a function with _external linkage_ which is the default.
   it's definition must be provided later in the file or in other `translation unit`.

2. max is defined as an integer constant. The default linkage for constants is _internal_.
   it's linkage is changeged to external with the keyword `external`. So now `max` can be accessed in other files.

3. n is defined as integer variable. The default linkage for variables defined outside function bodies is external.

## Example

_An Example of static local variable in C_

```c
void my_function()
{
    // x is initialized only once across five calls of the function.
    // x will get incremented each call of the function.
    // the final value of x will be 5
    static int x = 0;

    x++;
    printf("%d\n", x);
}



int main()
{

    static hello = 1;

    my_function(); // prints 1
    my_function(); // prints 2
    my_function(); // prints 3
    my_function(); // prints 4
    my_function(); // prints 5
    if (hello == 250)
        return;
    hello++;
    main();

    return 0;
}
```
