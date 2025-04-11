## Configuring Compiler for Static Linking with glibc <br>
---<br>
To statically include all symbols from glibc and ensure your application runs on systems with older versions of glibc, you need to take a few specific steps. Static linking with glibc can be tricky due to its design, but here’s how you can approach it.

1<br>
---<br>
**Understanding Static vs Dynamic Linking**  
Static linking includes all the necessary libraries and symbols directly into the executable at compile time, while dynamic linking relies on shared libraries at runtime. When you use `-static`, it tells the linker to include static versions of libraries. However, glibc is primarily designed to be used dynamically, and static linking can lead to missing symbols or compatibility issues.

2<br>
---<br>
**Using the Correct Flags**  
To statically link your application with glibc, you can use the following flags:

```bash
gcc -o my_program my_program.c -static -static-libgcc -static-libstdc++
```

- `-static`: This flag tells the compiler to link against the static versions of libraries.
- `-static-libgcc`: This flag links the GCC runtime library statically.
- `-static-libstdc++`: This flag links the C++ standard library statically.

However, simply using these flags may not be sufficient for glibc, as it may still require dynamic linking for certain functionalities.

3<br>
---<br>
**Using musl libc as an Alternative**  
If you encounter issues with glibc, consider using musl libc, which is designed for static linking and is compatible with many applications. You can compile your application with musl by using the following command:

```bash
musl-gcc -o my_program my_program.c
```

This will create a statically linked executable that is more portable across different systems.

4<br>
---<br>
**Checking for Missing Symbols**  
If you still face issues with missing symbols, you can use the `ldd` command to check which dynamic libraries your executable is trying to link against:

```bash
ldd my_program
```

This will list all the shared libraries required by your executable. If you see any glibc libraries listed, it indicates that your application is not fully statically linked.

5<br>
---<br>
**Considerations**  
- Statically linking glibc can lead to larger binaries and potential compatibility issues. 
- Some features of glibc, such as threading and locale support, may not work correctly when statically linked.
- Always test your statically linked binaries on the target systems to ensure compatibility.

---<br>
**Example**  
Here’s a simple example of compiling a C program with static linking:

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

Compile it with:

```bash
gcc -o hello hello.c -static -static-libgcc -static-libstdc++
```

This will create a statically linked executable named `hello`.

---<br>
**References**  
## https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html  
## https://musl.libc.org/  
## https://www.gnu.org/software/libc/manual/html_node/Static-Linking.html  