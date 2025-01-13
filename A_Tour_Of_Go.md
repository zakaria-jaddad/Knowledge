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
