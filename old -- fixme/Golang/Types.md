
---

## Refs

---

## Desc


### General
```

Interface value consists of concrete values and _dynamic_ type: [Value, Type]

STATIC TYPE
DYNAMIC TYPE

type I interface{ F() }

type X Struct{}
func (X) F() {}

type Y Struct{}
func (X) F() {}

var i I
i = X{}

// static type is I
// dynamic type is X (and can be reassigned to Y)

We can check Dynamic Type of variable by TYPE ASSERTION or TYPE SWITCHES
Type Assertion
x.(T)

```

## What is `any`?
`any` is a `type` which could be represented as empty interface `interface{}`
Which means it is implemented by every `interface`

### Underlying Type
Each type T has an underlying type: If T is one of the pre-declared boolean, numeric, or string types, or a type literal, the corresponding underlying type is T itself. Otherwise, T's underlying type is the underlying type of the type to which T refers in its declaration. For a type parameter that is the underlying type of its type constraint, which is always an interface.

```
type (
	A1 = string
	A2 = A1
)

type (
	B1 string
	B2 B1
	B3 []B1
	B4 B3
)

func f[P any](x P) { … }
```

The underlying type of `string`, `A1`, `A2`, `B1`, and `B2` is `string`. The underlying type of `[]B1`, `B3`, and `B4` is `[]B1`. The underlying type of `P` is `interface{}`.

### Core Type
Each non-interface type T has a core type, which is the same as the underlying type of T.

---