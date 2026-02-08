# Select

* `select` is similar to `switch`, but specifically for **multiple channel operations**.
* It waits for **one** of the channel operation in its cases to become ready.
* **Syntax**
	```go
	select {
		case x := <-ch1:
			//...some code here...
		case y := <-ch2:
			//...some code here...
		default:
			//...some code here...
	}
	```
* **Behavior**:
	* If one `case` is ready, it executes immediately.
	* If multiple cases are ready, a **random** one is chosen for execution.
	* If no cases is ready:
		* If there is a `default`: `default` runs immediately (non-blocking).
		* If there is **no `default`**: the goroutine **blocks** until a case becomes ready.
* Very useful for:
	* Timeouts
	* Cancellation or done channels
	* Multiplexing work
	* Non-blocking send or receive
* **Use Case**
	* Implementing **timeouts**.
	* Implementing **cancellation or done propagation**.
	* **Non-blocking** communication (sends and receives).
	* Handling **multiple input channels** simultaneously.
* **Example**
	```go
	// fibonacci generates Fibonacci numbers and sends them on channel `c`.
	// The select waits on a send to `c` OR a receive from `quit`.
	func fibonacci(c, quit chan int) {
		x, y := 0, 1
	
		for {
			select {
			case c <- x:
				x, y = y, x+y
	
			case <-quit:
				fmt.Println("quit")
				return
			}
		}
	}
	
	func main() {
		c := make(chan int)
		quit := make(chan int)
	
		go func() {
			for i := 0; i < 10; i++ {
				fmt.Println(<-c)
			}
	
			quit <- 0
		}()
	
		fibonacci(c, quit)
	}
	```

## Default Case
* `default` is always ready.
* It executes **only if** all other cases would block.

> [!NOTE]
> Never use `default` inside a tight loop unless intentional. It causes busy waiting and wastes CPU.

#### Example
```go
func sendOrDrop(data []byte) {
	select {
		case ch <-data:
			// sent ok; do nothing
		default:
			log.Printf("overflow: drop %d bytes", len(data))
	}
}
```

## Handling Closed Channels
* `ok` is `false` when the channel is closed and drained of all values.
* Receiving from a closed channel **never blocks**.
```go
select {
	case val1, ok := <-ch1:
		if ok {
			fmt.Println("received:", val1)
		
		} else {
			fmt.Println("channel closed!")
		}
		
	case val2 := <-ch2:
		fmt.Println("received:", val2)
}
```

## Timeout
* **Best practice**: call `timer.Stop()` if your timeout doesn't fire.
```go
timer := time.NewTimer(2 * time.Second)

select {
	// If `ch1` sends a message before the timer completes, we can immediately stop the timer
	// with `timer.Stop()`, freeing up its associated resources.
	case val1 := <-ch1:
		fmt.Println("received:", val1)
		timer.Stop()
		
	case <-timer.C:
		fmt.Println("timeout")
}
```

### Label + `for` + `select`
* Using a `for` loop together with a `select` statement is a common way to continuously listen to multiple channels.
* Labels allows you to break out loop cleanly.

```go
outer:
	for {
		select {
			case val1, ok := <-ch1:
				if !ok {
					fmt.Println("ch1 is closed. exiting loop")
					// `break outer` statement tells the application to exit the `outer`
					// when it runs and the label helps in directing the flow precisely,
					// avoiding unintended continuations.
					break outer
				}
				
				fmt.Println("received ch1:", ch1)
				
			case val2, ok := <-ch2:
				if !ok {
					fmt.Println("ch2 is closed. exiting loop")
					break outer
				}
				
				fmt.Println("received ch2:", ch2)
			
			// `timer.After` creates a fresh timer every loop iteration.
			case <-time.After(1 * time.Second):
				fmt.Println("waiting...")
		}
	}
```

### Non-Blocking Send
* Channels allow communication between goroutines. By default, channel operations execute in a blocking manner.
* To implement a non-blocking channel operations, the `select` statement is used.

```go
func main() {
	nums := make(chan int)
	msgs := make(chan string, 3)
	
	// We use the select statement to check if a number was received from
	// the channel.
	select {
		case num := <-nums:
			fmt.Println("received:", num)
			
		default:
			fmt.Println("number not received")
	}
	
	// We define a msg to be sent to the msgs channel.
	msg := "hello, world!"
	
	// We use the select statement to check in the first case if msg is
	// received by the msgs channel.
	select {
		case msgs <- msg:
			fmt.Println("sent:", msg)
			
		default:
			fmt.Println("message not sent")
	}
	
	// If none of the cases is true, the default statement is executed.
	select {
		case msg := <-msgs:
			fmt.Println("received a message:", msg)
			
		case num := <-nums:
			fmt.Println("received a number:", num)
			
		default:
			fmt.Println("nothing happened")
	}
}
/* OUTPUT
 * number not received
 * sent: hello, world!
 * received a message: hello, world!
 */
```

### Select Control Flow
* A `select-case` code block without any branches, `select{}`, will make the current goroutine stay in blocking state forever.
* No `fallthrough` statements are allowed to be used in `case` branches.
* Each statement following a `case` keyword in a `select-case` code block must be either a receive channel or send channel operation statement.
* If there are one or more non-blocking `case` operations, Go runtime will randomly select one of these non-blocking operations to execute, then continue to execute the corresponding `case` branch.
* If all the `case` operations in are blocking operations, the `default` branch will be selected if it is present. If the `default` branch is absent, the current goroutine will be pushed into the corresponding sending or receiving goroutine queue of every channel involved in all `case` operations, then enter blocking state.

##  🔖 Extras
* [Channels in Go](https://go101.org/article/channel.html#select)
- [Essentials of Go - Select](https://essentials-of-go-programming.readthedocs.io/concurrency.html#select)
- [Select waits on a group of channels](https://yourbasic.org/golang/select-explained/)
- [Select & For Range Channel in Go: Breaking Down](https://blog.devtrovert.com/p/select-and-for-range-channel-i-bet)
* **Video**
	* [Go Class: 24 Select](https://www.youtube.com/watch?v=tG7gII0Ax0Q&list=PLoILbKo9rG3skRCj37Kn5Zj803hhiuRK6&index=24)