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
* 2. [Gin Framework](#GinFramework)
	* 2.1. [**Returning Data in Different Formats**](#ReturningDatainDifferentFormats)
	* 2.2. [**Struct-tag-based Validation Feature**](#Struct-tag-basedValidationFeature)
	* 2.3. [**Extending Gin with Middleware**](#ExtendingGinwithMiddleware)
		* 2.3.1. [**Customize Middleware**](#CustomizeMiddleware)
	* 2.4. [**Calling Microservices internally**](#CallingMicroservicesinternally)
	* 2.5. [**K,V in Gin Context**](#KVinGinContext)
		* 2.5.1. [GetString()](#GetString)
* 3. [**Context in Go**](#ContextinGo)
* 4. [**YAML in GO**](#YAMLinGO)
	* 4.1. [**Unmarshal**](#Unmarshal)

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

#### **Type Embedding**
The theory behind embedding is pretty straightforward: by including a type as a nameless parameter within another type, the exported parameters and methods defined on the embedded type are accessible through the embedding type. The compiler decides on this by using a technique called “promotion”: the exported properties and methods of the embedded type are promoted to the embedding type.

```go
type Ball struct {
    Radius   int
    Material string
}

type Football struct {
	Ball
}
```
So now you have embedded Ball into Football. Now, if you have a method defined on the embedded type, Ball:
```go
func (b Ball) Bounce() {
    fmt.Printf("Bouncing ball %+v\n", b)
}
```
You can access the method through the embedding type, Football:
```go 
fb.Bounce()
```
##### Embedding interfaces
If the embedded type implements a particular interface, then that too is accessible through the embedding type. Here is an interface and a function that accepts the interface as parameter:
```go
type Bouncer interface {
    Bounce()
}
func BounceIt(b Bouncer) {
    b.Bounce()
}
```
Now you can call the method using the embedding type:
```go
BounceIt(fb)
```
It is also possible to embed interfaces in structs. You can use this to explicitly state that the embedding struct needs to satisfy the embedded interface and at the same time hide it's data.
```go
type Football struct {
	Bouncer
}
```
NOTE: The embedded struct has no access to the embedding struct. Unlike traditional inheritance, it is not possible for the embedded struct (child) to access anything from the embedding struct (parent.) If you duplicate a property, its value is never accessible "downstream". Consider this adjustment to the Football struct and the Bounce method:
```go
type Football struct {
    Ball
    Radius int
}
func (b Ball) Bounce() {
    fmt.Printf("Radius = %d\n", b.Radius)
}

fb := Football{Ball{Radius: 5, Material: "leather"}, 7}
```
Then these two method calls will both print "Radius = 5"
```go
fb.Bounce()
fb.Ball.Bounce()
```


#### **Receiver**
```go
func (h handler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    ...
}

func (s *GracefulServer) BlockingClose() bool {
    ...
}
```
This is called the 'receiver'. Function Receiver sets a method on variables that we create. In the first case (h handler) it is a value type, in the second (s *GracefulServer) it is a pointer. The way this works in Go may vary a bit from some other languages. The receiving type, however, works more or less like a class in most object-oriented programming. It is the thing you call the method from, much like if I put some method A inside some class Person then I would need an instance of type Person in order to call A (assuming it's an instance method and not static!).

One gotcha here is that the receiver gets pushed onto the call stack like other arguments so if the receiver is a value type, like in the case of handler then you will be working on a copy of the thing you called the method from meaning something like h.Name = "Evan" would not persist after you return to the calling scope. For this reason, anything that expects to change the state of the receiver needs to use a pointer or return the modified value (gives more of an immutable type paradigm if you're looking for that).

##  2. <a name='GinFramework'></a>Gin Framework


**Highlighted features of Gin**\
Gin is a fully-featured, high-performance HTTP web framework for the Go ecosystem. It’s becoming more popular every day among Gophers (Go developers) due to the following features.

**Performance**\
Gin comes with a very fast and lightweight Go HTTP routing library (see the detailed benchmark). It uses a custom version of the lightweight HttpRouter routing library, which uses a fast, Radix tree-based routing algorithm.

###  2.1. <a name='ReturningDatainDifferentFormats'></a>**Returning Data in Different Formats**
Specifiy different returned field names according to the format
```go
type album struct {
	ID string `json:"id" xml:"Id"`
	Title string `json:"title" xml:"Title"`
	Artist string `json:"artist" xml:"Artist"`
	Price float64 `json:"price" xml:"Price"`
}
```
```xml
//xml return
<album><Id>1</Id><Title>Blue Train</Title><Artist>John Coltrane</Artist><Price>56.99</Price></album><album><Id>2</Id><Title>Jeru</Title><Artist>Gerry Mulligan</Artist><Price>17.99</Price></album><album><Id>3</Id><Title>Sarah Vaughan and Clifford Brown</Title><Artist>Sarah Vaughan</Artist><Price>39.99</Price></album>
```
```Indented JSON
//Indented JSON return
[
    {
        "id": "1",
        "title": "Blue Train",
        "artist": "John Coltrane",
        "price": 56.99
    },
    {
        "id": "2",
        "title": "Jeru",
        "artist": "Gerry Mulligan",
        "price": 17.99
    },
    {
        "id": "3",
        "title": "Sarah Vaughan and Clifford Brown",
        "artist": "Sarah Vaughan",
        "price": 39.99
    }
]
```

###  2.2. <a name='Struct-tag-basedValidationFeature'></a>**Struct-tag-based Validation Feature**
```go
type PrintJob struct {
    JobId int `json:"jobId" binding:"required,gte=10000"`
    Pages int `json:"pages" binding:"required,gte=1,lte=100"`
}
```
We need to use the binding struct tag to define our validation rules inside the PrintJob struct. Gin uses go-playground/validator for the internal binding validator implementation. The above validation definition accepts inputs based on the following rules:

- ```JobId: Required, x ≥ 10000```
- ```Pages: Required, 100 ≥ x ≥ 1```



###  2.3. <a name='ExtendingGinwithMiddleware'></a>**Extending Gin with Middleware**

Gin’s middleware system lets developers modify HTTP messages and perform common actions without writing repetitive code inside endpoint handlers. When you create a new Gin router instance with the ```gin.Default()``` function, it attaches logging and recovery middleware automatically.

For example, you can enable CORS in microservices with the following code snippet:
```go
package main
import (
    "github.com/gin-gonic/gin"
    "github.com/gin-contrib/cors"
)

func main() {
    router := gin.Default()
    router.Use(cors.Default())
    router.GET("/", func(c *gin.Context) {
        c.JSON(200, gin.H{"message": "CORS works!"})
    })
    router.Run(":5000")
}
```

####  2.3.1. <a name='CustomizeMiddleware'></a>**Customize Middleware**
It’s possible to build your own middleware with Gin’s middleware API, too. For example, the following custom middleware intercepts and prints (logs to the console) the User-Agent header’s value for each HTTP request. (Similar to "@AROUND" in Java Spring AOP)

Logger middleware will write the logs to gin.DefaultWriter even if you set with GIN_MODE=release. By default gin.DefaultWriter = os.Stdout

```go
func FindUserAgent() gin.HandlerFunc {
    return func(c *gin.Context) {
        log.Println(c.GetHeader("User-Agent"))
        // Before calling handler
        c.Next() // continue the request handling 
        // After calling handler
    }
}
func main() {
    router := gin.Default()
    router.Use(FindUserAgent())
    router.GET("/", func(c *gin.Context) {
        c.JSON(200, gin.H{"message": "Middleware works!"})
    })
    router.Run(":5000")
}
```

**Use Group() to group apis serve the same aspect together**
```go
// router for test
	test := router.Group("/api/v1/test/")
	{
		test.GET("ping", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{
				"msg":    "pong",
				"status": 0,
			})
		})
		test.GET("param/:id", handler.Param) 
		test.GET("param2", handler.Param2)   
	}
```

### **Gin Recovery Middleware**
Recovery Middleware recovers from any panics and writes a 500 if there was one.
```go
router.Use(gin.Recovery())
```

###  2.4. <a name='CallingMicroservicesinternally'></a>**Calling Microservices internally** 
**Resty** Use ```go-resty``` to call microservices.
```go
package main
import (
    "math/rand"
    "time"
    "log"
    "github.com/gin-gonic/gin"
    "github.com/go-resty/resty/v2"
)

type Invoice struct {
    InvoiceId int `json:"invoiceId"`
    CustomerId int `json:"customerId" binding:"required,gte=0"`
    Price int `json:"price" binding:"required,gte=0"`
    Description string `json:"description" binding:"required"`
}
type PrintJob struct {
    JobId int `json:"jobId"`
    InvoiceId int `json:"invoiceId"`
    Format string `json:"format"`
}
func createPrintJob(invoiceId int) {
    client := resty.New()
    var p PrintJob
    // Call PrinterService via RESTful interface
    _, err := client.R().
        SetBody(PrintJob{Format: "A4", InvoiceId: invoiceId}).
        SetResult(&p).
        Post("http://localhost:5000/print-jobs")

    if err != nil {
        log.Println("InvoiceGenerator: unable to connect PrinterService")
        return
    }
    log.Printf("InvoiceGenerator: created print job #%v via PrinterService", p.JobId)
}
func main() {
    router := gin.Default()
    router.POST("/invoices", func(c *gin.Context) {
        var iv Invoice
        if err := c.ShouldBindJSON(&iv); err != nil {
            c.JSON(400, gin.H{"error": "Invalid input!"})
            return
        }
        log.Println("InvoiceGenerator: creating new invoice...")
        rand.Seed(time.Now().UnixNano())
        iv.InvoiceId = rand.Intn(1000)
        log.Printf("InvoiceGenerator: created invoice #%v", iv.InvoiceId)

        createPrintJob(iv.InvoiceId) // Ask PrinterService to create a print job
        c.JSON(200, iv)
    })
    router.Run(":6000")
}
```

###  2.5. <a name='KVinGinContext'></a>**K,V in Gin Context**
####  2.5.1. <a name='GetString'></a>GetString()
```go
return c.GetString(TraceIDKey)
```
```go
// GetString returns the value associated with the key as a string.
func (c *Context) GetString(key string) (s string) {
	if val, ok := c.Get(key); ok && val != nil {
		s, _ = val.(string)
	}
	return
}
```

### **Log Response Body in Gin**

You need to intercept writing of response and store it somewhere first. Then you can log it. And to do that you need to implement your own Writer intercepting Write() calls.

```go
type bodyLogWriter struct {
    gin.ResponseWriter
    body *bytes.Buffer
}

func (w bodyLogWriter) Write(b []byte) (int, error) {
    w.body.Write(b)
    return w.ResponseWriter.Write(b)
}

func ginBodyLogMiddleware(c *gin.Context) {
    blw := &bodyLogWriter{body: bytes.NewBufferString(""), ResponseWriter: c.Writer}
    c.Writer = blw
    c.Next()
    statusCode := c.Writer.Status()
    if statusCode >= 400 {
        //ok this is an request with error, let's make a record for it
        // now print body (or log in your preferred way)
        fmt.Println("Response body: " + blw.body.String())
    }
}
```

##  3. <a name='ContextinGo'></a>**Context in Go**
Incoming requests to a server should create a Context, and outgoing calls to servers should accept a Context. The chain of function calls between them just propagate the Context, optionally replacing it with a derived Context created using WithCancel, WithDeadline, WithTimeOut, or WithValue. **When a Context is canceled, all Contexts derived from it are also canceled.** 

The WithCancel, WithDeadline, WithTimeOut functions take a Context (the parent) and returns a derived Context (the child) and a CancelFunc. Calling the CancelFunc cancels the child and its children, removes the parent's reference to the child, and stops any associated timers. Failing to call the CancelFunc leaks the child and its children until the parent is canceled or the timer fires. The govet tool checks that CancelFuncs are used on all control-flow paths.

Programs that use Contexts should follow these rules to keep interfaces consistent across packages and enable static analysis tools to check context propagation:

Do not store Contexts inside a struct type; instead, pass a Context explicitly to each function that needs it. The Context should be the first parameter, typically named ctx:
```go
func DoSomething(ctx context.Context, arg Arg) error {
	// ... use ctx ...
}
```
Do not pass a nil Context, even if a function permits it. Pass context.TODO if you are unsure about which Context to use.

Use context Values only for request-scoped data that transits processes and APIs, not for passing optional parameters to functions.

The same Context may be passed to functions running in different goroutines; **Contexts are safe for simultaneous use by multiple goroutines.**

##  4. <a name='YAMLinGO'></a>**YAML in GO**

###  4.1. <a name='Unmarshal'></a>**Unmarshal**
```go
func Unmarshal(in []byte, out interface{}) (err error)
````
Unmarshal decodes the first document found within the ```in``` byte slice and assigns decoded values into the ```out``` value.\
**Maps and pointers (to a struct, string, int, etc) are accepted as out values** If an internal pointer within a struct is not initialized, the yaml package will initialize it if necessary for unmarshalling the provided data. The out parameter mast not be nil.\
The type of the decoded values should be compatible with the respective values in out. If one or more values cannot be decoded due to a type mismatches, decoding continues partially untiil the end of the YAML content, and a *yaml.TypeError is returned with details for all missed values.

Struct fields are only unmarshalled if they are exported (have an upper case first letter), and are unmarshalled using the field name lowercassed as the default key. Custom keys may be defined via the "yaml" name in the field tag: the content proceding the first comma is used as the key, and the following comma-seperated options are used to tweak the marshalling process (see Marshal).
Conflicting names result in a runtime error. 

For Example:
```go
type T struct {
	F int `yaml:"a,omitempty"`
	B int
}
var t T
yaml.Unmarshal([]byte("a: 1\nb: 2"), &t)
```

