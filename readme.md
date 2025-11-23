# Fibonacci in RISC-V

Files included:
- `fibonacci_iter.s` — iterative implementation (RARS-style syscalls), with input validation and 32-bit overflow detection
- `rars.jar` — a copy of the RARS simulator is included in this folder (run with `java -jar rars.jar`)

How to run (RARS — recommended)

1. The project folder includes a copy of RARS as `rars.jar`.
2. Run RARS directly (PowerShell):

```powershell
java -jar rars.jar fibonacci_iter.s
```

3. When prompted `n =`, type an integer (digits only, optionally preceded by `+` or `-`; leading spaces are ignored) and press Enter. Note: the program presents only this prompt — there is no menu or option selection.

Input validation and behavior

- The program reads the input as a string and validates it character-by-character. Invalid characters (letters or other non-digit symbols besides an optional leading `+`/`-`) cause the program to print an error message and exit. The program currently prints the messages in Portuguese; exact outputs are shown below.

Example invalid input output (exact text produced by the program):

```
entrada invalida: somente inteiros
```

- If the input is valid, it is converted to a signed 32-bit integer and used by the algorithm.

- The program detects potential signed 32-bit overflow before performing additions that could exceed `2^31 - 1`. For example, Fibonacci(47) = 2971215073, which is greater than 2^31−1; in that case the program prints:

```
overflow: resultado excede 32-bit
```

Note: Without overflow checking, RV32 arithmetic would wrap and yield a negative number for such inputs. The code now detects and reports overflow instead.

Examples of inputs and behavior
- `10` → prints Fibonacci(10) (valid output)
- `47` → prints `overflow: resultado excede 32-bit`
- `12a` → prints `entrada invalida: somente inteiros`

Using GNU toolchain + simulators (`spike` / `qemu`)

- If you prefer to assemble and link with the RISC-V GNU toolchain, the examples above still apply for assembling and linking, but keep in mind that the program uses RARS-style syscall numbers. When running under `pk`/Linux the syscall numbers differ; you would need to adapt the I/O code or use C for I/O handling.

Useful commands (PowerShell)

```powershell
# Run with RARS (quick and educational):
java -jar rars.jar fibonacci_iter.s

# Assemble + link with GNU toolchain (bare-metal example):
riscv64-unknown-elf-as -march=rv32i -o fib.o fibonacci_iter.s
riscv64-unknown-elf-ld -o fib.elf fib.o

# Or using gcc with a linker script (if you have linker.ld):
riscv64-unknown-elf-gcc -march=rv32i -mabi=ilp32 -nostdlib -nostartfiles -T linker.ld -o fib.elf fibonacci_iter.s

# Run on Spike with proxy-kernel 'pk' (requires pk):
spike pk fib.elf

# Or run with QEMU (rv32 example):
qemu-system-riscv32 -nographic -machine sifive_u -kernel fib.elf
```

Final notes

- The implementations use RARS conventions to keep the examples simple and educational. If you want full support for running under `spike`/`qemu` with `pk`, or want the programs to use Linux syscalls directly, I can adapt the I/O routines.
- If you would like me to add a minimal `linker.ld` and update `run_fib.ps1` to automate assemble/link/run steps, reply "yes" and I will add them.

