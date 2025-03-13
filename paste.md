The symbol `__aeabi_memcpy` is typically associated with ARM architecture and is part of the ARM EABI (Embedded Application Binary Interface). This symbol is often found in libraries that provide low-level memory manipulation functions, such as the C standard library (libc) or specific runtime libraries for embedded systems.

### Finding the Library

1. **Check Existing Libraries**: You can use the `nm` command to search for the symbol in your existing libraries. For example, you can check the standard C library (glibc) or any other libraries you suspect might contain it:

   ```bash
   nm -D /path/to/library.so | grep __aeabi_memcpy
   ```

   Replace `/path/to/library.so` with the actual path to the libraries you want to check, such as `/lib/arm-linux-gnueabi/libc.so.6` or similar.

2. **Using `find` and `grep`**: If you want to search through all libraries in a directory, you can use:

   ```bash
   find /lib /usr/lib -name "*.so*" -exec nm -D {} \; | grep __aeabi_memcpy
   ```

### Adding the Library

If you find that the library containing `__aeabi_memcpy` is not present on your system, you may need to install the appropriate library. Here are some steps you can take:

1. **Install the Required Packages**: If you are working on an ARM-based system, you may need to install the ARM version of glibc or other relevant libraries. You can use your package manager (like `apt`, `yum`, or `pacman`) to install the necessary packages.

   For example, on a Debian-based system, you might run:

   ```bash
   sudo apt-get install libc6-armel
   ```

2. **Cross-Compiling**: If you are developing for an ARM target on a different architecture (like x86), you may need to set up a cross-compilation environment. This often involves installing a cross-compiler and the corresponding libraries.

3. **Using `patchelf`**: If you have multiple versions of glibc or other libraries, you can use `patchelf` to change the dynamic linker or library paths for your executable. This allows you to specify which version of the library to use.

   Example usage of `patchelf`:

   ```bash
   patchelf --set-interpreter /path/to/your/ld-linux-armhf.so.3 your_executable
   patchelf --set-rpath /path/to/your/libs your_executable
   ```

### Conclusion

To resolve the `CANNOT LINK EXECUTABLE: cannot locate symbol "__aeabi_memcpy"` error, you need to ensure that the library containing this symbol is available on your system. You can search for it using `nm`, install the necessary packages, or use `patchelf` to adjust the library paths if you have multiple versions. If you are cross-compiling, ensure that you have the correct libraries for your target architecture.