![Go](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQdfTh-cZhYChh7v0pwGZLhswU85BDA5FyO7Q&usqp=CAU)

# Golang from 0 to hero

Go is an open-source, statically-typed compiled, and explicit programming language, supports concurrency, is also
multi-paradigm and object-oriented, and it was inspired by Python and C.

#### Fun facts

* Robert Griesemer (V8 JavaScript creator), Rob Pike (Unix team, UTF-8 creator), and Ken Thompson (Unix creator and B
  language [C predecessor]) designed Go at Google in 2007.

* Google wanted an alternative to C++ that solved the issues of software engineering, this gave rise to the development
  of the Go programming language.

* Go is supported on almost every OS like DragonFly BSD, FreeBSD, Linux, macOS, NetBSD, OpenBSD, Plan 9, Solaris, and
  Windows.

* Go provides 2 features that are capable of replacing Class inheritance (embedding and interfaces).

* Go has garbage collection which automatically performs memory management and permits deferred execution of functions.

* Although it currently does not support generics, there are rumors that in the upcoming versions it might.


## Advantages

![GoNinja](https://juststickers.in/wp-content/uploads/2019/01/gopher-ninja.png)

* **Modern**

* **Simplicity & Performance**

* **Syntax Sugar**

* **Slices**

* **Complainer Compiler**

* **Multi paradigm (Structured & Functional & OO)**

* **Go Routines & Channels**

## Guide

![Guide](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTjjqqQy4iBpGcG0oFXsnsTUftl9lzIOss3-Q&usqp=CAU)

### Naming

* **Package names**

* **Interface names**

* **MixedCaps (snakeCase) -> access modifier**

        function thisIsProtected() { }
        function ThisIsPublic() { }
* **Semicolon** -> not needed, exception (assignment/condition check):

        if foo := bar(); foo > 10 { code ... }

### Control Structures

* **Assignment** (const/var, implicit/explicit)

        var example string
        const example = "text string"
        example := "text string"
        example := new(FooBar) //pointer -> see pointers section
        exampleSlice := make([]string, 0) // make for slices (arrays) and maps

        const (
            _           = iota // ignore first value by assigning to blank identifier
            KB ByteSize = 1 << (10 * iota)
            MB
            GB
            TB
            PB
            EB
            ZB
            YB
        )


* **Redeclaration and reassignment**

      f, err := os.Open(name)
      .
      .
      .
      d, err := f.Stat()

* **Multiple assignment**

      a, b := 10, "text"

* **Loops**

       // Like a C for
      for init; condition; post { }

      // Like a C while
      for condition { }
    
      // Like a C for(;;)
      for { }
      
      //slice/map iteration  
      for key, value := range list {
        newList[key] = value
      }

* **if / switch / type switch**

        if condition { code } // no parenthesis needed

        func unhex(c byte) byte {
          switch {
            case '0' <= c && c <= '9': 
              return c - '0'
            case 'a' <= c && c <= 'f':
              return c - 'a' + 10
            case 'A' <= c && c <= 'F':
              return c - 'A' + 10
          }
          return 0
        }

        func shouldEscape(c byte) bool {
           switch c {
             case ' ', '?', '&', '=', '#', '+', '%':
               return true
           }
           return false
        }

        var t interface{}
        t = functionOfSomeType()
        switch t := t.(type) {
          default:
            fmt.Printf("unexpected type %T\n", t)     // %T prints whatever type t has
          case bool:
            fmt.Printf("boolean %t\n", t)             // t has type bool
          case int:
            fmt.Printf("integer %d\n", t)             // t has type int
          case *bool:
            fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
          case *int:
            fmt.Printf("pointer to integer %d\n", *t) // t has type *int
        }

### Functions

* **Multiple return values / Named result parameters**

        func (file *File) Write(b []byte) (int, error) { }

        func nextInt(b []byte, pos int) (value, nextPos int) { }

* **Defer**

        func Contents(filename string) (string, error) {
            f, err := os.Open(filename)
            if err != nil {
               return "", err
            }
            defer f.Close()  // f.Close will run when we're finished.
            .
            .
            . // more code
            
            return string(result), nil // f will be closed if we return here.
        }

* **init** -> niladic state initializing function

### Structure/Constructors

* **new and make**

        p := new(SyncedBuffer)  // type *SyncedBuffer initiallized with zeroed values
        var v SyncedBuffer      // type  SyncedBuffer
        
        var p *[]int = new([]int)       // allocates slice structure; *p == nil; rarely useful
        var v  []int = make([]int, 100) // the slice v now refers to a new array of 100 ints

        // Unnecessarily complex:
        var p *[]int = new([]int)
        *p = make([]int, 100, 100) 

        // Idiomatic:
        v := make([]int, 100)


* **Composite literals**

       func NewFile(fd int, name string) *File {
           if fd < 0 {
               return nil
           }
           f := File{fd, name, nil, 0} //creating the object with fields
           return &f
       }

* **Arrays/Slices**

    * Arrays are values. Assigning one array to another copies all the elements.

    * In particular, if you pass an array to a function, it will receive a copy of the array, not a pointer to it.

    * The size of an array is part of its type. The types [10]int and [20]int are distinct.

    * Slices hold references to an underlying array, and if you assign one slice to another, both refer to the same
      array. If a function takes a slice argument, changes it makes to the elements of the slice will be visible to the
      caller, analogous to passing a pointer to the underlying array.

              func (f *File) Read(buf []byte) (n int, err error) {
                  n, err := f.Read(buf[0:32]) //Reads from the 0 poisition up to 32 items of the buf slice (first 32 bytes)
              }

    * Append / Variadic params

          func append(slice []T, elements ...T) []T

          x := []int{1,2,3}
          y := []int{4,5,6}
          x = append(x, y...)
          fmt.Println(x)

* **Two-dimensional slices**

    * type Transform [3][3]float64 // A 3x3 array, really an array of arrays.

    * type LinesOfText [][]byte // A slice of byte slices.

* **Maps**

        var timeZone = map[string]int{
          "UTC":  0*60*60,
          "EST": -5*60*60,
          "CST": -6*60*60,
          "MST": -7*60*60,
          "PST": -8*60*60,
        }

* **Printing** -> C-like printing patterns

        fmt.Printf("Hello %d\n", 23)
        fmt.Fprint(os.Stdout, "Hello ", 23, "\n")
        fmt.Println("Hello", 23)
        fmt.Println(fmt.Sprint("Hello ", 23))
    
        var x uint64 = 1<<64 - 1
        fmt.Printf("%d %x; %d %x\n", x, x, int64(x), int64(x))

  prints

        18446744073709551615 ffffffffffffffff; -1 -1

        fmt.Printf("%v\n", timeZone)  // or just fmt.Println(timeZone)

  which gives output:

        map[CST:-21600 EST:-18000 MST:-25200 PST:-28800 UTC:0]

* **Receiver & Pointers vs. Values**

        type ByteSlice []byte

        func (slice ByteSlice) Append(data []byte) []byte {
            // Body exactly the same as the Append function.
        }

  vs

        func (p *ByteSlice) Append(data []byte) {
            //no need to return value because the refereced P will be modified in it's memory address
        }

### Interfaces

Implicit implementation

        type Foo interface {
            Bar(s string) error
        }

        type myClass struct {}

        func (this myClass) Bar (myString string) error { code... }    

MyClass now implicitly implements the Foo interface and could be returned as a Foo type:

      func NewFoo () Foo { return &MyClass{} }

* **Generality**

If a type exists only to implement an interface and will never have exported methods beyond that interface, there is no need to export the type itself.

* **Type assertion / cast operator**

      type Stringer interface {
        String() string
      }
      
      var value interface{} // Value provided by caller.
      switch str := value.(type) {
        case string:
          return str
        case Stringer:
          return str.String()
      }

### Blank Identifier

* **Explicitly ignoring values so the compiler does not complain**


      if _, err := os.Stat(path); os.IsNotExist(err) {
        fmt.Printf("%s does not exist\n", path)
      }

![ThisisFine](https://www.dictionary.com/e/wp-content/uploads/2018/03/This-is-Fine-300x300.jpg)
  
  **DO NOT do this:**

      // Bad! This code will crash if path does not exist.
      fi, _ := os.Stat(path)
      if fi.IsDir() {
          fmt.Printf("%s is a directory\n", path)
      }

* **Import for side effect**

      _ "github.com/go-sql-driver/mysql"

* **Interface checks**

      if _, ok := val.(json.Marshaler); ok {
        fmt.Printf("value %v of type %T implements json.Marshaler\n", val, val)
      }


### Embedding

    type Reader interface {
      Read(p []byte) (n int, err error)
    }
    
    type Writer interface {
      Write(p []byte) (n int, err error)
    }

    // ReadWriter is the interface that combines the Reader and Writer interfaces.
    type ReadWriter interface {
      Reader
      Writer
    }

    type Foo struct {
      S string
      N int
      F bool
    }
    
    //Bar will also contain all fields declared in Foo and all exported functions declared in log.Logger
    type Bar struct {
      Foo
      *log.Logger
      Date time.Time
    }

### Concurrency

*Do not communicate by sharing memory; instead, share memory by communicating.*

* **Goroutines**

It is a function executing concurrently with other goroutines in the same address space. 
It is lightweight, costing little more than the allocation of stack space. 
And the stacks start small, so they are cheap, and grow by allocating (and freeing) heap storage as required.

    go list.Sort()  // run list.Sort concurrently; don't wait for it.

Wrapping flow inside anonymous func to run with go routines:

    func Announce(message string, delay time.Duration) {
        go func() {
            time.Sleep(delay)
            fmt.Println(message)
        }()  // Note the parentheses - must call the function.
    }




* **Avoiding Data races sharing them among go routines using channels**

Like maps, channels are allocated with make, and the resulting value acts as a reference to an underlying data structure. If an optional integer parameter is provided, it sets the buffer size for the channel. The default is zero, for an unbuffered or synchronous channel.

    ci := make(chan int)            // unbuffered channel of integers
    cj := make(chan int, 0)         // unbuffered channel of integers
    cs := make(chan *os.File, 100)  // buffered channel of pointers to Files

Receivers always block until there is data to receive

    c := make(chan int)  // Allocate a channel.
    // Start the sort in a goroutine; when it completes, signal on the channel.
    go func() {
        list.Sort()
        c <- 1  // Send a signal; value does not matter.
    }()
    doSomethingForAWhile()
    <-c   // Wait for sort to finish; discard sent value.

* **Parallelization**

      
      type Vector []float64

      // Apply the operation to v[i], v[i+1] ... up to v[n-1].
      func (v Vector) DoSome(i, n int, u Vector, c chan int) {
          for ; i < n; i++ {
                v[i] += u.Op(v[i])
          }
          c <- 1    // signal that this piece is done
      }

      const numCPU = 4 // number of CPU cores

      func (v Vector) DoAll(u Vector) {
          c := make(chan int, numCPU)  // Buffering optional but sensible.
          for i := 0; i < numCPU; i++ {
            go v.DoSome(i*len(v)/numCPU, (i+1)*len(v)/numCPU, u, c)
          }
          // Drain the channel.
          for i := 0; i < numCPU; i++ {
            <-c    // wait for one task to complete
          }
          // All done.
      }


[Curiosity: Race Condition detector **-race** flag](https://go.dev/blog/race-detector)

    go run -race main.go

[Alternative: WaitGroups](https://gobyexample.com/waitgroups)

### Errors

    type error interface {
        Error() string
    }

    // PathError records an error and the operation and
    // file path that caused it.
    type PathError struct {
      Op string    // "open", "unlink", etc.
      Path string  // The associated file.
      Err error    // Returned by the system call.
    }
    
    func (e *PathError) Error() string {
      return e.Op + " " + e.Path + ": " + e.Err.Error()
    }

* **Panic/Recover**

**Panic**

      var user = os.Getenv("USER")
  
      func init() {
        if user == "" {
           panic("no value for $USER")
        }
      }

**Recover**

      func server(workChan <-chan *Work) {
          for work := range workChan {
            go safelyDo(work)
          }
      }

      func safelyDo(work *Work) {
        defer func() {
          if err := recover(); err != nil {
            log.Println("work failed:", err)
         }
        }()
        do(work)
      }

### Reflection

*Disclaimer: Reflection takes a heavy toll on performance, so use it with caution*

[Reflect package](https://pkg.go.dev/reflect)

[Laws of reflection](https://go.dev/blog/laws-of-reflection)

* Reflection goes from interface value to reflection object.

* Reflection goes from reflection object to interface value.

* To modify a reflection object, the value must be settable.

### Presentation primarily based on

[Effective Go](https://www.manning.com/books/effective-go)
(there's a free [online](https://go.dev/doc/effective_go) version as well)

### Cool articles

* [Why should you learn Go? — Keval Patel — Medium](https://medium.com/@kevalpatel2106/why-should-you-learn-go-f607681fad65)

* [Farewell Node.js — TJ Holowaychuk — Medium](https://medium.com/@tjholowaychuk/farewell-node-js-4ba9e7f3e52b)

### Recommended reading

* [Building Microservices with Go](https://www.amazon.com/Building-Microservices-Go-efficient-microservices/dp/1786468662)
* [Go Design Patterns](https://www.amazon.com.br/Design-Patterns-Mario-Castro-Contreras/dp/1786466201)
* [Go in Action](https://www.manning.com/books/go-in-action)

*(PS1: those are the ones I have paperback, very good books)*

*(PS2: first two I have the e-book version as well, I can share with anyone that wants)*

![Questions](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTmGVTvzs27YTGluVH6Zpjz67E6tvwYka45tQ&usqp=CAU)

The end

![The end](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRfT59sw-ycynv522AjyD3eLoBu49fs87p-Vw&usqp=CAU)
