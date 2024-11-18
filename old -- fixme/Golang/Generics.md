
---
## Refs
- https://go.dev/ref/spec#Type_parameter_declarations

---

## Desc
### Type parameters

```
TypeParameters  = "[" TypeParamList [ "," ] "]" .
TypeParamList   = TypeParamDecl { "," TypeParamDecl } .
TypeParamDecl   = IdentifierList TypeConstraint .
```

Each name declares a type parameter, which is a new and different [named type](https://go.dev/ref/spec#Types) that acts as a placeholder for an (as of yet) unknown type in the declaration. The type parameter is replaced with a _type argument_ upon [instantiation](https://go.dev/ref/spec#Instantiations) of the generic function or type.

```
[P any]
[S interface{ ~[]byte|string }]
[S ~[]E, E any]
[P Constraint[int]]
[_ any]
```

Just as each ordinary function parameter has a parameter type, each type parameter has a corresponding (meta-)type which is called its [_type constraint_](https://go.dev/ref/spec#Type_constraints).

## Type Constraint
A type constraint is an interface that defines the set of permissible type arguments for the respective type parameter and controls the operations supported by values of that type parameter.

If the constraint is an interface literal of the form `interface{E}` where `E` is an embedded [type element](https://go.dev/ref/spec#Interface_types) (not a method), in a type parameter list the enclosing `interface{ … }` may be omitted for convenience:

```
[T []P]                      // = [T interface{[]P}]
[T ~int]                     // = [T interface{~int}]
[T int|string]               // = [T interface{int|string}]
```

### Type Argument

A type argument T satisfies a type constraint C if T is an element of the type set defined by C

```
type argument      type constraint                // constraint satisfaction

int                interface{ ~int }              // satisfied: int implements interface{ ~int }
string             comparable                     // satisfied: string implements comparable (string is strictly comparable)
[]byte             comparable                     // not satisfied: slices are not comparable
any                interface{ comparable; int }   // not satisfied: any does not implement interface{ int }
any                comparable                     // satisfied: any is comparable and implements the basic interface any
struct{f any}      comparable                     // satisfied: struct{f any} is comparable and implements the basic interface any
any                interface{ comparable; m() }   // not satisfied: any does not implement the basic interface interface{ m() }
interface{ m() }   interface{ comparable; m() }   // satisfied: interface{ m() } is comparable and implements the basic interface interface{ m() }
```

### Instantiations
A generic function or type is instantiated by substituting type arguments for the type parameters. Instantiation proceeds in two steps:

Each type argument is substituted for its corresponding type parameter in the generic declaration. This substitution happens across the entire function or type declaration, including the type parameter list itself and any types in that list.
After substitution, each type argument must satisfy the constraint (instantiated, if necessary) of the corresponding type parameter. Otherwise instantiation fails.

```
type parameter list    type arguments    after substitution

[P any]                int               int satisfies any
[S ~[]E, E any]        []int, int        []int satisfies ~[]int, int satisfies any
[P io.Writer]          string            illegal: string doesn't satisfy io.Writer
[P comparable]         any               any satisfies (but does not implement) comparable

```

---

## Questions
