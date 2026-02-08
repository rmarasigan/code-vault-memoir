# `select` Statement

- It is used to choose from multiple send/receive channel operations
- `select` statement blocks until one of the send/receive operations is ready
- The syntax is similar to `switch` except that each of the `case` statements will be a channel operation

```go
func server1(ch chan string) {
	time.Sleep(6 * time.Second)
	ch <- "from server1"
}

func server2(ch chan string) {
	time.Sleep(3 * time.Second)
	ch <- "from server2"
}

func main() {
	output1 := make(chan string)
	output2 := make(chan string)
	
	// It will write to output1 channel after 6 seconds
	go server1(output1)
	
	// It will write to output2 channel after 3 seconds
	go server2(output2)
	
	// select statement blocks until one of its cases is ready
	select {
	case s1 := <-output1:
		fmt.Println(s1)
		
	// It will block for 3 seconds and will wait for server2 goroutine to write
	// to the output2 channel
	case s2 := <-output2:
		fmt.Println(s2)
	}
}
```

### Default Case
- The `default` case in a `select` statement is executed when none of the other cases is ready
- Generally used to prevent `select` statement from blocking

```go
func process(ch chan string) {
	// Sleeps for 10.5 seconds and then writes "process successful" to ch
	time.Sleep(10500 * time.Millisecond)
	ch <- "process successful"
}

func main() {
	ch := make(chan string)
	go process(ch)
	
	// The infinite loop sleeps for 1 second during the start of each iteration
	// and then performs a select operation
	for {
		time.Sleep(1000 * time.Millisecond)
		
		select {
		case v := <-ch:
			fmt.Println("received value: ", v)
			return
			
		default:
			fmt.Println("no value received")
		}
	}

}
```

During the first 10500 milliseconds (10.5 seconds), the first case of the `select` statement will not be ready since the `process` goroutine will write to the `ch` channel only after 10500 milliseconds. Hence, the `default` case will be executed during this time and the program will print ‘`no value received`' 10 times.

After 10.5 seconds, the `process` goroutine writes ‘`process successful`' to `ch`. Now the first case of the select statement will be executed and the program will print ‘`received value: process successful`' and then it will terminate.

```
no value received
no value received
no value received
no value received
no value received
no value received
no value received
no value received
no value received
no value received
received value:  process successful
```

### Deadlock and Default Case
```go
func main() {
	ch := make(chan string)
	
	select {
	case <-ch:
	}
}
```

The select statement will block forever since no other goroutine is writing to the `ch` channel and hence will result in deadlock. This will panic at runtime.

```go
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.main()
	/tmp/sandbox627739431/prog.go:6 +0x4d
```

If a `default` case is present, this deadlock will not happen since the `default` case will be executed when no other case is ready. Similarly, the `default` case will be executed even if the `select` has only `nil` channels.

```go
func main() {
	var ch chan string
	select {
	case v := <-ch:
		fmt.Println("received value", v)
	default:
		fmt.Println("default case executed")

	}
}
```

#### Gotcha — Empty Select
The select statement will block until one of its cases is executed. In here, the select statement doesn't have any cases and hence it will block forever resulting in a deadlock.

```go
func main() {
	select {}
}
```

## Overview for Concurrency Communication
### Concurrency

- Refers to **code that is written in a concurrent design pattern**
- The code has the potential **ability to execute multiple tasks simultaneously**, where **each task may make progress independently of the others**
- It is achieved using **goroutines**, lightweight threads of execution that are managed by the Go runtime
- The _communication_ and _synchronization_ of these goroutines is typically done using **channels**, which provide a way for goroutines to exchange data and coordinate their execution

### Parallelism
- Refers to the **ability of a program to execute multiple tasks simultaneously by utilizing multiple CPUs or cores**
- It can **often speed up the execution of a program** by allowing multiple parts of the program to run in parallel on different processors
- Can be achieved by running multiple goroutines on different processors using the `go` keyword

### **Serial / Sequential Execution**
- Opposite of parallel computing
- Each instruction or task is **executed one after the other in a predefined order**, so that each instruction must wait for the previous one to finish before it can start
- Typically used when the instructions or tasks are dependent on each other, or when the resources required to execute the code are limited

## 🔖 Extras
- [Select](https://golangbot.com/select/)