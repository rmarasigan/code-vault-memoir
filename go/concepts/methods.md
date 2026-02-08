# Methods

- Are like functions but with a key difference: **they have a receiver argument**, which allows to access the receiver's properties. The receiver can be a struct or non-struct type, but both must be in the same package.
- Methods are really just functions that provide syntactic sugar to provide the ability for data to exhibit behavior.

## **Syntax and Declaration**
- **Syntax**
    ```go
    func(r ReceiverType) MethodName(parameter Type) (returnType) {
    	// Method Body
    }
    ```
    - Where:
        - `r`: A variable that you will use to access the data structure in the method. It is effectively an argument of the method.
        - `ReceiverType`: The type that this method is associated with. It can be any type except for interface types or pointer types.
        - `MethodName`: The name of the method. It follows the same conventions as naming functions in Go.
- **Struct Type Receiver**
    - The receiver is accessible inside the method.
- **Non-Struct Type Receiver**
    - Go allows methods with non-struct type receivers, as long as the receiver's type and method definition are present in the same package.
    - You cannot define a method with a receiver type from another package (e.g., `int`, `string`).
    - **Example**
        ```go
        // Creating a custom type based on int
        type number int
        
        // Defining a method with a non-struct receiver
        func (n number) square() number {
        	return n * n
        }
        
        func main() {
        	a := number(4)
        	b := a.square()
        	
        	fmt.Println("Square of", a, "is", b)
        }
        ```
- **Value Receiver**
    - The method operates under value semantics and will operate on its own copy of the value used to make the call.
    - **Syntax**
        ```go
        func (r Type) MethodName(...Type) {
        	// Method Body
        }
        ```
- **Pointer Receiver**
    - The method operates under pointer semantics and will operate on shared access to the value used to make the call.
    - This **allows changes made in the method to reflect in the caller**, which is not possible with value receivers.
    - **Syntax**
        ```go
        func (r *Type) MethodName(...Type) {
        	// Method Body
        }
        ```
    - **Example**
        ```go
        // Defining a struct
        type person struct {
        	name string
        }
        
        // Method with pointer receiver to modify data
        func (p *person) changeName(newName string) {
        	p.name = newName
        }
        
        func main() {
        	p1 := person{name: "John Doe"}
        	fmt.Println("Before:", p1.name)
        	
        	// Calling the method to change the name
        	p1.changeName("Jenny MoneyPenny")
        	fmt.Println("After:", p1.name)
        	// OUTPUT
        	// Before: John Doe
        	// After: Jenny MoneyPenny
        }
        ```
- **Value and Pointer Receivers**
    - You can pass either a pointer or a value, and the method will handle it accordingly.
    - **Example**
        ```go
        type person struct {
        	name string
        }
        
        // Method with pointer receiver
        func (p *person) updateName(newName string) {
        	p.name = newName
        }
        
        // Method with value receiver
        func (p person) showName() {
        	fmt.Println("Name:", p.name)
        }
        
        func main() {
        	p1 := person{name: "John Doe"}
        	
        	// Calling pointer method with value. It rewrites this as
        	// (&p1).updateName("...").
        	p1.updateName("Jenny MoneyPenny")
        	fmt.Println("After pointer method:", p1.name)
        	
        	// Calling value method with pointer. It rewrites this as p1.showName().
        	// You can just write p1.showName(). You need to use &p1 only if you're
        	// storing the pointer (e.g. ptr := &p1), passing a pointer to a
        	// function that expects one, or using an unaddressable value.
        	(&p1).showName()
        	// OUTPUT
        	// After pointer method: Jenny MoneyPenny
        	// Name: Jenny MoneyPenny
        }
        ```

## Method Calls
- When making a method call, the compiler doesn't care if the value used to make the call matches the receiver's data semantics exactly. Go adjusts to make the method call when calling the method. The compiler just wants a value or pointer of the same type.

## Data Semantic Guideline
### Internal Types

- If the data you're working with is an internal type (slice, map, channel, function, interface), then use value semantics to move the data around the program. This includes declaring fields on a type.
- A function can decide what data input and output it needs. What it can't decide is the data semantics for how the data flows in or out. The data drives that decision and the function must comply.

### Struct Types
- If the data you're working with is a struct type then you have to think about what the data represents to make a decision.
- A good general rule is to ask if the struct represents data or an API.
    - **Data**: Use value semantics.
    - **API**: Use pointer semantics.
- If value semantics are at play, you can switch to pointer semantics for some functions as long as you don't let the data in the remaining call chain switch back to value semantics. Once you switch to pointer semantics, all future calls from that point need to stick to pointer semantics.
- **Example**
    ```go
    type data struct {
        name string
        age  int
    }
    
    func (d data) displayName() {
        fmt.Println("My Name Is", d.name)
    }
    
    func (d *data) setAge(age int) {
        d.age = age
        fmt.Println(d.name, "Is Age", d.age)
    }
    
    func main() {
        d := data{
            name: "Bill",
        }
    
    		// This is not a method call, but an assignment which creates a level
    		// of indirection.
        f1 := d.displayName
        f1()
        
        d.name = "Joan"
        
        // The method is called once again, but you don't see the change. Bill is
        // the output once again. It has to do with the data semantics at play.
        // The displayName method is using a value receiver so value semantics are
        // at play. This means that f1 variable maintains and operates against its
        // own copy of d.
        f1()
        
        f2 := d.setAge
        f2(45)
        
        // This time, the method setAge is executed passing for Bill's age. Then
        // Bill's name is changed to Sammy and the f2 variable is used again to
        // make the call. The setAge function is using a pointer receiver so it
        // doesn't operate on its own copy of the d variable, but it is operating
        // directly on the d variable.
        d.name = "Sammy"
        f2(45)
        // OUTPUT
        // My Name Is Bill
        // My Name Is Bill
        // Bill Is Age 45
        // Sammy Is Age 45
    }
    ```

## 🔖 Extras
- [Methods in Golang](https://www.geeksforgeeks.org/methods-in-golang/)
- [Ardanlabs — Methods](https://tour.ardanlabs.com/tour/eng/methods/1)