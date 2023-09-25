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

# Custom Types

- struct
- enum
- const/static

## Structures

- Tuple structs, which are, basically, named tuples, for generics
- The classic C structs
- Unit structs, which are field-less, are useful for generics

```rust
let point: Point = Point { x: 10.3, y: 0.4 };

let bottom_right = Point { x: 5.2, ..point }; //auto-fill

// Destructure the point using a `let` binding
let Point { x: left_edge, y: top_edge } = point;
```

## enums

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

## Constants

- const: An unchangeable value (the common case).
- static: A possibly mutable variable with 'static lifetime. The static lifetime is inferred and does not have to be specified. Accessing or modifying a mutable static variable is unsafe

