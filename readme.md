# Fibonacci Sequence in RISC-V Assembly

This project demonstrates a simple iterative implementation of the Fibonacci sequence in RISC-V assembly, designed for educational purposes and easy testing with the RARS simulator.

## Files

- `fibonacci_iter.s`: Iterative Fibonacci implementation with input validation and 32-bit overflow detection.
- `rars.jar`: RARS simulator (included for convenience).

## How to Run

1. Make sure you have Java installed.
2. Open a terminal in this folder and run:

    ```powershell
    java -jar rars.jar fibonacci_iter.s
    ```

3. The program will display instructions and then:

    ```
    Int Number=
    ```

    Type a positive integer (digits only, you can use `+` for sign, spaces before the number are ignored) and press Enter. Negative numbers or non-integers will be rejected.

## What to Expect

- The program reads your input as a string and checks if it is a valid integer. If you enter anything other than a valid integer (like letters or symbols), you'll see:

    ```
    invalid input: only integers allowed
    ```

- If the number is valid but too large for a 32-bit signed integer (for example, `47` for Fibonacci), you'll see:

    ```
    overflow: result exceeds 32-bit
    ```

- For valid inputs within range, the program prints the Fibonacci number for your input.

### Examples

- Input: `10` → Output: `55`
- Input: `47` → Output: `overflow: result exceeds 32-bit`
- Input: `12a` → Output: `invalid input: only integers allowed`

## Advanced Usage

If you want to assemble and run the program using the RISC-V GNU toolchain or other simulators (like Spike or QEMU), you can use these commands:

```powershell
# Assemble and link (bare-metal):
riscv64-unknown-elf-as -march=rv32i -o fib.o fibonacci_iter.s
riscv64-unknown-elf-ld -o fib.elf fib.o

# Or with GCC and a linker script:
riscv64-unknown-elf-gcc -march=rv32i -mabi=ilp32 -nostdlib -nostartfiles -T linker.ld -o fib.elf fibonacci_iter.s

# Run with Spike (requires pk):
spike pk fib.elf

# Run with QEMU:
qemu-system-riscv32 -nographic -machine sifive_u -kernel fib.elf
```

**Note:** The program uses RARS-style syscalls for input/output. If you run it outside RARS, you may need to adapt the code for Linux syscalls or use C for I/O.

## About

This project is intended for students and anyone interested in learning RISC-V assembly. The code is simple, well-commented, and ready to run in RARS. Feel free to experiment, modify, or use it as a starting point for your own RISC-V projects.
