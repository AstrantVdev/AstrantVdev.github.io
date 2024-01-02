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

***Inferrence***

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

---

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

---

***Slices***
- signature: `&[T]` and has a pointer and a length
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

## 3.3 Constants

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

---

***scope***
- block of code containing some variables and code
- inner variables of a scope have a lifetime equal to their scope ending by default

## 4.3 Declare first

***declaration***
- name a variable
- can be done without *initialisation*, but compil error if never initialized
- *declaration first* is seldom because risky

---

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

---

***casting***
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







