# The `init()` Function

- It will execute as soon as the package is imported, and can be used when you need your application to initialize in a specific state, such as when you have a specific configuration or set of resources with which your application needs to start
- Can be declared multiple times throughout a package, however, multiple `init()` functions can make it difficult to know which one has priority over the others

> [!NOTE]
> - It is incredibly important to note that you cannot rely upon the order of execution of your `init()` functions. It is instead better to focus on writing your systems in such a way that the order does not matter.

**Example**
```go
var weekday string

func main() {
	fmt.Printf("Today is %s", weekday)
}
```

We declared a global variable called `weekday` and by default, the value of `weekday` is an empty string.

```bash
$ go run main.go
Today is
```

When we run the program, the value of `weekday` is blank. We can fill in the blank variable by introducing an `init()` function that initializes the value of `weekday` to the current day.
```go
var weekday string

func init() {
	weekday = time.Now().Weekday().String()
}

func main() {
	fmt.Printf("Today is %s", weekday)
}
```

When we run the program, we are going to get the current weekday.
```bash
$ go run main.go
Today is Monday
```

## Pros and Cons of Using `init()`
- **Pros**
    - **Automatic Execution**
        - The `init` function is called automatically when a package is initialized, right before the `main` function executes
    - **Multiple `init` Functions**
        - You can have multiple `init` functions within a single package or even a single file
    - **No Parameters or Returns**
        - The `init` function cannot accept parameters or return values
- **Cons**
    - **Hidden Side Effects**
        - **Complex Debugging** — hidden initializations make debugging more complicated because the program state changes without explicit calls
        - **Unpredictable Behavior** — unexpected side effects can lead to unpredictable behavior, especially in larger code bases
    - **Testing Difficulties**
        - **Order of Execution** — execution order of `init` functions across packages can affect tests, making them flaky or unreliable
        - **Isolation Issues** — it can be hard to isolate and test individual components without triggering unwanted initializations
    - **Decreased Readability and Maintainability**
        - **Implicit Actions** — developers new to the code base might not realize that certain initializations occur in the `init` function
        - **Maintenance Overhead** — as the code evolves, the `init` function can become a dumping ground for various initializations, increasing complexity
    - **Package Import Issues**
        - **Unintended Imports** — importing a package solely for its side effects (i.e., its `init` function) can lead to messy dependencies
        - **Cyclic Dependencies** — overuse of `init` functions can contribute to cyclic dependencies between packages, complicating the build process

## Best Practices
- **Keep it simple** — limit the `init` function to simple, unavoidable initializations
- **Avoid side effects** — do not perform actions that alter the state in unexpected ways
- **Document thoroughly** — clearly document what the `init` function does to aid other developers

## 🔖 Extras
- [Understanding init in Go](https://www.digitalocean.com/community/tutorials/understanding-init-in-go)
- [The Go init Function](https://tutorialedge.net/golang/the-go-init-function/)
- [Why You Should Think Twice Before Using the init Function in Go](https://www.bytesizego.com/blog/init-function-golang)