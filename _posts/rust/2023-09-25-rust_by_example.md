# Intro
 
Vous trouverez ici mon "résumé", ou plutôt la liste des différntes notions à retenir dans chaque chapitre du [rust by example](https://doc.rust-lang.org/stable/rust-by-example/). Chaque élément en italique et gras est une de ces notions.

# Sommaire

# 2. Primitives

***Scalar Types***
- Signed integers: i8, i16, i32, i64, i128 and isize (pointer size)
- Unsigned integers: u8, u16, u32, u64, u128 and usize (pointer size)
- Floating point: f32, f64
- char Unicode scalar values like 'a', 'α' and '∞' (4 bytes each)
- bool either true or false
- The unit type (), whose only possible value is an empty tuple: ()

***Compound Types***
- Arrays like [1, 2, 3]
- Tuples like (1, true)

***Inferrence***

***Shadowing***

***Mutable***

***Byte*** 
- est la plus petite unité de donnée allouable par un ordinateur
- pas un ***Bit !***
- constitué d'un certains nombre de bits (très couramment 8, soir un octet)

***64 bits processor*** 
- un processeur qui peut créer 2e64 bytes(souvent octet) adresses mémoires
- tous les processurs modernes le sont car 2e32 n'était pas un assez grand nombre

## 2.1 Litterals and operators

***Bitwise operation***
- ">>" permet de décaler tous les bits vers la droite

## 2.2 Tuples

***Tuples***
- collection
- different types
- used to return multiple values in fn
- signature: `(T1, T2, ...)`
```rust
// Destructure a tuple
let (int_param, bool_param) = pair;

// To create one element tuples, comma or it's just an integer
println!("One element tuple: {:?}", (5u32,));
```

***indexing***
```rust
let pair = (1, 2);
let snd = pair.1;
```


## 2.3 Arrays and slices

***Arrays***
- collection
- same types
- stored in contigues memory in stack
- signature : `[T; length]`

**Slices***
- signature: `&[T]` and ahs a pointer and a length
- to borrow a section of an array

# 3. Custom Types

- struct
- enum
- const/static

## 3.1 Structures

- Tuple structs, which are, basically, named tuples, for generics
- The classic C structs
- Unit structs, which are field-less, are useful for generics

```rust
let point: Point = Point { x: 10.3, y: 0.4 };

let bottom_right = Point { x: 5.2, ..point }; //auto-fill

// Destructure the point using a `let` binding
let Point { x: left_edge, y: top_edge } = point;
```

## 3.2 Enums

***enum***
```rust
enum WebEvent {
    PageLoad,
    Paste(String),
    Click { x: i64, y: i64 },
}
```

***casting***
```rust
enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}

println!("roses are #{:06x}", Color::Red as i32); //casting
```

***use***
```rust
enum Status {
    Rich,
    Poor,
}

fn main() {

    match status {
        // Note the lack of scoping because of the explicit `use` above.
        Rich => println!("The rich have lots of money!"),
        Poor => println!("The poor have no money..."),
    }
}
```

## 3.3 Constants

- const: An unchangeable value (the common case).
- static: A possibly mutable variable with 'static lifetime. The static lifetime is inferred and does not have to be specified. Accessing or modifying a mutable static variable is unsafe

# 4. Variable Bindings

***Variable Bindings***
- bind a value to a variable

***Type inference***
- compiler guess type of a value, no need to annotate/cast 

***_variable***
- no unutilized warning for variable begining by *_*

## 4.1 Mutability

***Mutable***
- all variable are immutable by default with *let binding*, *let mut* is needed to make it mutable

## 4.2 Scope and Shadowing

***shadowing***
- immutable variables can be shadowed and attributes another value
```rust
let value = 0;
let value = "uwu"
```

***scope***
- block of code containing some variables and code
- inner variables of a scope have a lifetime equal to their scope ending by default

## 4.3 Declare first

***declaration***
- name a variable
- can be done without *initialisation*, but compil error if never initialized
- *declaration first* is seldom because risky

***initialization***
- set value/type of a declared variable

## 4.4 Freezing

***freezing***
- cancel mutability of a variable by using :
```rust
let mut _mutable_integer = 7i32;
{
    let _mutable_integer = _mutable_integer;
}
```

# 5. Types

## 5.1 Casting

***coercion***
- unexplicit type conversion, opposit to casting

***casting***
- for primitive types and cost free : `borrowed -> borrowed`
```rust
let owo = 1;
let uwu = owo as f64;
```

## 5.2 Litterals

***type annotated*** if not, type depend context
```rust
let x = 1u8;
let y = 2u32;
let z = 3f32;
```

## 5.3 Inference
***inference engine***
```rust
let elem = 5u8;
let mut vec = Vec::new();

//OMG ! u8 vec !
vec.push(elem);
```

## 5.4 Aliasing

***aliasing***
```rust
type NanoSecond = u64;
type Inch = u64;
type U64 = u64;
// Inch, NanoSecond, U64, u64 are SAME types
```

# 6. Conversion

Conversion for custom types use ***From*** and ***Into*** ***traits*** in general, but there are others.

## 6.1 From and Into

***From***
```rust
let my_str = "hello";
let my_string = String::from(my_str);

struct Number {
    val: i32;
}

impl From<i32> for Number { //why From<i32> for ? Cause necessary to use Into
    fn from(num: i32) -> Self {
        Number { val: num }
    }
}
```

***Into***
```rust
let num: Number = int.into(); //into always implemented when from is

//impl into from foreign types (not your crate's types), here imagine Number is not created by u
impl Into<Number> for i32 {
    fn into(self) -> Number {
        Number { value: self }
    }
}
```

## 6.2 TryFrom and TryInto

- like ***into*** and ***from***
- return ***Result***

```rust
struct EvenNumber(i32);

impl TryFrom<i32> for EvenNumber {
    type Error = ();

    fn try_from(value: i32) -> Result<Self, Self::Error> {
        if value % 2 == 0 {
            Ok(EvenNumber(value))
        } else {
            Err(())
        }
    }
}
```

## 6.3 To and Strings

```rust
struct Circle {
    radius: i32
}

impl fmt::Display for Circle {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Circle of radius {}", self.radius)
    }
}

fn main() {
    let circle = Circle { radius: 6 };
    println!("{}", circle.to_string());
}
```

***parsing*** string into another type
- ***FromStr*** implemented on this type
- idiomatic  : parse
- arrange type : turbofish synthax
```rust
let parsed: i32 = "5".parse().unwrap();
let turbo_parsed = "10".parse::<i32>().unwrap();
```

# 7. Expresssions

Rust program = (mostly) series of statments :
```rust
fn main() {
    // statement
    // statement
    // statement
}

fn main() {
    // variable binding
    let x = 5;

    // expression;
    x;
    x + 1;
    15;
}
```

function are also statement
```rust
// assign x to 2
let x = {
    1 + 1
}

// assign x to ()
let x = {
    1 + 1;
}
```

# 8. Flow of Control

Lets modify control flow !

## 8.1 if/else

```rust
if n < 0 {
    print!("{} is negative", n);
} else if n > 0 {
    print!("{} is positive", n);
} else {
    print!("{} is zero", n);
}

let big_n =
    if n < 10 && n > -10 {
        println!(", and is a small number, increase ten-fold");
        10 * n
    };
```

## 8.2 loop
- ***break;*** to exit loop
- ***continue;*** skip ***iteration*** and start new one

### 8.2.1 Nesting and Labels
```rust
'outer loop {
    println!("first");

    'inner loop {
        println!("second");
        break 'outer;
        continue 'outer; // skiped by previouss break
    }

    println!("never");
}
```

### 8.2.2 returning from loops

- loop can be used to retry an expression until it succeed and returns a value
```rust
let mut counter = 0;
let i = loop {
    counter += 1;

    if(counter == 10) {
        break 4;
    }

} // i = 4 !!
```

# 8.3 while 

```rust
let mut counter = 0;

while counter < 10 {
    counter += 1;
    println!("{}", counter);
}
```

# 8.4 for and range

- ***for in*** iterates through a ***iterator***
- a..b (range notation) create an iterator, b excluded
- a..=b b included

```rust
for i in 1..5 {
    println!("{}", i);
}
```

- convert some collections to iterators with ***.into_iter()*** by default uwu

***.iter()*** borrow each element of iterator
***.into_iter()*** consumes collection
***.iter_mut()*** borrow mut each element of collection to allow modif
```rust
let names = vec!["Bob", "Frank", "Ferris"];

for name in names.iter() {
    match name {
        &"Ferris" => println!("There is a rustacean among us!"),
        _ => println!("Hello {}", name),
    }
}

for name in names.into_iter() {
    match name {
        "Ferris" => println!("There is a rustacean among us!"),
        _ => println!("Hello {}", name),
    }
}

for name in names.iter_mut() {
    *name = match {
        &mut "Ferris" => println!("There is a rustacean among us!"),
        _ => println!("Hello {}", name),
    }
}
```

## 8.5 match

patern matching with ***match** keyword (switch in c)
```rust
let number = 13;
match number {
    1 => println!("One!"),
    2 | 3 | 5 | 7 | 11 => println!("This is a prime"),
    13..=19 => println!("A teen"),
    _ => println!("Ain't special"),
}
```

### 8.5.1 Descructing

***match*** can ***deconstruct*** items in a variety of ways

#### 8.5.1.1 Tuples



### 8.5.2 Guards

***match guard*** allow condition inside match
```rust
#[allow(dead_code)]
enum Temperature {
    Celsius(i32),
    Fahrenheit(i32),
}

fn main() {
    let temperature = Temperature::Celsius(35);

    match temperature {
        Temperature::Celsius(t) if t > 30 => println!("{}C is above 30 Celsius", t),
        // The `if condition` part ^ is a guard
        Temperature::Celsius(t) => println!("{}C is below 30 Celsius", t),

        Temperature::Fahrenheit(t) if t > 86 => println!("{}F is above 86 Fahrenheit", t),
        Temperature::Fahrenheit(t) => println!("{}F is below 86 Fahrenheit", t),
    }
}
```
