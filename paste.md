## summary Statically Linking to libc in Clang and Resolving Symbol Errors <br>
---<br>
Explanation: This guide helps in understanding the problem of unresolved symbols such as `getgrgid_r` and how to statically link to the C library (`libc`) using Clang.

1  
The error **"CANNOT LINK EXECUTABLE: cannot locate symbol 'getgrgid_r' referenced by .."** suggests that the linker cannot find a specific function, `getgrgid_r`, which is likely due to the way libraries are linked or the environment configuration. This function is typically included in the standard C library. When you link dynamically, the linker tries to resolve symbols from the shared library at runtime. If it cannot find it, you get this error.

2  
To resolve this error, you need to ensure that the `libc` library is statically linked when compiling your program. Static linking incorporates all the library code into the final executable, removing the need for shared libraries during execution. 

   You can do this using the following command: 
   ```
   clang -static <source_files> -o <output_executable>
   ```
   In this command:
   - `-static`: Tells the compiler to link statically to libraries, including `libc`.
   - `<source_files>`: Replace this with your source code files (e.g., `main.c`).
   - `<output_executable>`: The desired name for the output file (e.g., `my_program`).

3  
It’s important to note that when statically linking, you may encounter other issues if your code or the libraries it depends on require shared dynamic libraries. You can verify what libraries your program is using by checking the linking with:
   ```
   clang -v <source_files> -o <output_executable>
   ```
   This command will provide verbose output of the compilation process, including the linked libraries.

   If you still encounter issues, consider checking if you have the development packages for the C library installed, as missing header files or library binaries can also trigger this type of errors. 

4  
Additionally, if you're developing on a system with different architectures or platform compatibility, ensure that the versions of libraries you're linking against support your target architecture. You can often specify paths to the libraries using the `-L` option and include specific header files using the `-I` option. 

Example: If you have a program `main.c` that uses standard functions and you want to compile it statically, the command would be:
```bash
clang -static main.c -o my_program
```
If you still face symbol errors, verify that the source code uses supported and correctly implemented functions.

---<br>
references: 
## https://clang.llvm.org/docs/CommandGuide/clang.html 
## https://man7.org/linux/man-pages/man3/getgrgid_r.3.html