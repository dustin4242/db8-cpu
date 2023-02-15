# DB-8 Cpu Emulator
This is a idea for a project that i've had for a very long time and I've finally made a emulator for it.
---
## How to initialize memory:
Every DB-8 cpu also has a memory sequence to go along with it and the way we initialize said memory is using the `memory_init![]` macro.
The macro takes in multiple `Vec<u8>` lists and concats each of them to make a final number sequence.

**Ex:**
```rust
let mut memory: Vec<u8> = memory_init![
	vec![11], // Halt Instruction
];
```
***Note:*** *make sure memory is mutable for `set_mem_to_a!()` instuctions*

## How to use instructions:
Now we can get into the fun part: **Programming**. Programming a DB-8 cpu's memory is relatively easy however it can get easy to get lost in the code since it is in a assembly-like syntax. To make programming the memory easier I have provided many macros for you guys to use:
```
set_a!(value)
set_b!(value)
set_a_to_mem!(mem_pos)
set_mem_to_a!(mem_pos)
add!()
sub!()
jump_to!(mem_pos)
jump_to_if_zero!(mem_pos)
print!()
print_char!()
halt!()
```
***Notes:***
- `value` & `mem_pos` is of type `u8` to respect the memory's layout
- `add` & `subtract` respectively use `a + b` & `a - b`
- `jump_to_if_zero!`, `print!`, & `print_char!` are all respective to the value in the a register

To use these macros to easily set the memory just follow the layout below:
```rust
let mut memory: Vec<u8> = memory_init![
	set_a!(119), // Ascii Char "w"
	set_b!(32), // Set B to 32
	sub!(), // Will set a to 87 a.k.a Ascii Char "W"
	print_char!(), // Will print "W" in A Register
];
```

## How to execute memory:
We now need to initialize a new cpu to execute this memory we set and we can easily do that by using the DB8 type:
```rust
let mut cpu = DB8::new();
```

And then pass in the memory to the `exec()` function on the cpu:
```rust
cpu.exec(memory);
```

## Final Notes:
- In my initial main.rs file I have already included the necessary import however if you intend to make a seperate file for the memory or anything else here are the specified imports:
```rust
mod macros; // Imports memory_init and instruction macros

mod cpu;
use cpu::DB8; // DB8 CPU Type
```