## Static Linking Explained
---  
Static linking is a method of including libraries in a program at compile time, resulting in a single executable file that contains all the necessary code. This contrasts with dynamic linking, where libraries are linked at runtime.

### What is Static Linking?
Static linking involves copying all used library functions into the final executable. This means that the executable is self-contained and does not rely on external libraries at runtime. 

### Key Characteristics of Static Linking
- **Self-Contained Executable**: The final executable includes all the code it needs, which means it can run independently of the system libraries.
- **No Runtime Dependencies**: Since all necessary code is included, there are no dependencies on shared libraries like `libc.so` at runtime.
- **Larger Executable Size**: The resulting executable is typically larger than its dynamically linked counterpart because it contains all the library code.
- **Performance**: Static linking can lead to faster startup times since there is no need to load shared libraries.

### Limitations of Static Linking
- **Updates**: If a library is updated (for security or performance), the executable must be recompiled to include the new version.
- **Memory Usage**: Multiple programs using the same library will each have their own copy in memory, which can lead to increased memory usage.

### Example of Static Linking
To create a statically linked executable in C, you can use the `gcc` compiler with the `-static` flag. Here’s a simple example:

```bash
gcc -static -o my_program my_program.c -lm
```
- `gcc`: The GNU Compiler Collection, used for compiling C programs.
- `-static`: This flag tells the compiler to link statically.
- `-o my_program`: This specifies the output file name.
- `my_program.c`: The source code file to compile.
- `-lm`: This links the math library statically.

### Conclusion
Static linking means that the executable does not depend on shared libraries like `libc.so` at runtime. It is a method that provides independence from the system's library versions but comes with trade-offs in terms of size and update management.

## References
## https://en.wikipedia.org/wiki/Static_library  
## https://www.gnu.org/software/libc/manual/html_node/Static-Libraries.html  