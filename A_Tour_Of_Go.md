# A Tour of Go

## Named return value

Go's return values may be named, if so, they are treated as variables defined at the top of the function.

This names should be used to documet the meaning of the return values.

A return statement without arguments returns the named return values.
This is known as a `"naked"` return.

Naked return statements should be used only in short functions, as with the example shown here.
They can harm readability in longer functions.

```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```

## Variables

The var statement declares a list of variables; as in function argument lists, the type is last.

A var statement can be at package or function level. We see both in this example.

```go
package main

import "fmt"

// variables declared at a package level
var c, python, java bool

func main() {
    // variable declared at a function level
	var i int
	fmt.Println(i, c, python, java)
}
```

## Variables With Initializers

A var declaration can include initializers, one per variable.

If an initializer is present, the type can be omitted, the variable will take the type of the initializer.

```go
package main

import "fmt"

// i and j variables of type int
var i, j int = 1, 2

func main() {
    // since here variables have multiple types, type not specified
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```

## Short Variable Declarations

 Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

Outside a function, every statement begins with a keyword (`var`, `func`, and so on) and so the `:=` construct is not available. 

```go
package main

import "fmt"

var h = "hello" // valid
var hh string = "world" // valid
// var hhh := "!" // not valid

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

## Go Basic Types

Go's basic Types are

- bool
- string
- integer 
    - int8
    - int16
    - int32
    - int64
- unsigned integer
    - uint8
    - uint16
    - uint32
    - uint64
- byte: alias for unint8

- rune alias for int32, represent a Unicode code point

- float
    - float32
    - float64

- complex
    - complex32
    - complex64

```go
package main

import (
	"fmt"
	"math/cmplx"
)

// variables declared in one block
var (
	ToBe   bool       = false
	MaxInt uint64     = 1 << 64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)      // Type: bool Value: false
	fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)  // Type: uint64 Value: 18446744073709551615
	fmt.Printf("Type: %T Value: %v\n", z, z)            // Type: complex128 Value: (2+3i)
}
```
The example shows variables of several types, and also that variable declarations may be `factored` into blocks,
as with import statements. 

The `int`, `uint`, and `uintptr` types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems.
When you need an integer value you should use int unless you have a specific reason to use a sized or unsigned integer type.

## Zero Values

Variables declared without an explicit initila value are given their *zero* value.

The zero value is:
- `0` for numeric types
- `false` for the boolean types
- `""` __empty string__ for strings

```go
package main

import "fmt"

func main() {
	var i int
	var f float64
	var b bool
	var s string
	fmt.Printf("%v %v %v %q\n", i, f, b, s) // 0 0 false ""
}
```

## Type Conversions

The expression `T(v)` converts the value `v` to the type `T`.

Unlike in C, In Go assignment between items of different type requires an explicit conversions.

```go
var i int = 42
var f float64 = i // error
```
should be like this

```go
var i int = 42
var f float64 = float64(i)
```

When declaring a variable without specifying an explicit type, either by using `:=` or `var`, 
the variable's type is inferred from the value on the right hand side.

```go
package main

import "fmt"

func main() {
	v := 42 // change me!
	fmt.Printf("v is of type %T\n", v) // v is of type int
}
```

## Constants

Constants are declared like variables, but with the `const` keyword

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the `:=` syntax. 

```go
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "World"
	fmt.Println("Hello", World) // Hello World
	fmt.Println("Happy", Pi, "Day") // Happy 3.14 Day

	const Truth = true
	fmt.Println("Go rules?", Truth) // Go rules? true
}
```

## For

Go has only one looping construct, The `for` loop.

Just a basic `for` loop like in other languages. 

Unlike other languages like **C, Java, or JavaScript** there are no parentheses surrounding the three components of the for statement and the braces { } are always required.

```go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

it's optional to add init and the post statements

```go
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```

C's `while` is spelled `for` in Go.

```go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

Now not necessary to add semicolons.

if the loop condition i't loops forever, so an infinite loop is compactly expressed

## If

Go's if statements are like its for loops;

The expression need not be surrounded by parentheses `( )` but the braces `{ }` are required. 

```go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
```

Variables declared inside an `if` short statement are also available inside any of the else blocks

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
        // v accessible
		return v 
	} else {
        // v accessible
		fmt.Printf("%g >= %g\n", v, lim) 
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

## Exercise: Loops and Functions

Computers typically compute the square root, using the __Newton's Method__

Starting with some guess `z`, we can adjust `z` based on how close z² is to x, producing a better guess.

```go
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
	z := 1.0

	for i := 0; i < int(x/2) && z*z != x; i++ {
		z = z - (((z * z) - x) / (2 * z))
	}
	return z
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(1))
	fmt.Println(Sqrt(25))
	fmt.Println(Sqrt(100))
	fmt.Println(Sqrt(100_000_000))
}
```

Add iteration over i / 2 just to be save that square root would be correct.

## Switch

A `switch` in Go is like the one in other languages, except that Go only runs the selected case, not
all the cases that follow, in effect the `break` statement that is needed at the end of each case in other languages

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

### Switch With no condition

A Switch statement without a condition is the same as `switch true`

The construct can be clean way to write long if-then-else chanins.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

Had a long time to understand This, not used to `switch` like this

## Defer

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately,
but the function call is not executed until the surrounding function returns. 

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

`Hello` would be printed first, then `world`, since the first print statement has the key word `defer`, the print statement not executed 
until the surrounding function returns

A defer statement pushes a function call onto a list. The list of saved calls is executed after the surrounding function returns. Defer is commonly used 
to simplify functions that perform various clean-up actions.

### Stacking Defers

Deferred function calls are pushed onto a stack. When a function returns,
its deferred calls are executed in last-in-first-out order. 

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```
## Panic

**Panic** is a built-in function that stops the ordinary flow of control and begins panicking

When the function `F` calls panic, execution of `F` stops, any deferred functions in `F` are executed normally, and then `F` returns to its caller

## Recover

Recover is a built-in function that regains control of a panicking goroutine.

Recover is only useful inside deferred functions.

During normal execution, a call to `recover` will return nil and have no other effect.

If the current goroutine is panicking, a call to `recover` will capture the value given to panic and resume normal execution.

Each Panic should be Recoverd, if not, The process continues up the stack until all functions in the current goroutine have returned, at which point the program crashes

In summary, the defer statement (with or without panic and recover) provides an unusual and powerful mechanism for control flow.

It can be used to model a number of features implemented by special-purpose structures in other programming languages.

## Pointers

Go has pointers. A pointer holds the memory address of a value.

The type *T is a pointer to a T value. Its zero value is nil.

```go
var p *int
```

The & operator generates a pointer to its operand.

```go
i := 42
p = &i
```

The `*` operator denotes the pointer's underlying value.


```go
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

This is known as *"dereferencing"* or *"indirecting"*

Unlike *C*, *Go* has no pointer arithmetic. 

## Structs

A `struct` is a clollection of fields, a struct can be represented like the following

Struct fields can be accessed using a dot.

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
    v := Vertex{1, 2}
    v.X = 4
	fmt.Println(v.X)
}
```

## Pointers to Struct

Struct fields can be accessed through a struct pointer.

To access the field `X` of a struct when we have the struct pointer p we could write `(*p).X`.

However, that notation is cumbersome, so the language permits us instead to write just `p.X`,
without the explicit dereference. 

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v // p pointer to the struct Vertex
	p.X = 1e9 // accessed to v element from the p pointer, in C we do it as follow p->X
	fmt.Println(v) // {1e9, 2}
}
```

## Struct Literals

```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```

## Arrays

`[n]T` is an array of n element of type T.

The expression `var a [10]int` declares a variable `a` as an array of integers.

```go
package main

import "fmt"

func main() {
	
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	
	fmt.Println(a[0], a[1]) // Hello World
	
	fmt.Println(a) // [Hello World]

	primes := [6]int{2, 3, 5, 7, 11, 13}
	
	fmt.Println(primes) // [2 3 5 7 11 13]
}
```

An array's length is part of its type, so arrays cannot be resized. This seems limiting, but don't worry;
Go provides a convenient way of working with arrays. 

## Slices

An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays. An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array.
In practice, slices are much more common than arrays.  

A slice is formed by specifying two indices, a low and high bound, separated by a colon: 

```go
a[low : high]
```

The type `[]T` is a slice with elements of type `T`. 

```go
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	s := primes[1:4]
	fmt.Println(s)
}
```

### Slices are like references to arrays

A slice does not stores any data, it just describes a section of an underlying array

Changing an element of a slice modifies the corresponding elements of it's underlying array. 

```go
package main

import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]     // [John Paul]
	b := names[1:3]     // [Paul George]
	fmt.Println(a, b)   // [John Paul] [Paul George]

	b[0] = "XXX"
	fmt.Println(a, b)   // [John XXX] [XXX George]
	fmt.Println(names)  // [John XXX George Ringo]
}
```

Other slices that share the same underlying array will se those changes. 

### Slice literals

A slice literal is like an array literal without the length. 

This is an arryay literal:

```go
[3]bool{true, true, false}
```

And this creates the same array as above, then builds a slice that references it.

```go
[]bool{true, true, false}
```

### Slice defaults

When slicing, we might omit the high or low bounds to use their defaults insteda

For Example: 

```go
var a [10]int

//  these slice expressions are equivalent

a[0:10]
a[:10]
a[0:]
a[:]
```

### Slice length and capacity

A Slice has both a length and a capacity. 

The length of a slice is the number of elements it contains.

The capacity of a slice is the number of elements in the underlying array,
counting from the first element in the slice

The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`

### Nil Slice

An empty slice has a value of `nil`

A `nil` slice has a length and capacity of `0` and hase no underlying array

```go
package main

import "fmt"

func main() {

	var s []int
	fmt.Println(s, len(s), cap(s)) // [] 0 0

    // the if statement would be evaluated to true, since the slice is empty
	if s == nil {
		fmt.Println("nil!") // nil! 
	}
}
```

### Creating a slice with make

Slices can be created with the build-in `make` function.

This is how you create dynamically-sized arrays.

__The `make` function allocate a zeroed array and returns a slice that refers to that array, To Specify a capacity, 
pass a third argument to `make`__

```go
a := make([]int, 5) // len(a) = 5, cap(a) = 5
```

```go
a := make([]int, 0, 5) // len(a) = 0, cap(a) = 5
```

```go
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)      // a len=5 cap=5 [0 0 0 0 0]

	b := make([]int, 0, 5)
	printSlice("b", b)      // b len=0 cap=5 []

	c := b[:2]
	printSlice("c", c)      // c len=2 cap=5 [0 0]

	d := c[2:5]
	printSlice("d", d)      // d len=3 cap=3 [0 0 0]
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}

```

### Slices of slices

Slices can contain any type, including other slices. 

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	// Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// The players take turns.
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
```

### Appending to a slice

Go provides a built-in `append` function, The first parameter `s` of `append` is a slice of type `T`.

```go
append(slice []Type, elems ...Type)
```

The resulting value of `append` is a slice containing all the elements of the original slice plus
the provided values.

```go
package main

import (
	"fmt"
)

func hello(sum int) int {
	x := sum + 1
	return x
}

func main() {
	s := make([]int, 4)
	s = append(v, 1, 2, 3)

	fmt.Println(s) // [0 0 0 0 1 2 3]
}
```

if the array of `s` is too small to tir all the given values a bigger array will be allocated, 
The return slice will point to the newly allocated array.

## Range

The `range` from of the `for` loop iterates over a slice or map.

When rangin over a slice, two values are returned for each iteration, 
- The first is the index.
- The second is a copy of the element at that index.

Consider this example

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    // i is current element index
    // v is a copy of the element at that index.
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

### Range continued
