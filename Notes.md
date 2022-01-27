# **Go Learning Notes**
<!-- vscode-markdown-toc -->
* 1. [Basics](#Basics)
	* 1.1. [**Imports**](#Imports)
	* 1.2. [Functions](#Functions)
		* 1.2.1. [Omit Type](#OmitType)
		* 1.2.2. [Multiple Returns](#MultipleReturns)
		* 1.2.3. [Named return values](#Namedreturnvalues)
	* 1.3. [Variables](#Variables)
		* 1.3.1. [Variables with Initializers](#VariableswithInitializers)
		* 1.3.2. [Short Variable Declarations](#ShortVariableDeclarations)
	* 1.4. [Type Conversions](#TypeConversions)
	* 1.5. [Flow Control Statements: for, if, else, switch and defer](#FlowControlStatements:forifelseswitchanddefer)
		* 1.5.1. [For is Go's "While"](#ForisGosWhile)
		* 1.5.2. [Switch](#Switch)
		* 1.5.3. [**Defer**](#Defer)
		* 1.5.4. [**Stacking Defers**](#StackingDefers)
	* 1.6. [Structs, Slices and Maps](#StructsSlicesandMaps)
		* 1.6.1. [**Pointer**](#Pointer)
		* 1.6.2. [**Structs**](#Structs)
		* 1.6.3. [**Arrays**](#Arrays)
		* 1.6.4. [**Slices**](#Slices)
		* 1.6.5. [**Range**](#Range)
		* 1.6.6. [**Consice way to create two-dimensional slices**](#Consicewaytocreatetwo-dimensionalslices)
		* 1.6.7. [**Map**](#Map)
		* 1.6.8. [**Mutating Maps**](#MutatingMaps)
		* 1.6.9. [**Function values**](#Functionvalues)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

##  1. <a name='Basics'></a>Basics
###  1.1. <a name='Imports'></a>**Imports**
```go
package main

import (
  "fmt"
  "os"
)

func main() {
  if len(os.Args) != 2 {
    os.Exit(1)
  }
  fmt.Println("It's over", os.Args[1])
}
```
###  1.2. <a name='Functions'></a>Functions
 In this example, add takes two parameters of type *int* and return int type result.
####  1.2.1. <a name='OmitType'></a>Omit Type
 **When two or more consecutive named function parameters share a type, you can omit the type from all but the last**
 ```go
package main
import "fmt"

func add(x, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42,13))
}
 ```
####  1.2.2. <a name='MultipleReturns'></a>Multiple Returns
A function can return any number of results
```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```
####  1.2.3. <a name='Namedreturnvalues'></a>Named return values
Go's return values may be named. If so, they are treated as variables defined at the top of the function (**function level variables**). \
**A return statement without arguments returns the named return values.**
```go
package main

import "fmt"

func split(sum int) (tens, ones int) {
    tens = sum / 10
    ones = sum % 10
    return 
}

func main() {
    split(17)
    fmt.Println(tens, ones)
}
```
###  1.3. <a name='Variables'></a>Variables
####  1.3.1. <a name='VariableswithInitializers'></a>Variables with Initializers
A var declaration can include initializers, one per variable. \
If an initializer is present, the type can be omitted; the variable will take the type of the initializer.
```go
package main

import "fmt"

var i, j int = 1, 2

func main(){
    var c, python, java = true, false, "no!"
    fmt.println(i, j, c, python, java)
}
```
####  1.3.2. <a name='ShortVariableDeclarations'></a>Short Variable Declarations
Inside a function the ":=" short assignment can be used in place of a var declaration with implicit type
**Outside a function, the short assignment is not available**
```go
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```
###  1.4. <a name='TypeConversions'></a>Type Conversions
The expression T(v) convets the value v to the type T
```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

###  1.5. <a name='FlowControlStatements:forifelseswitchanddefer'></a>Flow Control Statements: for, if, else, switch and defer
####  1.5.1. <a name='ForisGosWhile'></a>For is Go's "While"
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
####  1.5.2. <a name='Switch'></a>Switch 
Go's switch is like the one in C, C++, Java, JavaScript, and PHP, except that Go only runs the selected case, not all the cases that follow. In effect, the break statement that is needed at the end of each case in those languages is provided automatically in Go. Another important difference is that Go's switch cases need not be constants, and the values involved need not be integers.
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

####  1.5.3. <a name='Defer'></a>**Defer**
A defer statement defers the execution of a function until the surronding function returns
####  1.5.4. <a name='StackingDefers'></a>**Stacking Defers**
Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order


###  1.6. <a name='StructsSlicesandMaps'></a>Structs, Slices and Maps
####  1.6.1. <a name='Pointer'></a>**Pointer**
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

The * operator denotes the pointer's underlying value.
```go
fmt.Println(*p) // read i through the pointer p \
*p = 21         // set i through the pointer p \
```
 This is known as "dereferencing" or "indirecting". 
**Unlike C, Go has no pointer arithmetic.**

####  1.6.2. <a name='Structs'></a>**Structs**
A struct is a collection of fields.
Struct fields are accessed using a dot.
```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := vertex{1, 2}
	v.X = 4
}
```

Struct fields can be accessed through a struct pointer

To access the field X of a struct when we have the struct pointer p we could write (*p).X. However, that notation is cumbersome, so the language permits us instead to write just p.X, without the explicit dereference.

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}

```
Struct Literals

A struct literal denotes a newly allocated struct value by listing the values of its fields
You can list just a subset of fields by using the **"Name : "** syntax
```go
var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```
####  1.6.3. <a name='Arrays'></a>**Arrays**
The type [n]T is an array of n values of type
```go
var a[10]int
var b[2]sting
```

####  1.6.4. <a name='Slices'></a>**Slices**
 An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

 The type []T is a slice with elements of type T

 ```go
 a[low : high]
 ```
This selects a half-open range which includes the first element, but excludes the last one.
The following expression creates a slice which includes elements 1 through 3 of a:
```go
a[1:4]
```

**Slices are like references to arrays**. A slice does not store any data, it is just a flexible view, changing the elements of a slice modifies the corresponding elements of its underlying array
```go
func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}

// expected results
[John Paul George Ringo]
[John Paul] [Paul George]
[John XXX] [XXX George]
[John XXX George Ringo]
```

**Slice Literals**
A slice literal is like an array literal without the length.

This is an array literal:

```go
[3]bool{true, true, false}
```
**And this creates the same array as above, then builds a slice that references it:**
```go
[]bool{true, true, false}
```

Common usages:
```go
func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}
```
```
// expected results
[2 3 5 7 11 13]
[true false true true false true]
[{2 true} {3 false} {5 true} {7 true} {11 false} {13 true}]
```                      
**Slice defaults**\
When slicing, you may omit the high or low bounds to use their defaults instead.

The default is zero for the low bound and the length of the slice for the high bound.

these slice expressions are equivalent:
```go
a[0:10]
a[:10]
a[0:]
a[:]
```
**Slice length and capacity**

Length of a slice is the number of elements it contains
The capacity of a slice is the number of elements in the underlying array, **counting from the first element in the slice. In another word, it is how many elements a slice can extend to the end of the array, starting from slice's first element**

```go
func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```
```
// expeceted results:
len=6 cap=6 [2 3 5 7 11 13]
len=0 cap=6 []
len=4 cap=6 [2 3 5 7]
len=2 cap=4 [5 7]
```
**Nil slices**

The zero value of a slice is nil.

A nil slice has a length and capacity of 0 and has no underlying array.

**Creating a slice with make**
Slices can be created with the built-in make function; this is how you create dynamically-sized arrays.

The make function allocates a zeroed array and returns a slice that refers to that array:
```go
a := make([]int, 5)  // len(a)=5
```
To specify a capacity, pass a third argument to make:
```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

**Slices of Slices**
multi-dimension slices. 
```go
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

**Appending to a slice**

It is common to append new elements to a slice, and so Go provides a built-in append function. The documentation of the built-in package describes append.
```go
func append(s []T, vs ...T) []T
```
The first parameter s of append is a slice of type T, and the rest are T values to append to the slice.

The resulting value of append is a slice containing all the elements of the original slice plus the provided values.

If the backing array of s is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

```
func main() {
	var s []int
	printSlice(s)

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s)

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s)

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```
```
// Expected results
len=0 cap=0 []
len=1 cap=1 [0]
len=2 cap=2 [0 1]
len=5 cap=6 [0 1 2 3 4]
```

####  1.6.5. <a name='Range'></a>**Range**
The range form of the for loop iterates over a slice or map
When ranging over a slice, **two values** are returned for each iteration. The first is the index, and the second is a copy of the element at that index
```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main(){
	for i, v := range pow {
		fmt.Printf("2**%d=%d\n", i, v)
	}
}
```
You can skip the index or value by assigning to _ .
```go
for i, _ := range pow
for _, value := range pow
```
If you only want the index, you can omit the second variable
```go
for i := range pow
```

####  1.6.6. <a name='Consicewaytocreatetwo-dimensionalslices'></a>**Consice way to create two-dimensional slices**
```go
func Pic(dx, dy int) [][]uint8 {
	image := make([][]uint8, dy)
	for i := range image {
		image[i] = make([]uint8, dx)
		for j := range image[i] {
			image[i][j] = uint8((i + j) / 2)
		}
	}
	return image
}
```

####  1.6.7. <a name='Map'></a>**Map**
A map maps keys to values, the zero value of a map is ```nil```
The ```make``` function retruns a map of the given type, initialized and ready for use.\
Syntax: map[```KeyType```]```ValueType```
```go
func main(){
	m := make(map[string]Vertex)
	m["Bell Labs"] = Vertex{40,68433, -74.39967}
	fmt.Println(m["Bell Labs"])
}
```
**Map literals are like struct literals, but the keys are required.
```go
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```
If the top-level type is just a type name, you can omit it from the elements of the literal
```go
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```
####  1.6.8. <a name='MutatingMaps'></a>**Mutating Maps**
```
// Insert or Update in map m:
m[key] = elem
// Retrieve an element
elem = m[key]
// Test that a key is present with a two-value assignment
// If key is in m, ok is true.
elem, ok = m[key]
```
 if ```elem``` or ```ok``` have not yet been declared you could use a short declaration form:
```
elem, ok := m[key]
```

####  1.6.9. <a name='Functionvalues'></a>**Function values**

Functions are values too. They can be passed around just like other values
Function values mayu be used as function arguments and return values




