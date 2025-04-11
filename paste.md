## Summary Statically Linking C Library and Clang Runtime Library <br>
---
Static linking is the process of including all external libraries into the final executable at compile time, rather than relying on dynamic libraries at runtime. This makes the executable larger, but ensures that all necessary code is included for the application to run independently of the external libraries.

### Explanation
1  
---
To statically link a C library using the Clang compiler, you usually need to use specific flags in your `LDFLAGS`. The flags you mentioned, `-static-libgcc` and `-static-libstdc++`, are used to statically link the GCC runtime library and the C++ standard library, respectively.

- **-static-libgcc**: This option tells the linker to link the GCC runtime library (libgcc.a) statically. It ensures that the necessary low-level runtime support used by the GCC compiler is included in the output binary. This library contains functions that handle exception handling and passing of complex data types.

- **-static-libstdc++**: This option tells the linker to link the C++ Standard Library (libstdc++.a) statically. It includes the code from the C++ Standard Library that your application uses, making the application independent of the shared version of the library.

2  
---
To apply these flags, you can modify your makefile or compilation command as follows:

```bash
clang my_program.c -o my_program $LDFLAGS
```

In this command, `my_program.c` is your source file, and `my_program` is the resulting static executable. The `$LDFLAGS` variable holds your linker flags.

Additional options might be necessary depending on the specific libraries your code requires. You might also want to include `-static` to enforce static linking of all libraries:

```bash
LDFLAGS="-static -static-libgcc -static-libstdc++"
```

3  
---
It's important to note that not all libraries support static linking. Ensure that you have the static versions of the libraries available on your system. They usually have a `.a` extension (e.g., `libexample.a`).

Sometimes, you might encounter compatibility issues since certain libraries are designed to be used with shared linking. You can check if the static library exists in your system with:

```bash
ls /usr/lib/x86_64-linux-gnu/lib*.a
```

This command lists all static libraries available on a typical Linux system. Make sure the libraries you plan to link statically are present.

### Example
Here's a simple command that compiles a C program named `example.c` while linking with specified flags:

```bash
clang example.c -o example -static -static-libgcc -static-libstdc++
```

In this command:
- `example.c` is your source file.
- `-o example` specifies the output binary name.
- The flags ensure that both the GCC and the C++ standard libraries are linked statically.

---
#### References:
## https://clang.llvm.org/docs/CommandGuide/clang.html
## https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html#Link-Options