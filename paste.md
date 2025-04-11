## summary Compiling C programs with GCC and the significance of the `-no-pie` flag <br>
---<br>
**Explanation:** 

1. **Understanding GCC and the `-no-pie` flag:**  
   GCC (GNU Compiler Collection) is a powerful compiler system that supports various programming languages, including C. The `-no-pie` flag is used while compiling to indicate that the output should not be Position Independent Executable (PIE). When compiling with this option:
   - The linker creates a regular (non-PIE) executable.
   - Non-PIE executables have fixed addresses in memory, which can lead to a minor performance improvement but reduce security compared to PIE.

2. **Static Compilation with `-static`:**  
   The `-static` flag is used to create a statically linked executable. This means that all the library functions that the program uses are included directly in the executable, rather than linking them at runtime. The benefits of static linking include:
   - No dependency on shared libraries at runtime; the executable can run independently.
   - Potentially increased performance because it avoids the overhead of dynamic linking.

3. **Compatibility considerations with NDK and API levels:**  
   The Android NDK (Native Development Kit) allows you to develop applications using native languages like C and C++. 
   - If you compile a program targeting API level 24 (which may include additional system calls or features) and attempt to run it on a device with API level 23, the compatibility will depend on:
     - Whether the compiled code uses any features exclusive to API level 24.
     - If the program relies on behavior from the newer version, it may fail to run on the older one.
   - Generally, if your code does not utilize any API 24 specific functionality, it should work on an API 23 environment.

---
**Example:**  
To illustrate the use of GCC with the `-no-pie` and `-static` flags, consider the following simple C program in `test.c`:

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

When you run the command:

```bash
gcc -no-pie -static test.c -o test_program
```

- Here, `gcc` is invoked to compile `test.c`.
- `-no-pie` specifies that the output should be a non-PIE executable.
- `-static` enables static linking.
- `-o test_program` names the resulting executable as `test_program`.

This results in a standalone executable that does not require shared libraries for standard I/O functions used, and it does not utilize position-independent code.

---
## References: 
## https://gcc.gnu.org/onlinedocs/gcc/Option-Summary.html 
## https://android.com/ndk/