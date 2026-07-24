# Simplearn Compiler

Collaborators: 
Pranav Mantri
Avni Gupta

A compiler for **Simplearn** — a simple, Pascal-inspired programming language. Written using **Lex** (lexer) and **Yacc** (parser), it translates `.sl` / `.txt` source files into C code (`a.c`), which can then be compiled and run.

---

## Project Structure

```
├── lexer.l          # Lexer (tokenizer) — Lex source
├── parser.y         # Parser + code generator — Yacc source
├── Makefile         # Build automation
├── test.txt         # Comprehensive test program
├── test.sl          # Sample source file
├── utx.txt          # Simple string test
└── a.c              # Generated C output (created at runtime)
```

---

## Prerequisites

Make sure you have the following installed:

```bash
# Ubuntu / Debian / WSL
sudo apt install gcc flex bison

# Fedora / RHEL
sudo yum install gcc flex bison
```

---

## Building the Compiler

### With Makefile

```bash
make
```

### Without Makefile (manual steps)

Run these **in order**:

```bash
# Step 1: Generate parser from Yacc
yacc -d parser.y

# Step 2: Generate lexer from Lex
lex lexer.l

# Step 3: Compile everything into the compiler executable
gcc -Wall -g -o compiler y.tab.c lex.yy.c
```

> If Step 3 fails, try adding `-lm` at the end:
> ```bash
> gcc -Wall -g -o compiler y.tab.c lex.yy.c -lm
> ```

---

## Running the Compiler

```bash
./compiler yourfile.txt
```

This generates `a.c` — the compiled C output.

To then run your program:

```bash
gcc -o program a.c
./program
```


---

## Other Make Commands

| Command        | Description                        |
|----------------|------------------------------------|
| `make`         | Build the compiler                 |
| `make test`    | Build and run on a built-in sample |
| `make clean`   | Delete all generated files         |
| `make rebuild` | Clean then rebuild from scratch    |

---

## Language Syntax Guide

### Program Structure

Every Simplearn program follows this structure:

```
program <name>;
var
    <variable declarations>
begin
    <statements>
end
```

**Example:**
```
program Hello;
var
    x: int;
begin
    x := 42;
    print(x);
end
```

---

### Data Types

| Type     | Description         | Example value     |
|----------|---------------------|-------------------|
| `int`    | Integer number      | `10`, `-5`, `0`   |
| `float`  | Decimal number      | `3.14`, `25.5`    |
| `string` | Text / string       | `"Hello, World!"` |

---

### Variable Declaration

Declare variables in the `var` block before `begin`. Multiple variables of the same type can be declared with commas.

```
var
    x, y, z: int;
    temperature, avg: float;
    name, greeting: string;
```

---

### Assignment

Use `:=` to assign a value to a variable.

```
x := 10;
name := "Alice";
pi := 3.14159;
```

---

### Arithmetic Operators

| Operator | Description    | Example         |
|----------|----------------|-----------------|
| `+`      | Addition       | `x + y`         |
| `-`      | Subtraction    | `x - y`         |
| `*`      | Multiplication | `x * y`         |
| `/`      | Division       | `x / y`         |
| `%`      | Modulo         | `x % y`         |
| `-`      | Unary minus    | `-x`            |

Expressions support parentheses and follow standard operator precedence:

```
result := (x + y) * z - 10;
result := x + y * z / 2;
result := -x;
```

---

### Print

Print a value, variable, or string literal to the console.

```
print("Hello, World!");
print(x);
print(temperature);
print(name);
```

---

### Read (Input)

Read user input into a variable.

```
read(x);
read(name);
read(temperature);
```

---

### Comments

Both single-line and multi-line comments are supported.

```
// This is a single-line comment

/* This is a
   multi-line comment */
```

---

### Comparison Operators

Used inside conditions (`if`, `while`, `for`):

| Operator | Description           |
|----------|-----------------------|
| `=`      | Equal to              |
| `!=`     | Not equal to          |
| `<`      | Less than             |
| `<=`     | Less than or equal    |
| `>`      | Greater than          |
| `>=`     | Greater than or equal |

---

### Logical Operators

| Operator | Description | Example                      |
|----------|-------------|------------------------------|
| `and`    | Logical AND | `x > 0 and x < 10`          |
| `or`     | Logical OR  | `x < 0 or x > 100`          |
| `not`    | Logical NOT | `not x = 0`                  |

---

### If Statement

```
if <condition> then
    <statements>
endif
```

With else:

```
if <condition> then
    <statements>
else
    <statements>
endif
```

**Example:**
```
if x > 10 then
    print("Big");
else
    print("Small");
endif
```

Nested if:

```
if x > 0 then
    if x > 100 then
        print("Very large");
    else
        print("Moderate");
    endif
endif
```

---

### While Loop

```
while <condition> do
    <statements>
endwhile
```

**Example:**
```
counter := 1;
while counter <= 5 do
    print(counter);
    counter := counter + 1;
endwhile
```

---

### For Loop

```
for <var> := <start> to <end> step <step> do
    <statements>
endfor
```

**Example:**
```
// Count 1 to 5
for i := 1 to 5 step 1 do
    print(i);
endfor

// Count by 2s: 0, 2, 4, 6, 8, 10
for i := 0 to 10 step 2 do
    print(i);
endfor
```

---

### Full Example Program

```
program ComprehensiveTest;
var
    x, y, counter: int;
    temperature: float;
    name: string;
begin
    // Integer arithmetic
    x := 10;
    y := 20;
    print(x + y);       // 30

    // Float
    temperature := 36.6;
    print(temperature);

    // String input/output
    print("Enter your name:");
    read(name);
    print("Hello,");
    print(name);

    // If-else
    if x < y then
        print("x is smaller");
    else
        print("y is smaller");
    endif

    // While loop
    counter := 1;
    while counter <= 3 do
        print(counter);
        counter := counter + 1;
    endwhile

    // For loop
    for counter := 1 to 5 step 1 do
        print(counter);
    endfor
end
```

---

## How It Works

1. **Lexer (`lexer.l`)** — scans the source file and breaks it into tokens (keywords, identifiers, literals, operators).
2. **Parser (`parser.y`)** — validates grammar and generates equivalent C code, writing it to `a.c`.
3. **GCC** — compiles the generated `a.c` into a runnable program.

---

## Limitations

- No functions or procedures
- No arrays
- String concatenation is not supported
- Max 100 variables in the symbol table
- String variables are fixed at 256 characters
