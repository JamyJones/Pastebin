## 10 Most Useful Compiler Options for Static Linking
---  
Static linking is a process where all the necessary library code is included in the final executable at compile time, rather than at runtime. This can lead to faster execution and fewer dependencies. Below are some of the most useful compiler options for static linking.

1  
---  
**`-static`**  
This option tells the compiler to perform static linking. It instructs the linker to use static libraries instead of shared libraries. This is essential when you want to ensure that all dependencies are included in the final executable.

2  
---  
**`-L<directory>`**  
This option specifies a directory to search for libraries. When using static linking, you may need to point the compiler to the location of your static libraries. For example, `-L/usr/local/lib` would add `/usr/local/lib` to the library search path.

3  
---  
**`-l<library>`**  
This option links against a specific library. When using static linking, you can specify the library you want to link against. For example, `-lm` links against the math library. If you have a static version of the library (e.g., `libm.a`), it will be used.

4  
---  
**`-Wl,-Bstatic`**  
This option is used to instruct the linker to prefer static libraries over shared ones. It can be useful when you want to link against both types of libraries but prefer static ones. This option is passed to the linker via the `-Wl,` prefix.

5  
---  
**`-Wl,-Bdynamic`**  
This option tells the linker to prefer dynamic libraries. It can be used in conjunction with `-Wl,-Bstatic` to switch between static and dynamic linking within the same command.

6  
---  
**`-static-libgcc`**  
This option links the GCC runtime library statically. It ensures that the GCC runtime is included in the executable, which can be useful for portability and reducing dependencies.

7  
---  
**`-static-libstdc++`**  
Similar to `-static-libgcc`, this option links the C++ standard library statically. It is important for C++ applications that need to be self-contained.

8  
---  
**`-o <output_file>`**  
This option specifies the name of the output file. When performing static linking, you can define the name of the final executable. For example, `-o my_program` will create an executable named `my_program`.

9  
---  
**`-fPIC`**  
While primarily used for position-independent code, this option can be useful when creating static libraries that may later be linked into shared libraries. It ensures that the code can be loaded at any memory address.

10  
---  
**`-Wl,--whole-archive`**  
This option tells the linker to include all object files from the specified static library, even if they are not referenced. This can be useful when you want to ensure that all parts of a library are included in the final executable.

---  
Example:  
To compile a C program with static linking, you might use a command like this:  
```bash
gcc -o my_program my_program.c -static -L/usr/local/lib -lm -Wl,-Bstatic -lmy_static_lib -Wl,-Bdynamic -lmy_dynamic_lib
```
In this command:  
- `gcc` is the GNU Compiler Collection.
- `-o my_program` specifies the output file name.
- `-static` enables static linking.
- `-L/usr/local/lib` adds a directory to the library search path.
- `-lm` links against the math library.
- `-Wl,-Bstatic -lmy_static_lib` links against a static library.
- `-Wl,-Bdynamic -lmy_dynamic_lib` links against a dynamic library.

---  
References:  
## https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html  
## https://www.gnu.org/software/libc/manual/html_node/Static-Libraries.html  