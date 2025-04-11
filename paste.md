## Statically Linking C Library and Clang Runtime Library  
---  
To statically link C libraries and the Clang runtime library, you typically use linker flags during the compilation process. The command you provided (`LDFLAGS+="-static-libgcc -static-libstdc++"`) is a method to achieve that with GCC/Clang.  

### Understanding LDFLAGS
- **LDFLAGS**: This is an environment variable used to specify options that the linker should use when linking object files.
- In the command, you append the flags `-static-libgcc` and `-static-libstdc++` to the `LDFLAGS`.

### Breakdown of Flags  
1. **-static-libgcc**  
   - **Purpose**: This flag instructs the compiler to statically link the GCC runtime library (libgcc).
   - **Usage**: It is particularly important when you want to ensure that your application does not rely on shared versions of the library, which could lead to versioning issues in different environments.

2. **-static-libstdc++**  
   - **Purpose**: Similar to `-static-libgcc`, this flag tells the compiler to statically link the GNU Standard C++ Library (libstdc++).
   - **Usage**: It ensures that all the standard C++ libraries used in the application are included in the executable, making it self-contained.

### Required Modules
- To use the flags properly, make sure you have the required static library files on your system. Static libraries typically have the `.a` extension.

### Example Scenario
Suppose you are compiling a simple C++ program called `example.cpp` that uses standard library features. You would run the following command:

```sh
clang++ example.cpp -o example -static-libgcc -static-libstdc++
```

This command does the following:
- `clang++`: Calls the Clang C++ compiler.
- `example.cpp`: Input file containing your C++ code.
- `-o example`: Specifies the output executable name.
- `-static-libgcc`: Statically links the GNU C runtime library.
- `-static-libstdc++`: Statically links the GNU C++ standard library.

### Additional Notes
- Ensure your system has the static versions of the libraries installed (often these are separate packages in many Linux distributions).
- Statically linking libraries can increase the size of your executable but can improve portability since it will not be dependent on the availability of shared libraries on the target machine.

## Example: Minimal Code Demonstration  
```cpp
#include <iostream>
int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```
The command to compile the above code statically would look like this:

```sh
clang++ hello.cpp -o hello -static-libgcc -static-libstdc++
```

Running `hello` will produce the output without needing any shared C++ libraries installed on the target machine.

## References  
## https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html  
## https://clang.llvm.org/docs/CommandGuide/clang.html  
## https://www.gnu.org/software/libc/manual/html_node/Static-Libraries.html