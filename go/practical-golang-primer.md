---
quality: low
author: 
date: 2018-08-18
draft: true
title: "Practical Golang Primer"
description: Golang basics 
tags: ['golang', 'language basics']
categories: ['golang']
toc: true

---

## Documentation

https://golang.org/doc/

https://golang.org/pkg/builtin

## Language Features

- go is a compiled language. It is based on C language families. 
- go tool can run without pre-compiling 
- compiling produces OS specific executables
- statically typed 
- object oriented with interfaces
- supports polymorphism with interfaces
- no type inheritance 
- no method or operator overloading
- no structured exception handling
- no implicit numeric conversions, type conversions are specific
- designed based on C, C++, C#, Pascal/Modula/Oberon

## Syntax

* case sensitive
* variables are lower and mixed case.
* exported functions and fields have an initial upper case character. 
* semicolons are not needed (usually) - the lexer will add them. 
* code blocks are wrapped with braces. 

## Go commands

```go
go 
go run file.go //run without building
go build // build binary/executable
go install // build executable and place in $GOPATH/bin

godoc fmt //documentation for fmt
godoc fmt.Println //documenation for fmt.Println
go help test //documentation for tesing

gofmt file.go // format code
```

## Project structure

* package names are all lower case and a single word

>A typical workspace contains many source repositories containing many
>packages and commands. Most Go programmers keep *all* their Go source code
>and dependencies in a single workspace.  
>
><cite>[golang.org/doc/code.html](golang.org/doc/code.html)</cite>

```bash
# src contains Go source files
# pkg contains package objects
# bin contains executable commands

cd $HOME/go # $HOME/go is the default on Unix
mkdir src bin pkg
set GOPATH=$PWD
export GOPATH=$(go env GOPATH)
export PATH=$PATH:$(go env GOPATH)/bin # Add the bin folder to you path so you can run your programs 

cd src
mkdir -p github.com/username
cd github.com/username
mkdir -p mypackage
cd mypackage
touch mypackage.go 
```

## Import Declarations

```go
package main   

import (
	"fmt"
	. "fmt" # import without needing to use 'fmt' namespace 
	_ "fmt" # import without using the package, it will intitialize it.
	f "fmt" # import but with your chosen namespace
) 
```

## File Structure

- package declaration
- import
- main function
- other functions

```go

// 	first statement must be package name
// 	executable(program) commands must always use package main
// 	other packages follow a convention
//	* match filename - last part of import path
//	* single word
//	* lowercase

package main   

// next import 
import (
	"fmt"
	"strings"
) 

// then the main function
func main() {
    fmt.Println("Hello from Go!");
    fmt.Println(strings.ToUpper("Hello from Go!"));
}          
```

## Variable types 

https://golang.org/pkg/go/types/#BasicKind

> The int, uint, and uintptr types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems. When you need an integer value you should use int unless you have a specific reason to use a sized or unsigned integer type. 
> https://tour.golang.org/basics/11


```go
// Explicit typing
var myInteger int = 43;
var myString string = "This is a string";

// Implicit typing, the variable type is inferred at runtime. 
var myInteger := 43;
var myString := "This is a string";

// constants are similar 
// Constants can be character, string, boolean, or numeric values.
// explicit constant
const myInt int = 43;
// implicit constant
const myInt = 43;

var var1, var2, var3 = 1, 2, 3 // multiple variables declaration and assignment, the type can be omitted here.
```

Variables defined without a value are given the zero value of their type. 0 for numbers, false for boolean, and "" for empty strings.

https://tour.golang.org/basics/13

https://tour.golang.org/basics/14

## If/Else

```go
if x < 0 {

} else { // else has to be on this line

}

// these variables are local to the if statement
if x:= 42; x < 0 {

} else {

}
```

## Switch Statements

https://tour.golang.org/flowcontrol/10

Go breaks in switch statements by default, but you can add fall through if you want that behavior. 

```go
cookie := "Chocolate Chip"
switch cookie {
case "Oatmeal":
    fmt.Println("A good choice.")
case "Chocolate Chip":
    fmt.Println("The very best.")
}
```

## Loops

```go
sum := 0
for i := 0; i < 10; i++ {
    sum += i
}
fmt.Println(sum)

aa := [4]int{1, 2, 3, 4}
for _, a := range aa {
    fmt.Println(a)
}

```

## Strings

https://blog.golang.org/strings

A string is an array of bites or a single scalar value.

```go
package main

import (
	"fmt"
	"strings"
)
str1 := "implicit string"

strings.ToUpper
strings.Title
string.EqualFold("str", "STR") // returns true/false
strings.Contains("string", "search")
```

## Numbers

Floating point numbers are represented in binary in go. 

```Go
package main

import (
	"math/big"
	"fmt"
);

n1, n2, n3 := 12, 13, 23
nSum := n1 + n2 + n3
fmt.Println("Integer sum:", nSum);

// This will not work
// f1, f2, f3 := 12.5, 32.1 12.4
// fSum := f1 + f2 + f3
// instead add floats like this
var b1, b2, b3 bSum big.float
b1.SetFloat64(23.12)
b2.SetFloat64(32.12)
b3.SetFloat64(21.23)
bSum.Add(&b1, &b2).Add(&bSum, &b3) // & is a pointer or reference
```

## Arrays 

[n]T

Arrays are fixed length.

```go
var a [12]int
var aa [4]int{1, 2, 3, 4} // assign and create

var s [3]string
s[0] = "String1"
s[1] = "String2"
```

## Slices 

In Go an array is fixed in length and must be manually "resized" to add additional elements. Other languages like JS and Python handle this automatically for you in the background. Go provides slices as an abstraction over arrays to accommodate this behavior. Slices may contain any type. 

a[low : high]

>A slice does not store any data, it just describes a section of an underlying array.
>
>Changing the elements of a slice modifies the corresponding elements of its underlying array.
>
>Other slices that share the same underlying array will see those changes.
>
><cite>https://tour.golang.org/moretypes/8<cite>

> Range: When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index .
>
> <cite>https://tour.golang.org/moretypes/16</cite>

```go
package main

import "fmt"

func main() {

	var array = [4]int{1, 2, 3, 4}
	var slice []int = array[0:4]

	slice = append(slice, 5, 6, 7, 8)
	for _, n := range slice {
		fmt.Println(n)
	}
}
```

## Maps

https://blog.golang.org/go-maps-in-action

https://tour.golang.org/moretypes/22

Maps can be created in several ways. `make()` = allocates and initializes memory so values can be added right away. `new()` = allocates but doesn't initialize memory. 

```go
package main

import (
	"fmt"
)

func main() {
	cookie := make(map[string]string)

	cookie["Chocolate Chip"] = "the best"
	cookie["Butter Scotch"] = "a fine choice"

	delete(cookie, "Butter Scotch")

	if c, ok := cookie["Chocolate Chip"]; ok {
		fmt.Printf("Chocolate chip cookie's are %v", c)
	}
}
```

## Structs

Structs provide a way to organize data and create custom types. 

```go
package main

import (
	"fmt"
)

type Chocolate struct {
	origin      string
	weight      int
	ingredients []string
}

func main() {
	milkChocolate := Chocolate{
		origin:      "Pennsylvania",
		weight:      1,
		ingredients: []string{"milk", "almonds", "chocolate", "sugar"},
	}
	
	fmt.Println(milkChocolate.origin)
}
```

## Interfaces

A simple value is an instance of a type ( int, string,  byte, etc). Every type in go uses the empty interface. Every type in go is an implementation of at least one interface.  There is no type inheritance only type inference or implied inheritance. 

```go
package main

import (
	"fmt"
)

type Duck interface {
	quack() string
	waddle() string
}

type Mallard struct{}

func (m Mallard) quack() string {
	return "quack, quack"
}

func (m Mallard) waddle() string {
	return "waddling,away"
}

func main() {
	duck := Mallard{}
	fmt.Println(duck.quack())
}
```

## Handling Errors 

There is no try/catch. An error is an instance of an interface with an error method that returns a string. 

You can create your own functions with their own custom error objects. You can also just use if/else for error handling. The error type is a built-in interface similar to `fmt.Stringer`

```go
// type error interface {
//     Error() string
// }

package main

import (
	"fmt"
	"os"
)

func main() {
	_, err := os.Open("filename.ext")

	if err != nil {
		fmt.Println("Catch me quick")
	}
}
```

## Defer  

`defer` is similar to finally in other languages. `defer` will defer the function call until everything else is finished. If there are multiple they are handled in LIFO order. 

```go
package main
import "fmt"

func main() {
    defer fmt.Println(1)
    defer fmt.Println(2)
    defer fmt.Println(3)
}
```

## Functions

Functions are defined with the `func` keyword, they can receive and return multiple values. 

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println(divide(7, 8))
}

func divide(a, b int) (int, int) {
	x := a / b
	y := a % b
	return x, y
}
```

## Go routines

Placing `go` before a function creates a go routine. A go routine is a lightweight thread manged by go's runtime.  They are useful for writing concurrent, multi-threaded software applications. When the applications main function finishes the program will exit even if the go routines aren't finished running. 

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	numbers := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
	for i, num := range numbers {
		go sum(i, num)
	}
	time.Sleep(100 * time.Millisecond)
}

func sum(a, b int) {
	fmt.Println(a + b)
}
```

## Channels

https://tour.golang.org/concurrency/2

Channels allow you to send and receive values. Channels are indicated with `<-` , the data flows in the direction of the channel. An unbuffered channel in go blocks until the data has been sent or received. You can create a buffered channel by providing a second argument to `make` when you initialize the channel. The second argument is the size of the buffer. When the buffer is full the channel will block. 

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int)

	go echo(ch, 10)

	for num := range ch {
		fmt.Println(num)
	}

}

func echo(ch chan<- int, num int) {

	for i := 0; i < num; i++ {
		ch <- i
	}
	close(ch)
}
```

> **Note:** Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.
>
> **Another note:** Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a `range` loop.	
>
> <cite>https://tour.golang.org/concurrency/4</cite>

## Select 

> The `select` statement lets a goroutine wait on multiple communication operations.
>
> <cite>https://tour.golang.org/concurrency/5</cite>

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	stream1 := make(chan string)
	stream2 := make(chan string)
	ocean := make(chan string)
	timeout := time.After(1000 * time.Millisecond)

	go func() {
		gurgle := []string{"gurgle", "gurgle", "gurgle"}
		for _, val := range gurgle {
			stream1 <- val
			time.Sleep(100 * time.Millisecond)
		}
	}()

	go func() {
		trickle := []string{"trickle", "trickle", "trickle"}
		for _, val := range trickle {
			stream2 <- val
			time.Sleep(100 * time.Millisecond)
		}
	}()

	go func() {
		for water := range ocean {
			fmt.Println(water)
		}
	}()

	for {
		select {
		case water := <-stream1:
			ocean <- water
		case water := <-stream2:
			ocean <- water
		case <-timeout:
			fmt.Println("done")
			close(ocean)
			return
		}
	}
}
```

## Data Races 

https://tour.golang.org/concurrency/9

A data race is caused when multiple operations are performed simultaneously on the same data. For example, a variable may be read and mutated at the same time. Since go routines provide a mechanism for concurrent code execution, care must be taken to avoid data races. A mutex or a semaphore can be used to   prevent data races. 

## Packages 

Packages provide a way to package and share code.  Lowercase variables and functions are scoped to the package while uppercase variables and functions are exported. 

```go
package mypackage

import "fmt"

func main() {
	fmt.Println("I am a package.")
	local()
}

func local() {
    fmt.Println("I am a local method.")
}

func Export() {
    fmt.Println("I am an exported method.")
}
```

```go
package main

import "mypackage"

main() {
	mypackage.Export()   
}
```

## Testing

Golang provides an excellent set of testing and benchmarking tools. The naming convention for tests is `package_test.go` .

https://blog.golang.org/cover

I found Mike Sickles course on the topic to be quite good. 

https://www.pluralsight.com/courses/go-testing-applications

## Working with files and the Web. 

There are several go web frameworks such a Gorilla, Gin, and Buffalo. But go provides excellent built in support for building web applications, so you may not even need a framework. Here is a code sample demonstrating a simple server and router. 

```go
package main

import (
	"io"
	"net/http"
)

func hello(res http.ResponseWriter, req *http.Request) {
	io.WriteString(res, "Hello my name is Go.")
}

func main() {
	http.HandleFunc("/", hello)
	http.ListenAndServe(":3000", nil)
}
```

If you want to learn web development with go, watch  Todd McLeod's course: https://www.udemy.com/go-programming-language

## References

- https://www.lynda.com/Go-tutorials/Up-Running-Go/412378-2.html
- https://www.udemy.com/go-programming-language/
- https://www.safaribooksonline.com/library/view/ultimate-go-programming/9780134757476/
- https://play.golang.org/
- https://tour.golang.org/
- https://golang.org/doc/#articles
- https://golang.org/doc/effective_go.html
- https://research.swtch.com/gotour
- https://www.youtube.com/watch?v=XCsL89YtqCs
- https://github.com/karlseguin/the-little-go-book
