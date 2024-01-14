---
layout: post
title: Apprendre rust par l'exemple
tags: dev rust
---

# Intro
 
Vous trouverez ici mon "résumé", ou plutôt la liste des différentes notions à retenir chapitre par chapitre du [rust by example](https://doc.rust-lang.org/stable/rust-by-example/). Chaque élément en italique et gras est une de ces notions.

# Sommaire

# 1.

porco rosso, je pleure, *bang bang*

# 2. Primitives

***Scalar Types***
- Signed integers: i8, i16, i32, i64, i128 and isize (pointer size)
- Unsigned integers: u8, u16, u32, u64, u128 and usize (pointer size)
- Floating point: f32, f64
- char Unicode scalar values like 'a', 'α' and '∞' (4 bytes each)
- bool either true or false
- The unit type (), whose only possible value is an empty tuple: ()

---

***Compound Types***
- Arrays like [1, 2, 3]
- Tuples like (1, true)

---

- explicit type conversion
- as "keyword"

---

***saturation cast***
```rust
300.0 as u8 == 255
```

---

***litterals***
```rust
let x = 1u8;
let y = 2u32;
let z = 3f32;

//how they are used (def: i32/f64)
let i = 1;
let f = 1.0;
```

***std::mem::size_of_val(&x)*** return variable's size in bytes

---

***inference***
```rust
    let elem = 5u8;
    let mut vec = Vec::new();
    // compiler doesn't know the exact type of `vec`
    vec.push(elem);
```

---

 ***aliasing***

---

***Shadowing***

---

***Mutable***

---

***Byte*** 
- est la plus petite unité de donnée allouable par un ordinateur
- pas un ***Bit !***
- constitué d'un certains nombre de bits (très couramment 8, soit un octet)

---

***64 bits processor*** 
- un processeur qui peut créer 2e64 bytes(souvent octet) adresses mémoires
- tous les processurs modernes le sont car 2e32 n'était pas un assez grand nombre

# 2.1 Litterals and operators

***Bitwise operation***
- ">>" permet de décaler tous les bits vers la droite

# 2.2 Tuples

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

---

***indexing***
```rust
let pair = (1, 2);
let snd = pair.1;
```


# 2.3 Arrays and slices

***Arrays***
- collection
- same types
- stored in contigues memory in stack
- signature : `[T; length]`

---

***Slices***
- signature: `&[T]` and has a pointer and a length
- to borrow a section of an array

# 3. Custom Types

- struct
- enum
- const/static

# 3.1 Structures

- Tuple structs, which are, basically, named tuples, for generics
- The classic C structs
- Unit structs, which are field-less, are useful for generics

```rust
let point: Point = Point { x: 10.3, y: 0.4 };

let bottom_right = Point { x: 5.2, ..point }; //auto-fill

// Destructure the point using a `let` binding
let Point { x: left_edge, y: top_edge } = point;
```

# 3.2 Enums

***enum***
```rust
enum WebEvent {
    PageLoad,
    Paste(String),
    Click { x: i64, y: i64 },
}
```

---

***casting***
```rust
enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}

println!("roses are #{:06x}", Color::Red as i32); //casting
```

---

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

# 3.3 Constants

- const: An unchangeable value (the common case).
- static: A possibly mutable variable with 'static lifetime. The static lifetime is inferred and does not have to be specified. Accessing or modifying a mutable static variable is unsafe

# 4. Variable Bindings

***Variable Bindings***
- bind a value to a variable

---

***Type inference***
- compiler guess type of a value, no need to annotate/cast

---

***_variable***
- no unutilized warning for variable begining by *_*

# 4.1 Mutability

***Mutable***
- all variable are immutable by default with *let binding*, *let mut* is needed to make it mutable

# 4.2 Scope and Shadowing

***shadowing***
- immutable variables can be shadowed and attributes another value
```rust
let value = 0;
let value = "uwu"
```

---

***scope***
- block of code containing some variables and code
- inner variables of a scope have a lifetime equal to their scope ending by default

# 4.3 Declare first

***declaration***
- name a variable
- can be done without *initialisation*, but compil error if never initialized
- *declaration first* is seldom because risky

---

***initialization***
- set value/type of a declared variable

# 4.4 Freezing

***freezing***
- cancel mutability of a variable by using :
```rust
let mut _mutable_integer = 7i32;
{
    let _mutable_integer = _mutable_integer;
}
```

# 5. Types

# 5.1 Casting

***coercion***
- unexplicit type conversion, opposit to casting

---

***casting***
- for primitive types and cost free : `borrowed -> borrowed`
```rust
let owo = 1;
let uwu = owo as f64;
```

# 5.2 Litterals

***type annotated*** if not, type depend context
```rust
let x = 1u8;
let y = 2u32;
let z = 3f32;
```

# 5.3 Inference
***inference engine***
```rust
let elem = 5u8;
let mut vec = Vec::new();

//OMG ! u8 vec !
vec.push(elem);
```

# 5.4 Aliasing

***aliasing***
```rust
type NanoSecond = u64;
type Inch = u64;
type U64 = u64;
// Inch, NanoSecond, U64, u64 are SAME types
```

# 6. Conversion

Conversion for custom types use ***From*** and ***Into*** ***traits*** in general, but there are others.

# 6.1 From and Into

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

# 6.2 TryFrom and TryInto

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

# 6.3 To and Strings

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

# 8.1 if/else

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

# 8.2 loop
- ***break;*** to exit loop
- ***continue;*** skip ***iteration*** and start new one

## 8.2.1 Nesting and Labels
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

## 8.2.2 returning from loops

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

for _ in 1..5 {
    println!("{}", "owo !");
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

# 8.5 match

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

## 8.5.1 Descructing

***match*** can ***deconstruct*** items in a variety of ways

### 8.5.1.1 Tuples

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

### 8.5.1.2 Arrays/slices

```rust
let array = [1, 4, 7];

match array {
    [first, middle @ .., last] => println!("{}{:?}{}", first, middle, last),
}
```

### 8.5.1.3 Arrays/Slices

```rust
enum Enum {
    rgb(i32, i32, i32)
}

match enum {
    enum::rgb(r, g, b) => ;
}
```

### 8.5.1.4 Pointer/Ref

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

### 8.5.1.4 Structs

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

## 8.5.2 Guards

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

## 8.5.3 Binding
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

# 8.6 If let
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

# 8.7 Let-else

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

# 8.8 while let

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
- `Fn` for closures that capture immutable variables
- `FnMut` atmost for closures that capture mutable variables
- `FnOnce` atmost for closures that consumme variables

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

```rust
fn find<P>(&mut self, predicator: P) -> Option<Self::Item> where
P: FnMut(&Self::Item) -> bool;
```

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

# 10. Modules

# 10.1 Visibility

- private visibility in module by default

```rust
// ancestor/paretn module only
pub(crate::my_mod)
pub(super)
pub(crate)
```

# 10.2 Struct visibility

- struct fields private by default

```rust
struct ello<T> {
    pub sir: T,
}
```

# 10.3 The use declaration

```rust
use crate::deeply::nested::{
    my_first_function,
    my_second_function,
    AndATraitType
};

use crate::ello::world as ello_world;
```

# 10.4 super and self

```rust
super::function();
self::function();
```

# 10.5 File hierarchy

... je suis dubitatif ...

# 11. Crates

# 11.1 Creating a Library

```
$ rustc --crate-type=lib rary.rs
$ ls lib*
library.rlib
```

# 11.2 Using a Library

```
$ rustc executable.rs --extern rary=library.rlib && ./executable 
```

# 12. Cargo

- dependency management and integration
- awareness of unit test
- awareness of benchmarks

***benchmarks*** are a lot of test to determine an aarchitecture/hardware performance

# 12.1 Dependencies

```
# A binary
cargo new foo

# A library
cargo new --lib bar
```

***semantic versionning***

```toml
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
clap = "2.27.1" # from crates.io
rand = { git = "https://github.com/rust-lang-nursery/rand" } # from online repo
bar = { path = "../bar" } # from a path in the local filesystem
```

```
$ cargo build
$ cargo run
```

# 12.2 Conventions

```
.
├── Cargo.lock // maintained by user
├── Cargo.toml // maintained by cargo
├── src/
│   ├── lib.rs // default lib
│   ├── main.rs // default binary
│   └── bin/ // other binary files
│       ├── named-executable.rs
│       ├── another-executable.rs
│       └── multi-file-executable/
│           ├── main.rs
│           └── some_module.rs
├── benches/
│   ├── large-input.rs
│   └── multi-file-bench/
│       ├── main.rs
│       └── bench_module.rs
├── examples/
│   ├── simple.rs
│   └── multi-file-example/
│       ├── main.rs
│       └── ex_module.rs
└── tests/ // each file is an integration test
    ├── some-integration-tests.rs
    └── multi-file-test/
        ├── main.rs
        └── test_module.rs
```

- bin/  use cargo build --bin my_other_bin to only build this binary

# 12.3 Testing

```
$ cargo test
// will test all tests with "uwu", ex "uwu", "uwu-owo"...
$ cargo test uwu
```

Warning: two tets can run concurently. ex : do not output tests to a same file

# 12.4 Build Scripts

- can be userrd to fullfil build pre-requisites
- will look for `build.rs` file by default

```toml
[package]
...
build = "build.rs"
```

# 13. Attributes

- `#[outer_attribute]` to the item directly following it
- `#![inner_attribute]` apply to the whole scope

```rust
// applies to the whole crate here
#![allow(unused_variables)]

fn main() {
    let x = 3; // This would normally warn about an unused variable.
}
```

# 13.1 dead_code

***!lint*** is static code analysis to flag programming errors, stylistic errors, etc... ex : dead_code lint

- #[allow(dead_code)] for unused fn in exemples

# 13.2 Crates

```rust
// This crate is a library
#![crate_type = "lib"]
// The library is named "rary"
#![crate_name = "rary"]
```
*useless* since doesn't work with cargo

# 13.3 cfg

```rust
// This function only gets compiled if the target OS is linux
#[cfg(target_os = "linux")]
fn are_you_on_linux() {
    println!("You are running linux!");
}

// And this function only gets compiled if the target OS is *not* linux
#[cfg(not(target_os = "linux"))]
fn are_you_on_linux() {
    println!("You are *not* running linux!");
}

fn main() {
    are_you_on_linux();

    println!("Are you sure?");
    if cfg!(target_os = "linux") {
        println!("Yes. It's definitely linux!");
    } else {
        println!("Yes. It's definitely *not* linux!");
    }
}
```

## 13.3.1 Custom

```rust
#[cfg(some_condition)]
fn conditional_function() {
    println!("condition met!");
}
```

```
$ rustc --cfg some_condition custom.rs && ./custom
condition met!
```

# 14. Generics

```rust
// A concrete type `A`.
struct A;

// In defining the type `Single`, the first use of `A` is not preceded by `<A>`.
// Therefore, `Single` is a concrete type, and `A` is defined as above.
struct Single(A);
//            ^ Here is `Single`s first use of the type `A`.

// Here, `<T>` precedes the first use of `T`, so `SingleGen` is a generic type.
// Because the type parameter `T` is generic, it could be anything, including
// the concrete type `A` defined at the top.
struct SingleGen<T>(T);

fn main() {
    // `Single` is concrete and explicitly takes `A`.
    let _s = Single(A);
    
    // Create a variable `_char` of type `SingleGen<char>`
    // and give it the value `SingleGen('a')`.
    // Here, `SingleGen` has a type parameter explicitly specified.
    let _char: SingleGen<char> = SingleGen('a');

    // `SingleGen` can also have a type parameter implicitly specified:
    let _t    = SingleGen(A); // Uses `A` defined at the top.
    let _i32  = SingleGen(6); // Uses `i32`.
    let _char = SingleGen('a'); // Uses `char`.
}
```

# 14.1 Functions

```rust
struct SGen<T>(T); // Generic type `SGen`.

fn gen<T>(_what: SGen<T>) {}

// explicitly specified type parameter to gen
gen::<char>(SGen('a'));

// implicitly specified type parameter to gen
gen(SGen('a'));
```

# 14.2 Implementation

```rust
struct S; // Concrete type `S`
struct GenericVal<T>(T); // Generic type `GenericVal`

// impl of GenericVal where we explicitly specify type parameters:
impl GenericVal<f32> {} // Specify `f32`
impl GenericVal<S> {} // Specify `S` as defined above

// `<T>` Must precede the type to remain generic
impl<T> GenericVal<T> {}
```

# 14.3 Traits

```rust
struct Null;
struct Empty;

trait<U> double_drop {
    fn<U> double_drop(self, _: U);
}

impl<T, U> double_drop for T {
    fn<U> double_drop(self, _: U) {}
}

null = Null;
empty = Empty;

// this move null and empty out of scope
empty.double_drop(null);
```

# 14.4 Bounds

- generic type `T` must impl a certain trait (ex: `Debug`)

```rust
// Define a function `printer` that takes a generic type `T` which
// must implement trait `Display`.
fn printer<T: Display>(t: T) {
    println!("{}", t);
}
```

## 14.4.1 Testcase: empty bounds

- bounds works even if trait is empty

# 14.5 Multiple bounds

```rust
fn compare_prints<T: Debug + Display>(t: &T) {
    println!("Debug: `{:?}`", t);
    println!("Display: `{}`", t);
}
```

# 14.6 Where clauses

```rust
impl <A, D> MyTrait<A, D> for YourType where
    A: TraitB + TraitC,
    D: TraitE + TraitF {}
```

```rust
trait PrintInOption {
    fn print_in_option(self);
}

// we need wher clause here, because Option<T> is the type that's printed
impl<T> PrintInOption for T where
    Option<T>: Debug {

    fn print_in_option {
        println("{:?}", Some(Self));
    }
}
```

# 14.7 New Type Idiom

- Ca semble évident... je sais pas à quoi sert ce chapitre ni le rapport avec sa catégorie
- The newtype idiom gives compile time guarantees that the right type of value is supplied to a program.

# 14.8 Associated items

- easier than trait for inner types

## 14.8.1 The Problem

```rust
struct Container(i32, i32);

// A trait which checks if 2 items are stored inside of container.
// Also retrieves first or last value.
trait Contains<A, B> {
    fn contains(&self, _: &A, _: &B) -> bool; // Explicitly requires `A` and `B`.
    fn first(&self) -> i32; // Doesn't explicitly require `A` or `B`.
    fn last(&self) -> i32;  // Doesn't explicitly require `A` or `B`.
}

impl Contains<i32, i32> for Container {
    // True if the numbers stored are equal.
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }

    // Grab the first number.
    fn first(&self) -> i32 { self.0 }

    // Grab the last number.
    fn last(&self) -> i32 { self.1 }
}

// `C` contains `A` and `B`. In light of that, having to express `A` and
// `B` again is a nuisance.
fn difference<A, B, C>(container: &C) -> i32 where
    C: Contains<A, B> {
    container.last() - container.first()
}
```

## 14.8.2 Associated types

```rust
struct Container(i32, i32);

// A trait which checks if 2 items are stored inside of container.
// Also retrieves first or last value.
trait Contains {
    // Define generic types here which methods will be able to utilize.
    type A;
    type B;

    fn contains(&self, _: &Self::A, _: &Self::B) -> bool;
    fn first(&self) -> i32;
    fn last(&self) -> i32;
}

impl Contains for Container {
    // Specify what types `A` and `B` are. If the `input` type
    // is `Container(i32, i32)`, the `output` types are determined
    // as `i32` and `i32`.
    type A = i32;
    type B = i32;

    // `&Self::A` and `&Self::B` are also valid here.
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }
    // Grab the first number.
    fn first(&self) -> i32 { self.0 }

    // Grab the last number.
    fn last(&self) -> i32 { self.1 }
}

fn difference<C: Contains>(container: &C) -> i32 {
    container.last() - container.first()
}
```

# 14.9 Phantom type parameters

- type that is used at compile time ONLY, not at runtime

## 14.9.1 Testcase: unit clarification

- here we need PhantomData because length have differents Units. We symobolize it with `<Unit>(PhantomData<Unit>)`

```rust
use std::ops::Add;
use std::marker::PhantomData;

/// Create void enumerations to define unit types.
#[derive(Debug, Clone, Copy)]
enum Inch {}
#[derive(Debug, Clone, Copy)]
enum Mm {}

/// `Length` is a type with phantom type parameter `Unit`,
/// and is not generic over the length type (that is `f64`).
///
/// `f64` already implements the `Clone` and `Copy` traits.
#[derive(Debug, Clone, Copy)]
struct Length<Unit>(f64, PhantomData<Unit>);

/// The `Add` trait defines the behavior of the `+` operator.
impl<Unit> Add for Length<Unit> {
    type Output = Length<Unit>;

    // add() returns a new `Length` struct containing the sum.
    fn add(self, rhs: Length<Unit>) -> Length<Unit> {
        // `+` calls the `Add` implementation for `f64`.
        Length(self.0 + rhs.0, PhantomData)
    }
}

fn main() {
    // Specifies `one_foot` to have phantom type parameter `Inch`.
    let one_foot:  Length<Inch> = Length(12.0, PhantomData);
    // `one_meter` has phantom type parameter `Mm`.
    let one_meter: Length<Mm>   = Length(1000.0, PhantomData);

    // `+` calls the `add()` method we implemented for `Length<Unit>`.
    //
    // Since `Length` implements `Copy`, `add()` does not consume
    // `one_foot` and `one_meter` but copies them into `self` and `rhs`.
    let two_feet = one_foot + one_foot;
    let two_meters = one_meter + one_meter;

    // Addition works.
    println!("one foot + one_foot = {:?} in", two_feet.0);
    println!("one meter + one_meter = {:?} mm", two_meters.0);

    // Nonsensical operations fail as they should:
    // Compile-time Error: type mismatch.
    //let one_feter = one_foot + one_meter;
}
```

# 15. Scoping rules

# 15.1 RAII

- RAII : Resource Acquisition Is Initialization

```rust
struct ToDrop;

// called when RESSOURCE (and not obj, item, etc...) is dropped
impl Drop for ToDrop {
    fn drop(&mut self) {
        println!("ToDrop is being dropped");
    }
}
```

# 15.2 Ownership and moves

- ressource only have ONE owner to prevent double droping
- so ressource ownership can be moved

```rust
let x = 2;

fn lol (x: int) {}

// here `x` is dropped
lol(x);
```

## 15.2.1 Mutability

- mutability of data can changed when ownership is transfered

```rust
let mut uwu = 1;
let owo = uwu;
```

## 15.2.2 Partial moves

- some struct fields can be moved and lost while other ones still here

```rust
#[derive(Debug)]
struct Person {
    name: String,
    age: Box<u8>,
}

let person = Person {
    name: String::from("Alice"),
    age: Box::new(20),
};

// `name` is moved out of person, but `age` is referenced
let Person { name, ref age } = person;
```

# 15.3 Borrowing

CREATE VARIABLE IN STACK OR HEAP
```rust
let boxed_i32 = Box::new(5_i32);
let stacked_i32 = 6_i32;
```

- borrowoing doesnt take ownership
- must last inside variable lifetime and not out (impossible to free variable if it is borrowed)

```rust
// This function takes ownership of a box and destroys it
fn eat_box_i32(boxed_i32: Box<i32>) {
    println!("Destroying box that contains {}", boxed_i32);
}

// This function borrows an i32
fn borrow_i32(borrowed_i32: &i32) {
    println!("This int is: {}", borrowed_i32);
}
```

## 15.3.1 Mutability

- `&mut T`

## 15.3.2 Aliasing

- while mutably borrowed, variable can't be borrowed ! Cause : reference are only a view of data

```rust
let mut point = Point { x: 0, y: 0, z: 0 };

let borrowed_point = &point;

println!("Point has coordinates: ({})", borrowed_point.x);

// Error! Can't borrow `point` as mutable because it's currently borrowed as immutable.
// let mutable_borrow = &mut point;

// The borrowed values are used again here
println!("Point has coordinates: ({})", borrowed_point.xz);

// The immutable references are no longer used for the rest of the code so it is possible to reborrow with a mutable reference.
let mutable_borrow = &mut point;
```

## 15.3.3 The ref pattern

with let binding :
***pattern matching***
***destructuring***

```rust
// A `ref` borrow on the left side of an assignment is equivalent to an `&` borrow on the right side.
let ref ref_c1 = c;
let ref_c2 = &c;
```

```rust
let _copy_of_x = {
    // `ref_to_x` is a reference to the `x` field of `point`.
    let Point { x: ref ref_to_x, y: _ } = point;

    // Return a copy of the `x` field of `point`.
    *ref_to_x
};
```

```rust
// `ref` can be paired with `mut` to take mutable references.
let Point { x: _, y: ref mut mut_ref_to_y } = mutable_point;
```

# 15.4 Lifetimes 

- borrow checker, to ensure variable lifetime start when it's created and ends when it's dropped
- lifetimes != scope 

## 15.4.1 Explicit annotation

- lifetime reference &'a must live LONGER than the 'a
- difficult to understand but it's easy after

```rust
// `foo` has a lifetime parameter `'a`
foo<'a>
// `foo` can not exceed  `'a` *or* `'b`
foo<'a, 'b>
```

## 15.4.2 Functions

```rust
// invalid cause &String is dropped right after function (cause it's a ref)
// fn invalid_output<'a>() -> &'a String { &String::from("foo") }

fn pass_x<'a, 'b>(x: &'a i32, _: &'b i32) -> &'a i32 { x }
```

- lifetime in action

```rust
struct Droppable {
    name: &'static str,
}

// This trivial implementation of `drop` adds a print to console.
impl Drop for Droppable {
    fn drop(&mut self) {
        println!("> Dropping {}", self.name);
    }
}

fn main() {
    let _a = Droppable { name: "a" };

    // block A
    {
        let _b = Droppable { name: "b" };

        // block B
        {
            let _c = Droppable { name: "c" };
            let _d = Droppable { name: "d" };

            println!("Exiting block B");
        }
        println!("Just exited block B");

        println!("Exiting block A");
    }
    println!("Just exited block A");

    // Variable can be manually dropped using the `drop` function
    drop(_a);
    // TODO ^ Try commenting this line

    println!("end of the main function");

    // `_a` *won't* be `drop`ed again here, because it already has been
    // (manually) `drop`ed
}```

## 15.4.3 Methods

- nothing new, same as fn

## 15.4.4 Structs

- nothing new, same as fn

## 15.4.5 Traits

- nothing new, same as fn

```rust
// A struct with annotation of lifetimes.
#[derive(Debug)]
struct Borrowed<'a> {
    x: &'a i32,
}

// Annotate lifetimes to impl.
impl<'a> Default for Borrowed<'a> {
    fn default() -> Self {
        Self {
            x: &10,
        }
    }
}

fn main() {
    let b: Borrowed = Default::default();
    println!("b is {:?}", b);
}
```

## 15.4.6 Bounds

- T: 'a: All references in T must outlive lifetime 'a.
- T: Trait + 'a: Type T must implement trait Trait and all references in T must outlive 'a

```rust
#[derive(Debug)]
struct Ref<'a, T: 'a>(&'a T);
// `Ref` contains a reference to a generic type `T` that has
// an unknown lifetime `'a`. `T` is bounded such that any
// *references* in `T` must outlive `'a`. Additionally, the lifetime
// of `Ref` may not exceed `'a`.

// A generic function which prints using the `Debug` trait.
fn print<T>(t: T) where
    T: Debug {
    println!("`print`: t is {:?}", t);
}

// Here a reference to `T` is taken where `T` implements
// `Debug` and all *references* in `T` outlive `'a`. In
// addition, `'a` must outlive the function.
fn print_ref<'a, T>(t: &'a T) where
    T: Debug + 'a {
    println!("`print_ref`: t is {:?}", t);
}
```

## 15.4.7 Coercion

- it's linking lifetimes together
    - chosing the short one
    - telling `'a` is at least as long as `'b`

```rust
// Here, Rust infers a lifetime that is as short as possible.
// The two references are then coerced to that lifetime.
fn multiply<'a>(first: &'a i32, second: &'a i32) -> i32 {
    first * second
}

// `<'a: 'b, 'b>` reads as lifetime `'a` is at least as long as `'b`.
// Here, we take in an `&'a i32` and return a `&'b i32` as a result of coercion.
fn choose_first<'a: 'b, 'b>(first: &'a i32, _: &'b i32) -> &'b i32 {
    first
}
```

## 15.4.8 Static

- these two ones are related but DIFFERENT

- static lifetime is read-only and last for all the program life

```rust
// A reference with 'static lifetime:
let s: &'static str = "hello world";

// 'static as part of a trait bound:
fn generic<T>(x: T) where T: 'static {}
```

- static trait bound

```rust
fn print_it( input: impl Debug + 'static ) {
    println!( "'static value passed in is: {:?}", input );
}

fn main() {
    // i is owned and contains no references, thus it's 'static:
    let i = 5;
    print_it(i);

    // oops, &i only has the lifetime defined by the scope of
    // main(), so it's not 'static:
    print_it(&i);
}
```

## 15.4.9 Elision

- ***elision*** implicitly annotates variables with lifetimes

# 16. Traits

- ***trait*** collection of methods that can be implemented for any data type.

```rust
struct Sheep { naked: bool, name: &'static str }

trait Animal {
    // Associated function signature; `Self` refers to the implementor type.
    fn new(name: &'static str) -> Self;

    // Method signatures; these will return a string.
    fn name(&self) -> &'static str;
    fn noise(&self) -> &'static str;

    // Traits can provide default method definitions.
    fn talk(&self) {
        println!("{} says {}", self.name(), self.noise());
    }
}

impl Sheep {
    fn is_naked(&self) -> bool {
        self.naked
    }

    fn shear(&mut self) {
        if self.is_naked() {
            // Implementor methods can use the implementor's trait methods.
            println!("{} is already naked...", self.name());
        } else {
            println!("{} gets a haircut!", self.name);

            self.naked = true;
        }
    }
}

// Implement the `Animal` trait for `Sheep`.
impl Animal for Sheep {
    // `Self` is the implementor type: `Sheep`.
    fn new(name: &'static str) -> Sheep {
        Sheep { name: name, naked: false }
    }

    fn name(&self) -> &'static str {
        self.name
    }

    fn noise(&self) -> &'static str {
        if self.is_naked() {
            "baaaaah?"
        } else {
            "baaaaah!"
        }
    }
    
    // Default trait methods can be overridden.
    fn talk(&self) {
        // For example, we can add some quiet contemplation.
        println!("{} pauses briefly... {}", self.name, self.noise());
    }
}
```

# 16.1 Derive 

- ***#[derive] attribute*** is a basic implementation for some traits

- Comparison traits: `Eq`, `PartialEq`, `Ord`, `PartialOrd`.
- `Clone`, to create `T` from `&T` via a copy.
- `Copy`, to give a type 'copy semantics' instead of 'move semantics'.
- `Hash`, to compute a hash from `&T`.
- `Default`, to create an empty instance of a data type.
- `Debug`, to format a value using the `{:?}` formatter.

```rust
#[derive(Debug)]
struct Inches(i32);
```

# 16.2 Returning Traits with dyn

- unlike others langages, rust's functions need to know returning data space. So, if a `Animal` trait exists, you can't return `Animal` cause there are differetn implementation of it

- but you can return a `Box` which contains some `Animal` in the heap with `dyn` keyword

- `box` is just a reference to some memory in the heap and it's size is know at compile time

```rust
// Returns some struct that implements Animal, but we don't know which one at compile time.
fn random_animal(random_number: f64) -> Box<dyn Animal> {
    if random_number < 0.5 {
        Box::new(Sheep {})
    } else {
        Box::new(Cow {})
    }
}
```

# 16.3 Operator Overloading

- operators are synthax sugar for methods and can be implemented with traits, here a list : [core::ops](https://doc.rust-lang.org/core/ops/)

```rust
use std::ops;

struct Foo;
struct Bar;

#[derive(Debug)]
struct FooBar;

impl ops::Add<Bar> for Foo {
    type Output = FooBar;

    fn add(self, _rhs: Bar) -> FooBar {
        println!("> Foo.add(Bar) was called");

        FooBar
    }
}
```

# 16.4 Drop

- called to free ressource

- COOL EXEMPLE TO SEE LIFETIME IN ACTION

```rust
struct Droppable {
    name: &'static str,
}

// This trivial implementation of `drop` adds a print to console.
impl Drop for Droppable {
    fn drop(&mut self) {
        println!("> Dropping {}", self.name);
    }
}

fn main() {
    let _a = Droppable { name: "a" };

    // block A
    {
        let _b = Droppable { name: "b" };

        // block B
        {
            let _c = Droppable { name: "c" };
            let _d = Droppable { name: "d" };

            println!("Exiting block B");
        }
        println!("Just exited block B");

        println!("Exiting block A");
    }
    println!("Just exited block A");

    // Variable can be manually dropped using the `drop` function
    drop(_a);
    // TODO ^ Try commenting this line

    println!("end of the main function");

    // `_a` *won't* be `drop`ed again here, because it already has been
    // (manually) `drop`ed
}
```

# 16.5 Iterators

- for conveniance, `for` turns some collections into iterators using `.into_iter()`

- require only one method to define `next` element

```rust
struct Fibonacci {
    curr: u32,
    next: u32,
}

// Implement `Iterator` for `Fibonacci`.
// The `Iterator` trait only requires a method to be defined for the `next` element.
impl Iterator for Fibonacci {
    // We can refer to this type using Self::Item
    type Item = u32;

    // Here, we define the sequence using `.curr` and `.next`.
    // The return type is `Option<T>`:
    //     * When the `Iterator` is finished, `None` is returned.
    //     * Otherwise, the next value is wrapped in `Some` and returned.
    // We use Self::Item in the return type, so we can change
    // the type without having to update the function signatures.
    fn next(&mut self) -> Option<Self::Item> {
        let current = self.curr;

        self.curr = self.next;
        self.next = current + self.next;

        // Since there's no endpoint to a Fibonacci sequence, the `Iterator` 
        // will never return `None`, and `Some` is always returned.
        Some(current)
    }
}

// Returns a Fibonacci sequence generator
fn fibonacci() -> Fibonacci {
    Fibonacci { curr: 0, next: 1 }
}

fn main() {
    // `0..3` is an `Iterator` that generates: 0, 1, and 2.
    let mut sequence = 0..3;

    println!("Four consecutive `next` calls on 0..3");
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());

    // `for` works through an `Iterator` until it returns `None`.
    // Each `Some` value is unwrapped and bound to a variable (here, `i`).
    println!("Iterate through 0..3 using `for`");
    for i in 0..3 {
        println!("> {}", i);
    }

    // The `take(n)` method reduces an `Iterator` to its first `n` terms.
    println!("The first four terms of the Fibonacci sequence are: ");
    for i in fibonacci().take(4) {
        println!("> {}", i);
    }

    // The `skip(n)` method shortens an `Iterator` by dropping its first `n` terms.
    println!("The next four terms of the Fibonacci sequence are: ");
    for i in fibonacci().skip(4).take(4) {
        println!("> {}", i);
    }

    let array = [1u32, 3, 3, 7];

    // The `iter` method produces an `Iterator` over an array/slice.
    println!("Iterate the following array {:?}", &array);
    for i in array.iter() {
        println!("> {}", i);
    }
}
```

# 16.6 `impl` Trait

- means you can not explicitly set the type you use
- arg type

```rust
fn parse_csv_document(src: impl std::io::BufRead) -> std::io::Result<Vec<Vec<String>>> {
    src.lines()
        .map(|line| {
            // For each line in the source
            line.map(|line| {
                // If the line was read successfully, process it, if not, return the error
                line.split(',') // Split the line separated by commas
                    .map(|entry| String::from(entry.trim())) // Remove leading and trailing whitespace
                    .collect() // Collect all strings in a row into a Vec<String>
            })
        })
        .collect() // Collect all lines into a Vec<Vec<String>>
}
```

- return type

```rust
fn combine_vecs_explicit_return_type(
    v: Vec<i32>,
    u: Vec<i32>,
) -> iter::Cycle<iter::Chain<IntoIter<i32>, IntoIter<i32>>> {
    v.into_iter().chain(u.into_iter()).cycle()
}

// OR !
fn combine_vecs(
    v: Vec<i32>,
    u: Vec<i32>,
) -> impl Iterator<Item=i32> {
    v.into_iter().chain(u.into_iter()).cycle()
}
```

- needed for closures and map/filter

```rust
// Returns a function that adds `y` to its input
fn make_adder_function(y: i32) -> impl Fn(i32) -> i32 {
    let closure = move |x: i32| { x + y };
    closure
}

fn main() {
    let plus_one = make_adder_function(1);
    assert_eq!(plus_one(2), 3);
}
```

```rust
fn double_positives<'a>(numbers: &'a Vec<i32>) -> impl Iterator<Item = i32> + 'a {
    numbers
        .iter()
        .filter(|x| x > &&0)
        .map(|x| x * 2)
}
```

# 16.7 Clone

- when you need to make a copy of a vraible and not only transfer it with `Clone` trait that define `.clone() method`

# 16.8 Supertraits

- no "inheriteance" but a trait can be a superset of another trait

```rust
trait Person {
    fn name(&self) -> String;
}


trait Programmer {
    fn fav_language(&self) -> String;
}

trait CompSciStudent: Programmer + Person {
    fn git_username(&self) -> String;
}
```

# 16.9 Disambiguating overlapping traits

- what if two traits have same method named `get()`

```rust
let username = <Form as UsernameWidget>::get(&form);
```

***rhs*** : right-hand-side

# 17. macro_rules!

- metaprogramming inside rust. it produces code inside source code
    - don't repeat code
    - special synthax
    - variadic interface (unset number of args, like println!())

# 17.1 Syntax

## 17.1.1 Designators

- a macro arg is preceded by a `$` and annotated with a *designator*

- Some designators :
    - `block`
    - `expr` is used for expressions
    - `ident` is used for variable/function names
    - `item`
    - `literal` is used for literal constants
    - `pat` (pattern)
    - `path`
    - `stmt` (statement)
    - `tt` (token tree)
    - `ty` (type)
    - `vis` (visibility qualifier)


```rust
macrorules! create_function {

    ($func_name:ident) => {
        fn $func_name() {
            println!("You called {}", 
                    stringify!($func_name));

        }
    };

}

macrorules! print_result {
    ($expression:expr) => {
        println!("{:?} = {:?}", 
                stringify!($expression),
                $expression);
    };
}
```

## 17.1.2 Overload

- different args combination !

```rust
// `test!` will compare `$left` and `$right`
// in different ways depending on how you invoke it:
macro_rules! test {
    // Arguments don't need to be separated by a comma.
    // Any template can be used!
    ($left:expr; and $right:expr) => {
        println!("{:?} and {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left && $right)
    };
    // ^ each arm must end with a semicolon.
    ($left:expr; or $right:expr) => {
        println!("{:?} or {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left || $right)
    };
}

fn main() {
    test!(1i32 + 1 == 2i32; and 2i32 * 2 == 4i32);
    test!(true; or false);
}
```

## 17.1.3 Repeat

- `$(...),+` one or more time
- `$(...),*` zero or more time

```rust
// `find_min!` will calculate the minimum of any number of arguments.
macro_rules! find_min {
    // Base case:
    ($x:expr) => ($x);
    // `$x` followed by at least one `$y,`
    ($x:expr, $($y:expr),+) => (
        // Call `find_min!` on the tail `$y`
        std::cmp::min($x, find_min!($($y),+))
    )
}

println!("{}", find_min!(5, 2 * 3, 4));
```

# 17.2 DRY (Don't Repeat Yourself)

- WOW I DON4T UNDERSTAND XD
- Macros allow writing DRY code by factoring out the common parts of functions and/or test suites. Here is an example that implements and tests the `+=`, `*=` and `-=` operators on `Vec<T>`:

```rust
use std::ops::{Add, Mul, Sub};

macro_rules! assert_equal_len {
    // The `tt` (token tree) designator is used for
    // operators and tokens.
    ($a:expr, $b:expr, $func:ident, $op:tt) => {
        assert!($a.len() == $b.len(),
                "{:?}: dimension mismatch: {:?} {:?} {:?}",
                stringify!($func),
                ($a.len(),),
                stringify!($op),
                ($b.len(),));
    };
}

macro_rules! op {
    ($func:ident, $bound:ident, $op:tt, $method:ident) => {
        fn $func<T: $bound<T, Output=T> + Copy>(xs: &mut Vec<T>, ys: &Vec<T>) {
            assert_equal_len!(xs, ys, $func, $op);

            for (x, y) in xs.iter_mut().zip(ys.iter()) {
                *x = $bound::$method(*x, *y);
                // *x = x.$method(*y);
            }
        }
    };
}

// Implement `add_assign`, `mul_assign`, and `sub_assign` functions.
op!(add_assign, Add, +=, add);
op!(mul_assign, Mul, *=, mul);
op!(sub_assign, Sub, -=, sub);

mod test {
    use std::iter;
    macro_rules! test {
        ($func:ident, $x:expr, $y:expr, $z:expr) => {
            #[test]
            fn $func() {
                for size in 0usize..10 {
                    let mut x: Vec<_> = iter::repeat($x).take(size).collect();
                    let y: Vec<_> = iter::repeat($y).take(size).collect();
                    let z: Vec<_> = iter::repeat($z).take(size).collect();

                    super::$func(&mut x, &y);

                    assert_eq!(x, z);
                }
            }
        };
    }

    // Test `add_assign`, `mul_assign`, and `sub_assign`.
    test!(add_assign, 1u32, 2u32, 3u32);
    test!(mul_assign, 2u32, 3u32, 6u32);
    test!(sub_assign, 3u32, 2u32, 1u32);
}
```

# 17.3 Domain Specific Languages (DSLs)

- create your owns synthax things ! here we create `eval`

```rust
macro_rules! calculate {
    (eval $e:expr) => {
        {
            let val: usize = $e; // :usize Force types to be integers
            println!("{} = {}", stringify!{$e}, val);
        }
    };
}

fn main() {
    calculate! {
        eval 1 + 2 // hehehe `eval` is _not_ a Rust keyword!
    }

    calculate! {
        eval (1 + 2) * (3 / 4)
    }
}
```

# 17.4 Variadic Interfaces

- take multiple args (recursive)

```rust
macro_rules! calculate {
    // The pattern for a single `eval`
    (eval $e:expr) => {
        {
            let val: usize = $e; // Force types to be integers
            println!("{} = {}", stringify!{$e}, val);
        }
    };

    // Decompose multiple `eval`s recursively
    (eval $e:expr, $(eval $es:expr),+) => {{
        calculate! { eval $e }
        calculate! { $(eval $es),+ }
    }};
}

fn main() {
    calculate! { // Look ma! Variadic `calculate!`!
        eval 1 + 2,
        eval 3 + 4,
        eval (2 * 3) + 1
    }
}
```

