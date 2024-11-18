### Dictionary
- package
- executable binary
```
Package called **main** is used to create executable binary. Program execution starts in package **main** by calling its function which also called **main**.
```
- runtime

### Why `go`?
* Why go?
* How it compares to Elixir?
* Can I write functional programming in `go`?
* Is there pattern matching in `go`?
* Uses cases where to use `go`?
* Use cases where to avoid using `go`?
* Concurrency model in `go`?

```
option type / result type would be nice
	How to avoid/combat `nil` values?
syntax to handle errors (?) - how to handle errors in go?

interfaces are always pointers?

OCAML type system?
	Doesnt tie LIFETIME to TYPES
		 WAT

What are generics?
Generics in GO

AI with `golang`?
What is ADT (Abstract Data Type)?

Dont Repeat Yourself
	is it really useful?
	 premature abstraction

Event Driven Approach to Golang

1/ Statically typed language
var myVar string
var myVar = "str" // infered

2/ Stronly Typed Language
Operation I can perform depends on its type

1 + "1" // javascript vs go

3/ Compiled 
	Machine Code \o/
	not interpreted
 
4/ Fast Compile Time
5/ Built in Concurency


Structure
	Module 
		Packages

	MODULE
		dir1/          <- package
			file11.go
			file12.go
		dir2/          <- package
			file21.go
			file22.go
			file23.go
		dir3/          <- package
			file31.go

go mod init NAME // initialize new module
vim go.mod

// cmd/tutorial_1/main.go
package main
func main() {}

go run cmd/tutorial_1/main.go

// ---------- ----------
int
int8
int16
int32
int64

uint
uint8
...

float32
float64

var a int32 = 1
var b float32 = 2

a + int32(b)

var str string = "Hello" + " " + "World!"
var str string = `Hello
World!`

len(str) - number of bytes, but string characters can be encoded on multiple ones!
utf8.RuneCountInString(str)
RUNES

var b bool = true

default values 0, "", false

var1 := 1
var1, var2 := 1, 2

const pi float32 = 3.14
```

Directory structure:
https://github.com/vmware-tanzu/velero/blob/main/pkg/util/kube/secrets_test.go

### How to `import` a package?
- With a construct called `import declaration`
```
package main

import(
	"fmt"   # import spec
	 "math"  
	m "math"  
	. "math"  
	_ "math"
)
```

### CSP vs Actor Model


![[Pasted image 20231108222225.png]]

- What is preemptive scheduling?
- What is reduction?

Each scheduler runs a process queue
	-> after certain number of reductions ( 4000? ). process is put to the back of the queue
	-> another process takes its place
 
+ All processes get's the same time
- not as powerful for intensive processes
	- dynamic typing
	- yielding between processes ( what if we did have only one? )
	- NIFS run on DIRTY SCHEDULERS

![[Pasted image 20231108225243.png]]

JVM
* shared heap
	* shared state concurrency - we can modify other threads
	* stop-the-world GC could result in hanging all threads
* Uses operating system threads and relies on OS scheduler
* Performant on heavy, sustained workload



The Actor Model
* named proccesses communicate with each other by message passing
* Cannot refuse to receive messages
* Theoretically can receive unlimited messages
* stores messages in inbox
* sending is non-blocking, receiving is blocking

CSP
- Communicating Sequential Processes
- Process pass messages to each other via channels
- The process are anonymous, while the channels are named
- Blocking and synchronous by default
 
 