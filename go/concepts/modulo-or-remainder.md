# Modulo or Remainder
- The modulo operation is **used to determine the remainder** of a division operation
- To find the modulo of a given number, we can use the `%` operator
- The `%` operator returns the remainder of the division operation
- It is commonly used in programming for tasks such as checking if a number is even or add, or for calculating the position of elements in an array

```go
a := 10
b := 3
fmt.Println(a % b)
```

> [!NOTE]
> We can also find the modulo of a floating-point numbers using the `math.Mod()` function from the `math` package. The function takes two arguments: `a` and `b`. It returns the floating-point remainder of `a` divided by `b`.
> 
> **Example**
> ```go
> a := 10.5
> b : 2.2
> fmt.Println(math.Mod(a, b))
> ```