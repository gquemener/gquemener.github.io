---
layout: post
title:  "Rust Enum vs Trait"
date:   2024-02-03 10:54:09 +0100
categories: rust
---

_Reading time: 5 minutes_

A few years ago, I've started getting interested in Rust.

> Rust is blazingly fast and memory-efficient: with no runtime or garbage collector, it can power performance-critical services, run on embedded devices, and easily integrate with other languages. [Source](https://www.rust-lang.org/)

I've been practicing on the [Parrot Kata](https://github.com/emilybache/Parrot-Refactoring-Kata) that basically presents some code containing what Corey Haines calls "Procedural Polymorphism" in his book [Understanding the 4 rules of simple design](https://leanpub.com/4rulesofsimpledesign).

Until now, in Rust, I've found two ways to solve this anti-pattern using [Trait](https://doc.rust-lang.org/book/ch10-02-traits.html) and [Enum](https://doc.rust-lang.org/book/ch06-00-enums.html).

Today, I'd like to propose a review of such solutions, with a few pros and cons (the analysis is a work in progress, feel free to reach me on [Twitter](https://twitter.com/GildasQ) to participate).

## The Parrot Kata

You're given a piece of software computing different speed and cry computation for a given parrot type.

The code has a good test coverage, but mostly suffer from breaking the Open/Close principle.

Here's the original code:

```rust
struct Parrot<'a> {
    parrot_type: &'a str,
    number_of_coconuts: usize,
    voltage: f32,
    nailed: bool,
}

impl<'a> Parrot<'a> {
    pub fn speed(&self) -> Result<f32, &'static str> {
        match self.parrot_type {
            "european_parrot" => Ok(base_speed()),
            "african_parrot" => {
                let african_speed = base_speed() - load_factor() * self.number_of_coconuts as f32;
                if african_speed > 0.0 { Ok(african_speed) } else { Ok(0.0)}
            }
            "norwegian_blue_parrot" => {
                if self.nailed == true {
                    Ok(0.0)
                }
                else {
                    Ok(compute_base_speed_for_voltage(self.voltage))
                }
            }
            _ => Err("Should be unreachable!")
        }
    }

    // (...)
}

// (...)
```

In short, the main issue with this design is that it does not manage complexity (and specifically cyclomatic complexity) in an efficient way. Add that to the fact that the function will need constant modification for any new kind of parrot being implemented, and you end with a lot of messy, hard to reason about, and to maintain, code.

Furthermore, event though Rust support the concept of [exhaustive pattern matching](https://doc.rust-lang.org/book/ch06-02-match.html#matches-are-exhaustive), we cannot use it against a literal string type, because there are an infinite possible values for a string literal (here are a few: "a", "b", ..., "z", "aa", "ab", ..., "az", ...).

This increases the cognitive load of the developer by making her aware that any new type of parrot needs to be added to the match block (something that she would hopefully remember thanks to a well written test).

Finally, having to write (and maintain) a line that states that it should never be reached (`_ => Err("Should be unreachable!")`) just feels plain wrong to me.

## Polymorphism with Trait

```rust
const BASE_SPEED: f32 = 12.0;

trait Parrot {
    fn speed(&self) -> f32;
}

struct EuropeanParrot;
impl Parrot for EuropeanParrot {
    fn speed(&self) -> f32 {
        BASE_SPEED
    }
}

struct AfricanParrot {
    number_of_coconuts: usize,
}
impl Parrot for AfricanParrot {
    fn speed(&self) -> f32 {
        const LOAD_FACTOR: f32 = 9.0;
        let african_speed = BASE_SPEED - LOAD_FACTOR * self.number_of_coconuts as f32;
        match african_speed > 0.0 {
            true => african_speed,
            false => 0.0,
        }
    }
}

struct NorwegianParrot {
    voltage: f32,
    nailed: bool,
}
impl Parrot for NorwegianParrot {
    fn speed(&self) -> f32 {
        match self.nailed {
            true => 0.0,
            false => compute_base_speed_for_voltage(self.voltage),
        }
    }
}
```

### Pros
  - Each behaviour is well encapsulated in code blocks

### Cons
  - You have to think about implementing the Parrot trait for any new Parrot type. However, this is not entirely true as any function requiring an argument typed with the Trait will complain when something else is passed.

## Polymorphism with Enum
```rust
enum Parrot {
    European,
    African(u32),
    Norwegian(bool, f32),
}

const BASE_SPEED: f32 = 12.0;
impl Parrot {
    fn speed(&self) -> f32 {
        match self {
            Parrot::European => BASE_SPEED,

            Parrot::African(number_of_coconuts) => {
                const LOAD_FACTOR: f32 = 9.0;
                let speed = BASE_SPEED - LOAD_FACTOR * *number_of_coconuts as f32;
                if speed > 0.0 {
                    speed
                } else {
                    0.0
                }
            }

            Parrot::Norwegian(nailed, voltage) => match *nailed {
                false => {
                    let fixed_base_speed = 24.0;
                    let base_speed_for_voltage = voltage * BASE_SPEED;
                    if base_speed_for_voltage < fixed_base_speed {
                        base_speed_for_voltage
                    } else {
                        fixed_base_speed
                    }
                }
                _ => 0.0,
            },
        }
    }
}
```
### Pros
  - exhaustive pattern matching will enforce providing an implementation for speed and cry of any new Parrot type!

### Cons
  - Use of tuple, we lose semantic of the properties => fixable by using a struct instead of a tuple
  - Lot of indentations level

## Conclusion

It's unclear to me for now, I love the fact that exhaustive pattern matching will detect before runtime any missing implementation. However, it does not really solve the cyclomatic complexity and the open/close principle issue (whereas the Trait solution does).

Maybe there's a way you know to mix the best of both worlds?
