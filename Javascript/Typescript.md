### Types
```
number
string
boolean
null

Array<T> or T[]
Array<string> ; string[]

//tuple
let t: [string, number] = "1", 1

//enum
enum Color {
	Red,
	Green, 
	Blue
}

let color: Color = Color.Red

// objects
interface User {
	firstName: string,
	lastName: string,
}

let u: User = {firstName: "Foo", lastName: "Bar"}

// intersection types
interface Drivable { drive(): void; } 
interface Flyable { fly(): void; } 
type FlyingCar = Drivable & Flyable;

// union types
let score: number | string;
score = 1
score = "1"

// literal types
let direction: "top" | "left" | .. ;
direction = "top"

//any
let data: any = "foobar";
data = 42;
```

```
let age: number = 25;
```