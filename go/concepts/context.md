# Context

* **`context.Context`** provides a standard mechanism for request lifecycle management in Go. It enables:
	* Cancellation propagation across goroutines
	* Timeouts and deadlines
	* Propagation of request-scoped metadata
* Contexts allow related operations to terminate gracefully and consistently, preventing goroutine leaks and unbounded waits.
* The `Done` method returns a **channel that acts as a cancellation signal** to functions running on behalf of the `Context`: when the channel is closed, the functions should abandon their work and return. It is a receive-only.
* The `Err` method **returns an error** indicating why the `Context` was canceled.
* The `Deadline` method allows functions to determine whether they should start work at all; if too little time is left, it may not be worthwhile.
* The `Value` allows a `Context` to carry **request-scoped data**. That data must be safe for simultaneous use by multiple goroutines.

### Creating Contexts
* Contexts **forms tree**. Child contexts derive cancellation, deadlines, and values from their parent.
* **`context.Background()`**
	* An empty root context.
	* Use only at application boundaries (e.g., `main`, tests, initialization).
	* Never create a new root context inside request-handling code.
* **`context.WithCancel(parent)`**
	* Returns a **cancelable child context** and a `cancel()` function.
* **`context.WithTimeout(parent, duration)`**
	* Cancels automatically after a duration.
* **`context.WithDeadline(parent, time)`**
	* Cancels automatically at a specific time.

> [!NOTE]
> Always call `cancel()`  to release resources, even when using timeouts or deadlines.
>
> **Example**
> ```go
> ctx, cancel := context.WithTimeout(parentCtx, 2*time.Second)
> defer cancel()
> ```

### Propagating Context
* Contexts **must be passed explicitly** through function calls. Never store them globally.
* `context.Context` is **always the first parameter**.
* **Example**
	```go
	func handler (ctx context.Context) error {
		return process(ctx)
	}
	```

### Context Values
* Contexts may carry **request-scoped metadata** (e.g., request IDs, authentication). They **must not** store business data, configuration, or large objects.
	* **Example**
		```go
		ctx = context.WithValue(ctx, "userID", 1234)
		
		// Retrieval
		userID, ok := ctx.Value("userID").(int)
		```

### Cancellation Propagation
* Cancellation signals propagate automatically to all derived contexts.
* **Example**
	```go
	func main() {
		ctx, cancel := context.WithCancel(context.Background())
		go worker(ctx)
		
		time.Sleep(2 * time.Second)
		cancel()
	}
	
	func worker(ctx context.Context) {
		for {
			select {
				// <-ctx.Done() is triggered on cancellation or deadline expiration.
				case <-ctx.Done():
					// ctx.Error() returns context.Canceled or context.DeadlineExceeded.
					fmt.Println("Canceled:", ctx.Err())
					return
					
				default:
					time.Sleep(500 * time.Millisecond)
			}
		}
	}
	```

### Timeouts and Deadlines
* Timeouts and deadlines prevent operations from blocking indefinitely.
* Use timeouts for network calls, database queries, external service dependencies, etc.
* **Example**
	```go
	ctx, cancel := context.WithDeadline(context.Background(), time.Now().Add(2*time.Second))
	defer cancel()
	```

### Cancellation Causes (Go 1.20+)
* **`context.WithDeadlineCause`**
	* Associate a custom error with a deadline.
	* **Example**
		```go
		ctx, cancel := context.WithDeadlineCause(parentCtx, time.Now().Add(100*time.Millisecond), errors.New("DB timeout"))
		```
* **`context.WithTimeoutCause`**
	* Associate a custom error with a timeout.
	* **Example**
		```go
		ctx, cancel := context.WithTimeoutCause(parentCtx, time.Now().Add(100*time.Millisecond), errors.New("backend DB timed out"))
		```
* Retrieve the cause: `err := context.Cause(ctx)`
* **Benefits**
	* Improved debugging of cascading timeout failures.
	* Greater visibility into timeout sources as errors propagate.
	* More context for handling and recovering from timeout errors.

### `context.AfterFunc`
* Registers a function that runs asynchronously **after** a context is canceled.
* Useful for cleanup and teardown logic tied to request lifecycles.
* **Example**: `stop := context.AfterFunc(ctx, func() { cleanup() })`

### `context.WithoutCancel`
* Creates a context that **does not inherit cancellation from its parent**, while still inheriting values.
* **Example**: `ctx := context.WithoutCancel(parentCtx)`
* Use it with care. Legitimate use cases include:
	* Background logging or metrics that must compile.
	* Cleanup or audit operations that should survive request cancellation.
* Avoid using this to bypass correct cancellation semantics.

## Common Use Cases
* **HTTP Servers**
	* It allows you to control request cancellation, deadlines, and request-scoped metadata.
* **Database Queries**
	* It allows you to manage cancellation and timeout enforcement.
* **Distributed Systems**
	* Consistent termination across service boundaries.

## Best Practices
* **Pass Context Explicitly**
	* Never use globals and never pass `nil`.
	* Always derive from the incoming context.
		* This makes it easier to manage the context's lifecycle and prevents potential data races.
	* The `Context` should be the first parameter, typically named `ctx`.
		```go
		func DoSomething(ctx context.Context, arg Arg) error {...}
		```
* **`context.TODO()`**
	* Use as a temporary placeholder.
	* Indicates unfinished refactoring.
	* It **should not remain** in production code.
* **Avoid using `context.Background()`**
	* Create a specific context using `context.WithCancel()` or `context.WithTimeout()` to manage its lifecycle and avoid resource leaks.
	* `Background` lacks cancelation and timeouts. Use the `WithCancel`, `WithTimeout`, or `WithDeadline` functions to add control.
* **Prefer Explicit Cancellation**
	* Use `context.WithCancel()` when cancellation is event-driven.
	* Use `context.WithTimeout()` when automatic enforcement is required.
	* Always call `cancel()` to prevent timer and resource leaks.
		* Idiomatic pattern: `defer cancel()` immediately after creation.
* **Keep Contexts Small**
	* Store only request-scoped metadata.
	* Avoid large values or optional parameters.
* **Avoid Chaining Contexts**
	* Propagate a single context throughout the application.
	* Contexts are designed to be derived. Each layer may add deadlines, cancellation, or values.
	* Never replace an incoming context with `Background()`.
* **Avoid Goroutine Leaks**
	* Always ensure that goroutines associated with a context are properly closed or terminated to avoid goroutine leaks.
	* Every goroutine should:
		* Observe `<-ctx.Done()`.
		* Exit promptly on cancellation.
* **Third-Party Libraries**
	* Prefer libraries that accept `context.Context`.
	* Wrapping third-party library is a best practice when they:
		* Do not accept `context.Context`.
		* Block for long periods.
		* Lack cancellation or timeout support.
	* When wrapping:
		* Accept `context.Context` in your wrapper function.
		* Check `ctx.Done()` before starting work.
		* Invoke the third-party API.
		* Check `ctx.Done()` again for early exit.
		* Run blocking calls in a goroutine if necessary.
		* Enforce reasonable defaults for timeouts and deadlines.

## 🔖 Extras
* **Medium**
	* [The Complete Guide to Context in Golang: Efficient Concurrency Management](https://medium.com/@jamal.kaksouri/the-complete-guide-to-context-in-golang-efficient-concurrency-management-43d722f6eaea)
* [Go Contexts and structs](https://go.dev/blog/context-and-structs)
* [Go `context` Documentation](https://pkg.go.dev/context)
* [Ardan Labs - Context Package](https://tour.ardanlabs.com/tour/eng/context/1)
* [Go Concurrency Patterns: Context](https://go.dev/blog/context)
* [Cancellation, Context, and Plumbing](https://go.dev/talks/2014/gotham-context.slide#1)