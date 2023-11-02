---
layout: post
title: Apprendre rust par l'exemple
tags: dev rust
---

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

```rust
let tupple = (1, 4, 7);

match tupple {
    (1, ..) => {},
    (x, 1, ..) => {},
    (1, .., 4) => {},
    (1, _, 4) => {},
    _ => {},
}
```

#### 8.5.1.2 Arrays/slices

```rust
let array = [1, 4, 7];

match array {
    [first, middle @ .., last] => println!("{}{:?}{}", first, middle, last),
}
```

#### 8.5.1.3 Arrays/Slices

```rust
enum Enum {
    rgb(i32, i32, i32)
}

match enum {
    enum::rgb(r, g, b) => ;
}
```

#### 8.5.1.4 Pointer/Ref

```rust
// Assign a reference of type `i32`. The `&` signifies there
// is a reference being assigned.
let reference = &4;
match reference {
    &val => println!("{}", &val),
}

// left value is ref so not_ref is referenced now
let not_ref = 4;
match not_ref {
    ref val => println!("{}", val),

}

let mut not_ref_mut = 4;
match not_ref_mut {

    ref mut val => {
        *val += 10;
        println!("{}", val);
    },
}
```

#### 8.5.1.4 Structs

```rust
struct Foo {
    x: (u32, u32),
    y: u32,
}

let foo = Foo{ x: 1, y: 2};
match foo {
    Foo{ x: (1, b), y} => println!("First of x is 1, b = {},  y = {} ", b, y),
    // the order is not important
    Foo{ y, x: (1, b)} => println!("First of x is 1, b = {},  y = {} ", b, y),
    Foo { y, .. } => println!("y = {}, we don't care about x", y),

}
```

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

### 8.5.3 Binding
/!\ NOT USEFULL

```rust
fn age() -> i32 {
    15
}

match age() {
    n @ 1..18 => println!("Je suis un mineur agé de {} ans"),
    n => println!("Je suis un majeur agé de {} ans"),
}

fn result() -> Option<u32> {
    Some(42)
}
match result() {
    Some(u @ 42) => println!("OMG IT IS {}", n),
    Some(u) => println!("{} is not 42...", n);
}
```

## 8.6 If let
Match is akward some times
```rust
//want to destructure 
let some = Some(5)

match some {
    Some(i) => println!("{}", i),
    _ => {},
}

if let Some(i) = some {
    println!("{}", i);
} else if ... {
    println!("FAIL !");
} else {
    println!("FAIL !");
}
```

necessary to comapre enums parameters
```rust
enum Foo {Bar}

let a = Foo::Bar
if let Foo::Bar = a {
    ...
}

//doesnt work !
if a == Foo::Bar {

}
```

## 8.7 Let-else

match patern and bind values in surrounding scope or diverge (panic, break, continue... )
nicer than a match

```rust
fn get_count_item(s: &str) -> (u64, &str) {
    let mut it = s.split(' ');
    let (Some(count_str), Some(item)) = (it.next(), it.next()) else {
        panic!("Can't segment count item pair: '{s}'");
    };
    let Ok(count) = u64::from_str(count_str) else {
        panic!("Can't parse integer: '{count_str}'");
    };
    (count, item)
}
```

## 8.8 while let

nicer than a match

```rust
let some = Some(0);

while let Some(i) = some {
    if( i < 9 ) {
        some = Some(i+1);
    } else {
        some = None;
    }
}
```

# 9. Functions

```rust
//return n+5
fn lol(n: i32) -> i32 {
    n + 5
}

fn lil(n: i32) -> i32 {
    return n + 5;
    n //never reached
}

fn lul(n: i32) {
    println!("{}", n);
} // actually return ()
```

# 9.1 Methods

Some functions are associated to a type :
- associated function, just associated with a type
- method, associated with a particular instance of a type. A reference to the instance is invisibly passed in function parameters.

***box*** stores data in heap with Box::new(value)

```rust
struct Point {
    x: f64,
    y: f64,
}

// implementator block
impl Point {
    // not linked with an instance, can be called at any time
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    },

    fn print(&self) {
        println!("x: {}, y: {}", self.x, self.y);
    }

    // &mut self dessugars to self: &mut Self
    fn changeX(&mut self, new_x: f64) {
        self.x = new_x;
    }
}

let point = Point::origin();
point.changeX(5.0);
point.print();

struct Pair(Box<i32>, Box<i32>); // tupple like pair

impl Pair {
    destruct(&self) {
        let Pair(fist, second) = self;
        println!("{}{}", first, second);
        // first and second go out of scope and pair became unusable
    }
}
```

# 9.2 Closures 

***closure*** function that captures the enclosing environment
- lighter synthax than function
- both return and args types inferred
- no closure {} needed

```rust
fn cuty() -> i32 {
    let x = 1;
    let closure_annotated = |i: i32| -> i32 { i + outer_var };
    let closure_inferred  = |i     |          i + outer_var  ;

    println!("closure_annotated: {}", closure_annotated(1));
}
```

# 9.2.1 Capturing

***move*** force closure to take ownership of captured variable

```rust
let color = String::from("red");

let print = || println!("{}", color);
print();

let mur count = 0;
let mut inc = || count += 1;
// let reborrow = &count; error cause inc() owns mut ref to count and is called later
inc();
let reborrow = &count; // now possible cause inc() not called anymore

let mut pretty = move || counr += 42;
pretty();
// let reborrow = &count; error cause count has been moved
```

# 9.2.3

Using closures as fn parameter requiring generics (traits: `Fn`, `FnMut`, or `FnOnce`) which dictate how a closure captures variables from the enclosing scope.

```rust
// `F` must implement `Fn` for a closure which takes no
// inputs and returns nothing - exactly what is required
// for `print`.
fn apply<F>(f: F) where
    F: Fn() {
    f();
}

let x = 7;

// Capture `x` into an anonymous type and implement
// `Fn` for it. Store it in `print`.
let print = || println!("{}", x);

apply(print);
```

# 9.2.4 Input Function

Functions can also be used as input using generic traits in fn signature

# 9.2.5 As output parameters

```rust
fn create_fn () -> impl Fn {
    let text = "Fn";
    // move keyword required cause references would be dropped after fn scope ending
    move || println!("{}", text)
}
```

# 9.2.6  Examples in std

# 9.2.6.1 Iterator::any


# 9.2.6.2 Searching through iterators

# 9.3 Higher order Function (HOF)

Rust provides Higher Order Functions (HOF). These are functions that take one or more functions and/or produce a more useful function. HOFs and lazy iterators give Rust its functional flavor.

```rust
fn is_odd(n: u32) -> bool {
    n % 2 == 1
}

fn main() {
    println!("Find the sum of all the squared odd numbers under 1000");
    let upper = 1000;

    // Imperative approach
    // Declare accumulator variable
    let mut acc = 0;
    // Iterate: 0, 1, 2, ... to infinity
    for n in 0.. {
        // Square the number
        let n_squared = n * n;

        if n_squared >= upper {
            // Break loop if exceeded the upper limit
            break;
        } else if is_odd(n_squared) {
            // Accumulate value, if it's odd
            acc += n_squared;
        }
    }
    println!("imperative style: {}", acc);

    // Functional approach
    let sum_of_squared_odd_numbers: u32 =
        (0..).map(|n| n * n)                             // All natural numbers squared
             .take_while(|&n_squared| n_squared < upper) // Below upper limit
             .filter(|&n_squared| is_odd(n_squared))     // That are odd
             .sum();                                     // Sum them
    println!("functional style: {}", sum_of_squared_odd_numbers);
}
```

# 9.4 Diverging Functions

It never returns !

```rust
#![feature(never_type)]

fn faa() {
    println!("");
}
let x = faa();

let x: ! = panic!("This call never returns.");
println!("You will never see this line!");

// continue return ! type, like exit() ot forever loop{} function
let mut acc = 0;
for i in 0..1024 {
    // Notice that the return type of this match expression must be u32
    // because of the type of the "addition" variable.
    let addition: u32 = match i%2 == 1 {
        true => i,
        // On the other hand, the "continue" expression does not return
        // u32, but it is still fine, because it never returns and therefore
        // does not violate the type requirements of the match expression.
        false => continue,
    };
    acc += addition;
}
```