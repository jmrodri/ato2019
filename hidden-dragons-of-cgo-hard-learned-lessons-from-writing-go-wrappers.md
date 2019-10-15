# Hidden Dragons of CGO
#### K.S. Bhaskar, YottaDB

## Outline

* Motification & background - YottaDB, C API, Go, CGO
* Parameter Passing
* Callbacks
* Garbage Collection (common thread)
* Questions & Answers

## YottaDB - https://yottadb.com (booth 59)
* A mature, high performance,
* rock solid ...

## Key-Value Tuples
```
["Capital","Belgium", "Brussels"]
["Capital","Thailand", "Bangkok"]
```

## Schemaless
```
["Capital","Belgium", "Brussels"]
["Capital","Thailand", "Bangkok"]
["Population","Belgium", 13670000]
["Population","Thailand", 84140000]
```

## Mix Key Sizes

## Keys <---> Array References

Sharing & Persistence - Database Access
If variable starts with ^ it is persisted, otherwise it's private to the
process.

## C
* low-level programming language developed in 1972 by Dennis Ritchie
* Lingua Franca in copmuting
* static typing ....

## C API
In the documentation there is a fairly complete C API to access YottaDB.

## Golang
* definition of Go
* Programming language from Google
 - static typing
 - cooperative multithreading
 - mix of C, erlang, and "java"

## Go API
Yotta wrote the documentation first then wrote the API. The Go matches the C
API but uses the thread-safe only.

## How about a Go API to YottaDB?
* Everything we do is 100% free / open source
* We make money from spuport contracts and funded enhancements
* What about a simpler than a Go wrapper to the C API as a funded project

Yes, indeed what could be simpler? :)
a lot more painful and took much more work than we anticipated.

## CGO
* https://golang.org/cmd/cgo/
 - A lot of code is written in C, calling into that code opens a whole new world
   of possibilities
 - C is not garbage collected; every c program is responsible for its own memory
   allocations and frees; CGO attempts to work around this by creating rules
 - C is the best assembly language :)

## Passing Go Pointers to C
- https://golang.org/cmd/cgo/#hdr-Passing_pointers

## Integers are OK to pass

## C char is an Integer

## C Strings are NOT Go Strings
* Compilation error - a Go string is a descrptor unlike a C string.

```
// void myFunc(char *c1) {
//}
```

## Passing a Reference to 1st Element ...1
```
// void myFunc(void *c)

msg := ([]byte)("world!")
C.myFunc(unsafe.Pointer(&msg[0])
```

## Passing a Reference to the 1st Element ...2
Create a structure, and pass the reference to the struct element

## Cannot Pass a Structure with a Pointer ...1
```
// myFunc(mystruct *strct)
```

missed what he said

## Cannot Pass a Structure with a Pointer ...2
```
// myFunc(mystruct *strct)
```

## Go Structures with Pointers to C structs ..1
```
// myFunc(myStruct *strct)

msg := C.myStruct{C.Cstring("world")}
C.myFunc(&msg)
```

* this leaks memory ^^^

## Go Structures with Pointers to C structs ..2
```
// myFunc(myStruct *strct)
msg := C.myStruct{C.Cstring("world")}
defer C.free(msg.msg)
C.myFunc(&msg)
```

## Go Structures with Pointers to C structs ..3
* finalizers

```
// myFunc(myStruct *strct)
msg := C.myStruct{C.Cstring("world")}
runtime.SetFinalizer(&msg, func(t *C.myStruct) {
   C.free(unsufe.Pointer(t.msg))
   })
C.myFunc(&msg)
```

* this segfaults

## Go Structures with Pointers to C structs 
* KeepAlive

```
// myFunc(myStruct *strct)
msg := C.myStruct{C.Cstring("world")}
runtime.SetFinalizer(&msg, func(t *C.myStruct) {
   C.free(unsufe.Pointer(t.msg))
   })
C.myFunc(&msg)
runtime.KeepAlive(&msg) this prevents the Go GC from deleting the function
```

## Callbacks
* Common in C, especially with UI Work
* YottaDB uses callbacks for ACID transactions

## Callbacks - "Obivious approach"
* does not compile

```
// void do_callback(void (*cb)()) {
//   cb()
//   }
func callback() {
  fmt.Printf("here\n")
  }

  func main() {
    C.do_callback(callback)
  }
```

## Callbacks - "Obvious" solution :)
* split into two files
* C func to return callback

```
// void do_callback(void(*cb)())
```

## Callbacks Drawbacks
* error prone
* makes testing hard
    - can't import C in test code
* feels kludgy

## Callbacks alternative approache
* write a callback function to pass back an index to a hashmap that stores the
  Golang callback; use closure for passing parameters
* less error prone, with small performance hit
* still feels kludgy, but not so much to gophers

## Callback for transaction processing
```
base application -> call yottadb.TpST() -> call Transaction Logic
                 <- return              <- return
```

## Closure
```
func helloWorld(i int) {
  ( func() {
      fmt.Printf("i: %v", i)
      })()
}
```

this is how we chose to do transaction callbacks.

## Taming the CGO Dragons
* issues may not show up until garbage collection occurs.
  - sometimes only after days of intensive use
  - and even then only at random

## Poking the CGO Dragons
* trigger more garbage collection, export GOGC=1

* catch for Go pointers geing passed
 - export GODEBUG="cgocheck=2"

* horrible.mean.ugly.tests.

took us alot longer to get this code done. Thought it was going to be a 3 month
project that took a year.

## CGO dragons are slumbering, not Slain
* Go still believes it owns the process
  - Go devs know best because they develop Go primarily for their own use
* Another dragon not discussed here - signal handlers
* the cost of sharing a cave with a dragon is eternal vigilance
 - Test continuously, and then test some more
