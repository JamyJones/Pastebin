## Resolving the Error: "symbol bzstd.so.1: unused DT entry" and "CANNOT LINK EXECUTABLE: cannot locate symbol 'getgrgid_r'"

---

**Explanation:**

1. **Understanding the Error Messages:**
   - The first part of the error, **"symbol bzstd.so.1: unused DT entry: type 0x1d arg 0x38ea,"** indicates that there is an issue with the dynamic linking of the `bzstd.so.1` shared library. This typically means that the dynamic linker found an entry in the dynamic table that it did not use, which can happen if the library is not compatible with the current environment or if there are missing dependencies.
   - The second part, **"CANNOT LINK EXECUTABLE: cannot locate symbol 'getgrgid_r' referenced by ...,"** suggests that the executable is trying to reference the `getgrgid_r` function, which is not found in the linked libraries. This function is typically used to retrieve group information in a thread-safe manner.

---

2. **Possible Causes:**
   - **Library Compatibility:** The version of `bzstd.so.1` may not be compatible with your system or the version of the libraries it depends on.
   - **Missing Dependencies:** The required libraries that contain the `getgrgid_r` function may not be installed or properly linked.
   - **Environment Issues:** There may be issues with the environment variables, such as `LD_LIBRARY_PATH`, which could affect how libraries are loaded.

---

3. **Steps to Resolve the Issue:**
   - **Check Library Installation:**
     - Ensure that the `bzstd` library is correctly installed. You can reinstall it using your package manager. For example, on Ubuntu, you can run:
       ```bash
       sudo apt-get install --reinstall libbzstd1
       ```
   - **Verify Dependencies:**
     - Use the `ldd` command to check if all dependencies of the executable are met:
       ```bash
       ldd /path/to/your/executable
       ```
     - Look for any "not found" messages in the output, which indicate missing libraries.
   - **Install Missing Libraries:**
     - If `getgrgid_r` is missing, ensure that the necessary libraries are installed. On many Linux systems, this function is part of the `libc` library, which should be present by default. If it's missing, you may need to reinstall the `glibc` package.
   - **Check Environment Variables:**
     - Ensure that your `LD_LIBRARY_PATH` includes the directories where your libraries are located. You can check this by running:
       ```bash
       echo $LD_LIBRARY_PATH
       ```
     - If necessary, you can set it temporarily in your terminal:
       ```bash
       export LD_LIBRARY_PATH=/path/to/your/libraries:$LD_LIBRARY_PATH
       ```

---

**Example:**
If you are trying to run a program that uses `bzstd` and you encounter this error, follow the steps above to check the installation and dependencies. For instance, if you find that `libbzstd.so.1` is missing, reinstall it using your package manager.

---

**References:**
## https://man7.org/linux/man-pages/man3/getgrgid_r.3.html
## https://linux.die.net/man/3/bzstd
## https://linux.die.net/man/1/ldd