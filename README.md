# CVM++

A custom compiler and stack-based virtual machine, built from scratch in C++17.

## Overview

CVM++ is a custom programming language with its entire toolchain implemented by hand — no parser generators, no interpreter libraries. Source code passes through a lexer, a recursive-descent parser, a bytecode compiler, and a stack-based virtual machine.

The language supports variables, arithmetic, comparisons, conditionals (`if`/`else`), and loops (`while`) — enough to implement general-purpose algorithms from Fibonacci to sorting.

## How It Works

**1. Lexer** — Scans raw text character by character and converts it into a stream of tokens (e.g., `NUMBER`, `PLUS`, `LET`).

**2. Parser** — A recursive-descent parser consumes tokens and builds an Abstract Syntax Tree. Operator precedence is enforced correctly (`1 + 2 * 3` parses as `1 + (2 * 3)`).

**3. Compiler** — Walks the AST and flattens it into a linear array of bytecode instructions. Control flow (`if`/`while`) uses jump instructions with backpatched offsets.

**4. VM** — A stack-based execution loop reads bytecode one opcode at a time. Values are pushed and popped from a value stack; variables are stored in a global map keyed by name.

Each stage has a single clear input and output, keeping the architecture modular and testable in isolation.

## Prerequisites

- C++17 compiler (g++ via MinGW on Windows)
- CMake 3.10 or newer

## Building (Windows / MinGW)

```bash
git clone https://github.com/PSAbhiram045/CVM.git
cd CVM
mkdir build && cd build
cmake -G "MinGW Makefiles" ..
cmake --build .
```

The compiled executable `cvm.exe` will appear inside the `build/` folder.

## Running

**Interactive REPL** — supports multi-line block detection for loops and conditionals:

```bash
.\cvm.exe
```

**File execution** — run a complete script:

```bash
.\cvm.exe ..\demo.cvm
```

A sample script `demo.cvm` is included in the project root.

## Project Layout

```
include/
  Token.h       Token definitions and types
  AST.h         AST node structures
  OpCode.h      Bytecode opcode definitions
  Lexer.h, Parser.h, Compiler.h, VM.h
src/
  Lexer.cpp     Source text -> Tokens
  Parser.cpp    Tokens -> AST
  Compiler.cpp  AST -> Bytecode
  VM.cpp        Executes bytecode on the stack
  main.cpp      CLI entry point (REPL and file runner)
```

## Design Notes

The goal was a complete, working compiler pipeline end-to-end rather than a large language feature set. Focusing on core data types (integers and booleans), foundational arithmetic, and basic control flow kept every stage testable in isolation. AST node ownership uses smart pointers rather than manual `new`/`delete`, keeping memory management predictable across the parser and compiler.
