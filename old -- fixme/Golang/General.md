## ToDO
- [ ]  What is the difference between Identifier, Literal and Operator?
- [ ] Strong vs Weak Typed 
- [ ] Static vs Dynamic Typed
- [ ]  assertion - check dynamic type during runtime

## What is the difference between `statement` and `expression`?

Expression evaluates to value, statement does not.
Statement is an instruction to do something, we want it for side effects

Expression is a part of Statement.
Expressions are reduced to value
```
1      // expression, statement
1+1    // expression, statement
x int = 1  // statement
func add(x int, y int) // statement
    return x+y         // statement as a whole, (x+y) is an expression
add(1, 2) // expression
```

### Functions
- [ ] TODO: https://go.dev/ref/spec#Function_types

```
func SIGNATURE

Signature = Parameters [ Result ]
```

How cool is that:
```
func woot() (succ bool) {
	return true
}
```

## Errors
```
// No error matching (static vs dynamic)
errors.New("could not open")
fmt.Errorf("could not open file: %q", file)


// With error matching (static vs dynamic)
var NotFoundError = errors.New("could not open")
type NotFoundError struct {
	File string
}
func (e *NotFoundError) Error() error {
	return fmt.Sprintf("File %q not found", e.File)
}
```