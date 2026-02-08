# Concrete Type vs Interface Type

### Concrete Type
- A type that you can **directly instantiate or create a value** from.
- A concrete type is a data type that is **not an interface** and specifies the **exact representation of its values**, exposing their intrinsic operations and any additional behaviours through methods.
- The type can directly represent a set of values, and you can create an instance of this type without any additional information.
- **Examples**
    - `int`
    - `bool`
    - `string`
    - `float64`
    - arrays
    - slices
    - `map`
    - `struct`
- They have a specific, predetermined storage layout and characteristics.
- **Example**
    ```go
    type Employee struct {
    	Name string
    	Age  int
    }
    
    func main() {
    	emp := Employee{Name: "John Doe", Age: 25}
    }
    ```

### Interface Type
- It defines a contract (set of methods or types) but does not represent a specific data layout or instance.
- An **abstract type that defines a set of methods** a concrete type must implement to satisfy the interface.
- Interface types are abstract — they represent behaviour or type, but not a specific set of values.
- They **do not expose the internal structure** of their values; they only **reveal the behaviours** provided by their methods.
- **Example**
    ```go
    type Reader interface {
    	Read(p []byte) (n int, err error)
    }
    ```

## 🔖 Extras
- [Chapter 7: Interfaces](https://notes.shichao.io/gopl/ch7/)