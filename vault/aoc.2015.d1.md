---
id: dcb2143d-1df6-4c9e-9289-cbc76082ace2
title: Day 1
desc: ''
updated: 1609049130527
created: 1609002966621
---

# Not Quite Lisp

Santa was hoping for a white Christmas, but his weather machine's "snow" function is powered by stars, and he's fresh out! To save Christmas, he needs you to collect fifty stars by December 25th.

## Part I
Santa is trying to deliver presents in a large apartment building, but he can't find the right floor - the directions he got are a little confusing.
 He starts on the ground floor (floor `0`) and then follows the instructions one character at a time.

An opening parenthesis, `(`, means he should go up one floor, and a closing parenthesis, ), means he should go down one floor.

The apartment building is very tall, and the basement is very deep; he will never find the top or bottom floors.

For example:
> `(())` and `()()` both result in floor `0`.
>
>`(((` and `(()(()(` both result in floor `3`.
>
>`))(((((` also results in floor `3`.
>
>`())` and `))(` both result in floor `-1` (the first basement
level).
>
>`)))` and `)())())` both result in floor `-3`.


To what floor do the instructions take Santa?

## Solution

The easiest way to solve this problem would be to split the input string into chars and iterate over each character.

After that we use the [iterator::fold](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fold) method of the iterator over the characters.

For each `(` we increment a sum value by 1 and for ')' we decrement a sum value by 1.
```rust
#[aoc(day1, part1)]
pub fn part1(input: &str) -> i32 {
    input.chars().fold(0, |sum, c| match c {
        '(' => sum + 1,
        ')' => sum - 1,
        _ => unreachable!(),
    })
}
```

## Part II

Now, given the same instructions, find the position of the first character that causes him to enter the basement (floor -1).

 The first character in the instructions has position 1, the second character has position 2, and so on.

For example:

> `)` causes him to enter the basement at character position `1`.
>
> `()())` causes him to enter the basement at character position `5`.

What is the position of the character that causes Santa to first enter the basement?

## Solution
The easiest way to solve this would be be to keep a check if the sum value every becomes less than 0.

This condition can be easily identified by using the [checked_sub](https://doc.rust-lang.org/std/primitive.isize.html#method.checked_sub)
```rust
#[aoc(day1, part2)]
pub fn part2(input: &str) -> usize {
    let mut sum: u32 = 0;
    for (i, c) in input.chars().enumerate() {
        match c {
            '(' => sum += 1,
            ')' => {
                if let Some(s) = sum.checked_sub(1) {
                    sum = s;
                } else {
                    return i + 1;
                }
            }
            _ => unreachable!(),
        }
    }
    unreachable!()
}
```