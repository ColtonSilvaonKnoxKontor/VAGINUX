# VaginaFuck Documentation

Here is the current list of syntaxes you can use if ever you want to add program scripts.

``` VaginaFuck (VF) Documentation & Syntax Specification
Version 2.4 Modular (Kernel-Init Edition)

VaginaFuck is a high-level, multi-paradigm evolution of Brainfuck. It uses a Sparse Memory Model with a Shadow Register, an internal Value Stack, and Dynamic Type Promotion.


1. Core Architecture
- Sparse Data Map: Memory is a std::map<long long, Value>. Address space is effectively infinite (64-bit indices).
- Dynamic Typing: Cells automatically switch types (INT, FLOAT, DOUBLE, BOOL, STRING).
- Shadow Register: A single "Value" storage used for binary arithmetic and variable transfers.
- Variable Map: A secondary key-value store isolated from the primary data cells.
- Value Stack: A Last-In-First-Out (LIFO) stack for temporary value storage.

---

2. Syntax List

Basic Navigation & Manipulation
┌─────┬─────────────────────────────────────────────────────────────┐
│ Cmd │ Description                                                 │
├─────┼─────────────────────────────────────────────────────────────┤
│ >   │ Increment Data Pointer (DP).                                │
│ <   │ Decrement Data Pointer (DP).                                │
│ +   │ Increment Cell (Normal mode: +1 for numbers).               │
│ -   │ Decrement Cell (Normal mode: -1 for numbers).               │
│ *   │ Quick Math: Multiply Cell by 4 (Legacy compat).             │
│ /   │ Quick Math: Divide Cell by 4 (Legacy compat).               │
│ z   │ Zero & Reset: Clears cell to 0 (INT) and disables mathMode. │
└─────┴─────────────────────────────────────────────────────────────┘

The Shadow Register, Variables & Stack
┌─────┬───────────────────────────────────────────────────────────┐
│ Cmd │ Description                                               │
├─────┼───────────────────────────────────────────────────────────┤
│ $   │ Store: Copy current Cell into Shadow Register.            │
│ !   │ Load: Copy Shadow Register into current Cell.             │
│ S   │ Swap: Exchange current Cell with Shadow Register.         │
│ {   │ Variable Save: Variables[Cell] = Shadow Register.         │
│ }   │ Variable Load: Shadow Register = Variables[Cell].         │
│ u   │ Push: Push current Cell onto the Value Stack.             │
│ p   │ Pop: Pop from Value Stack into current Cell.              │
│ :   │ Clone: Copy Cell[DP] to Cell[DP+1].                       │
└─────┴───────────────────────────────────────────────────────────┘

Advanced Arithmetic (Math Mode)
The _ prefix triggers Math Mode for the single next operation, using the Register as the right-hand operand.
┌─────┬─────────────────────────────────────────────────────────┐
│ Cmd │ Description                                             │
├─────┼─────────────────────────────────────────────────────────┤
│ _+  │ Cell = Cell + Register (Supports String concatenation). │
│ _-  │ Cell = Cell - Register.                                 │
│ _*  │ Cell = Cell * Register (Supports String repetition).    │
│ _/  │ Cell = Cell / Register.                                 │
│ _%  │ Cell = Cell % Register (Modulo).                        │
│ _=  │ Cell = (Cell == Register) (Boolean 1/0).                │
│ _!  │ Cell = (Cell != Register) (Boolean 1/0).                │
│ _<  │ Cell = (Cell < Register) (Boolean 1/0).                 │
│ _>  │ Cell = (Cell > Register) (Boolean 1/0).                 │
│ _[  │ Cell = (Cell <= Register) (Boolean 1/0).                │
│ _]  │ Cell = (Cell >= Register) (Boolean 1/0).                │
└─────┴─────────────────────────────────────────────────────────┘

Type Casting & Measurement
┌─────┬────────────────────────────────────────────────────────────────────────┐
│ Cmd │ Description                                                            │
├─────┼────────────────────────────────────────────────────────────────────────┤
│ i   │ Cast to Integer.                                                       │
│ f   │ Cast to Float.                                                         │
│ d   │ Cast to Double.                                                        │
│ b   │ Cast to Boolean (Non-zero = true).                                     │
│ s   │ Cast to String (Literal "123").                                        │
│ '   │ ASCII Cast: Converts numeric Cell value to a String of that character. │
│ m   │ Measure: Set Cell to the length of its string/number representation.   │
└─────┴────────────────────────────────────────────────────────────────────────┘

Kernel-Init & System
┌─────┬─────────────────────────────────────────────────────────────────┐
│ Cmd │ Description                                                     │
├─────┼─────────────────────────────────────────────────────────────────┤
│ W   │ Wait: Sleep for Cell milliseconds.                              │
│ R   │ Read: Read contents of file (path in Cell) into Cell.           │
│ V   │ Write: Write Register value into file (path in Cell).           │
│ E   │ Env: Get environment variable (Cell name) into Cell.            │
│ P   │ PID: Get current Process ID into Cell.                          │
│ C   │ Chdir: Change current working directory to Cell path.           │
│ A   │ Arg: Get CLI argument at index Register into Cell.              │
│ T   │ Time: Get current Unix Timestamp into Cell.                     │
│ X   │ Execute: Run the current Cell value as a shell command.         │
└─────┴─────────────────────────────────────────────────────────────────┘

Logic, Flow & Control
┌─────┬───────────────────────────────────────────────────────────────┐
│ Cmd │ Description                                                   │
├─────┼───────────────────────────────────────────────────────────────┤
│ [   │ Brainfuck Loop Start (Jump past ] if Cell == 0).              │
│ ]   │ Brainfuck Loop End (Jump to [ if Cell != 0).                  │
│ ?   │ If: Execute if Cell != 0. Supports \ (Else).                  │
│ \   │ Else: Part of the ? block.                                    │
│ ;   │ End If: Terminates a ? or ? \ block.                          │
│ G   │ Goto: Set Data Pointer to the value in the current Cell.      │
│ L   │ Location: Write the current Data Pointer index into the Cell. │
│ e   │ Exit: Terminate program immediately.                          │
│ #   │ Comment: Ignore everything until newline.                     │
└─────┴───────────────────────────────────────────────────────────────┘

Input / Output
┌───────┬──────────────────────────────────────────────────────────────────────┐
│ Cmd   │ Description                                                          │
├───────┼──────────────────────────────────────────────────────────────────────┤
│ . / o │ Output Cell as ASCII character.                                      │
│ O     │ Smart Output: Output Cell value as its current type (String/Number). │
│ x     │ Output Cell as Hexadecimal.                                          │
│ K     │ Input: Read full line from stdin into current Cell as String.        │
└───────┴──────────────────────────────────────────────────────────────────────┘

---

3. Implementation Notes
- String Conversion: toInt() and toDouble() prioritize numeric parsing. Falls back to ASCII.
- System Command (X): Extremely powerful for automation and audits.
- Init Mode: Use P, A, E to identify environment and V, R for configuration.
- Sparse Memory: Use L and G to manage dynamic pointers. Multi-array support.```
