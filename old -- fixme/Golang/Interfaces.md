
---
## Refs
- https://go.dev/ref/spec#Interface_types

---
## Desc

Let's refresh some vocabulary from `functions`, as we could draw similarities
- `parameter` is the variable in the declaration of the function.
- `argument` is the actual value of this variable that gets passed to the function.

### Interface type
An interface type defines a type set. A variable of interface type can store a value of any type that is in the type set of the interface. Such a type is said to implement the interface. The value of an uninitialised variable of interface type is nil.

```
InterfaceType  = "interface" "{" { InterfaceElem ";" } "}" .
InterfaceElem  = MethodElem | TypeElem .
MethodElem     = MethodName Signature .
MethodName     = identifier .
TypeElem       = TypeTerm { "|" TypeTerm } .
TypeTerm       = Type | UnderlyingType .
UnderlyingType = "~" Type .
```

#### Basic Interface
Interfaces whose type sets can be defined entirely by a list of methods are called `basic interfaces`.

For convenience, the pre-declared type `any` is an alias for the empty interface.

#### Embedded Interface
In a slightly more general form an interface `T` may use a (possibly qualified) interface type name `E` as an interface element. This is called `embedding interface` `E` in `T`.

```
type Reader interface {
	Read(p []byte) (n int, err error)
	Close() error
}

type Writer interface {
	Write(p []byte) (n int, err error)
	Close() error
}

// ReadWriter's methods are Read, Write, and Close.
type ReadWriter interface {
	Reader  // includes methods of Reader in ReadWriter's method set
	Writer  // includes methods of Writer in ReadWriter's method set
}
```

When embedding interfaces, methods with the [same](https://go.dev/ref/spec#Uniqueness_of_identifiers) names must have [identical](https://go.dev/ref/spec#Type_identity) signatures.

#### General Interface
In their most general form, an interface element may also be an arbitrary type term `T`, or a term of the form `~T` specifying the underlying type `T`, or a union of terms `t1|t2|…|tn`. Together with method specifications, these elements enable the precise definition of an interface's type set as follows:

- The type set of the empty interface is the set of all non-interface types.
- The type set of a non-empty interface is the intersection of the type sets of its interface elements.
- The type set of a method specification is the set of all non-interface types whose method sets include that method.
- The type set of a non-interface type term is the set consisting of just that type.
- The type set of a term of the form `~T` is the set of all types whose underlying type is `T`.
- The type set of a _union_ of terms `t1|t2|…|tn` is the union of the type sets of the terms.

In a term of the form `~T`, the underlying type of `T` must be itself, and `T` cannot be an interface.
```
type MyInt int

interface {
	~[]byte  // the underlying type of []byte is itself
	~MyInt   // illegal: the underlying type of MyInt is not MyInt
	~error   // illegal: error is an interface
}
```

The type `T` in a term of the form `T` or `~T` cannot be a [type parameter](https://go.dev/ref/spec#Type_parameter_declarations), and the type sets of all non-interface terms must be pairwise disjoint (the pairwise intersection of the type sets must be empty). Given a type parameter `P`:

```
interface {
	P                // illegal: P is a type parameter
	int | ~P         // illegal: P is a type parameter
	~int | MyInt     // illegal: the type sets for ~int and MyInt are not disjoint (~int includes MyInt)
	float32 | Float  // overlapping type sets but Float is an interface
}
```

--- 

```
type m0 interface{ int }
type m1 interface {
	int
	m()
}
type m2 interface{ m() }

func foobarz1[T m0](a T, b T) {}
func foobarz2[T m1](a T, b T) {}
func foobarz3[T m2](a T, b T) {}

func foobar123() {
	var _ m0 // cannot use type m0 outisde type constraint: interface contains type constraints
	var _ m1 // cannot use type m1 outisde type constraint: interface contains type constraints
	var _ m2
}
```


---
## Good practices
We can always validate if we indeed implement an interface by
`var x MyInterface = ImplementationStruct{}`

Also note that this is considered a good practice according to uber guidelines, its good practice to do _Interface Checks_
```
struct MyImplementation{...}

var _ somePackage.interface = MyImplementation{}
```

Ref:
* https://go.dev/doc/effective_go#blank_implements
- https://github.com/uber-go/guide/blob/master/style.md#verify-interface-compliance

--- 
## Questions
### Do I have to implement all methods? What if I don't?
Then we do not implement that interface.

### What is a `type set`?
Collection of types that satisfy given interface.

```
type Animal interface { Sound() string }
type Dog struct {}
func (d Dog) Soound() string { return "Woof!" }
type Cat struct {}
func (c Cat) Soound() string { return "Meow!" }
```

Type set of `Animal` is [`Dog`, `Cat`].

### Why implicit interfaces? Benefits over explicit?
Pros:
-  Separation of concerns - we can fulfil different interfaces in separation from one another (`Flyer`, `Walker`, `Swimmer`, `Quacker`)
- Write interface incorporating types from standard library, without changing them
- No need for wrappers

Cons:
* Accidentally implement interface, changing how our type interacts with 3rd party module
```
struct Person{
	firstName, lastName string
}

func (p Person) String() string{
	return "Hello " + p.firstName + " " + p.lastName
}

func main() {
	p := Person{"Jon", "Smith"}
	fmt.Println(p) // we expect {"Jon", "Smith"}
				   // but receive "Hello Jon Smith"
}
```
Note that in order this to happen you'd need to implement interface AND pass your type to a method that would use it

* when reading package - it may be unclear why given type implements given method ( if we don't know it's required by interface )
```
// implement for validation.Validatable interface
func (c Costumer) Validate() error { ... }
```