## Resolving the "unused DT entry" and "cannot locate symbol" Error
---  
This error typically occurs in environments where shared libraries are not properly linked or when there are compatibility issues between the executable and the libraries it depends on. The specific error messages indicate that the dynamic linker is having trouble finding a symbol (`getgrgid_r`) that is expected to be present in the shared libraries.

### Understanding the Error Messages
1  
---  
**Symbol `bzstd.so.1: unused DT entry: type 0x1d arg 0x38ea`**  
- This message indicates that there is an unused dynamic table entry in the shared library `bzstd.so.1`. The `DT` entry type `0x1d` refers to a specific kind of dynamic entry that is not being utilized. This can happen if the library was compiled with certain flags or if there are mismatches in the expected and actual library versions.

2  
---  
**CANNOT LINK EXECUTABLE: cannot locate symbol "getgrgid_r" referenced by ...**  
- This error means that the dynamic linker (usually `ld.so` or `ld-linux.so`) cannot find the `getgrgid_r` function, which is typically part of the standard C library (`libc`). This can occur if the library is missing, if there is a version mismatch, or if the library path is not correctly set.

### Steps to Resolve the Issue
- **Check Library Installation**: Ensure that the library `bzstd` and its dependencies are correctly installed. You can reinstall it using your package manager. For example, on Ubuntu, you can run:
  ```bash
  sudo apt-get install --reinstall libbz2-1.0
  ```

- **Verify Library Paths**: Make sure that the library paths are correctly set. You can check the paths using:
  ```bash
  ldconfig -p | grep bzstd
  ```
  This command lists the shared libraries and their paths. If `bzstd.so.1` is not listed, you may need to add its path to the `LD_LIBRARY_PATH` environment variable.

- **Check for Compatibility**: Ensure that the version of the library you are using is compatible with your executable. If you have recently updated your system or libraries, there may be compatibility issues.

- **Recompile the Executable**: If you have access to the source code of the executable, recompiling it against the current libraries can resolve symbol resolution issues. Make sure to clean the build environment before recompiling.

### Example of Setting Library Path
If you need to set the `LD_LIBRARY_PATH`, you can do so in your terminal:
```bash
export LD_LIBRARY_PATH=/path/to/your/library:$LD_LIBRARY_PATH
```
Replace `/path/to/your/library` with the actual path where `bzstd.so.1` is located.

### Additional Troubleshooting
- **Use `ldd` Command**: You can use the `ldd` command to check which shared libraries your executable is linked against:
  ```bash
  ldd your_executable
  ```
  This will show you if any libraries are missing.

- **Check for Multiple Versions**: Sometimes, multiple versions of a library can cause conflicts. Ensure that there is only one version of `bzstd` installed.

## References
## https://man7.org/linux/man-pages/man3/getgrgid_r.3.html  
## https://linux.die.net/man/1/ldd  
## https://man7.org/linux/man-pages/man1/ldconfig.8.html  