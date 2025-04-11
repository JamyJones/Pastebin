## 10 Most Useful Compiler Options for Static Linking
---  
Static linking is a process where all the necessary library code is included in the final executable at compile time, resulting in a standalone binary. Here are ten useful compiler options for static linking, primarily focusing on GCC (GNU Compiler Collection) and Clang, which are widely used compilers.

1  
---  
**`-static`**  
This option tells the compiler to perform static linking. It instructs the linker to use static libraries instead of shared libraries. This is essential when you want to ensure that your executable does not depend on shared libraries at runtime.

2  
---  
**`-L<directory>`**  
This option specifies a directory to search for libraries. When linking statically, you may need to point the compiler to the location of your static libraries. For example, `-L/usr/local/lib` tells the compiler to look in `/usr/local/lib` for libraries.

3  
---  
**`-l<library>`**  
This option links against a specific library. When used with static linking, it will link the static version of the library if available. For example, `-lm` links against the math library (`libm.a`).

4  
---  
**`-Wl,-Bstatic`**  
This option is passed to the linker to force static linking for the subsequent libraries. It can be used in conjunction with `-Wl,-Bdynamic` to switch back to dynamic linking for other libraries. For example:  
```bash
gcc main.o -Wl,-Bstatic -lmylib -Wl,-Bdynamic -lc
```

5  
---  
**`-Wl,-o <output>`**  
This option specifies the name of the output file. When performing static linking, you can define the name of the final executable. For example:  
```bash
gcc -static -o myprogram main.o
```

6  
---  
**`-static-libgcc`**  
This option links the GCC runtime library statically. It ensures that the C runtime library is included in the executable, which can be useful for portability.

7  
---  
**`-static-libstdc++`**  
Similar to `-static-libgcc`, this option links the C++ standard library statically. It is essential for C++ applications that need to be self-contained.

8  
---  
**`-Wl,--whole-archive`**  
This option tells the linker to include all object files from the specified static library, even if they are not referenced. This is useful when you want to ensure that all parts of a library are included in the final executable.

9  
---  
**`-Wl,--no-whole-archive`**  
This option is used to stop the effect of `--whole-archive`. It is useful when you want to include only specific parts of a library after using `--whole-archive`.

10  
---  
**`-fPIC`**  
While primarily used for creating position-independent code for shared libraries, it can also be useful in static linking when you want to ensure that the code can be used in both static and dynamic contexts.

---  
Example:  
Here’s a simple example of how to compile a static executable using GCC:  
```bash
gcc -static -o myprogram main.o -L/usr/local/lib -lm
```
In this command:  
- `-static` ensures static linking.
- `-o myprogram` specifies the output file name.
- `-L/usr/local/lib` adds a directory to the library search path.
- `-lm` links against the math library statically.

---  
References:  
## https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html  
## https://clang.llvm.org/docs/CommandGuide/clang.html  