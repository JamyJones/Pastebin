## Summary: Using `patchelf` to Replace Dependencies of a Shared Library <br>
---
In many Linux systems, shared libraries often depend on other libraries for their functionality. Sometimes, you may need to modify these dependencies — for instance, to change the location of a dependency file or to point the library to a different version of a dependency. `patchelf` is a command-line utility that allows you to manipulate ELF (Executable and Linkable Format) binaries, including modifying the dependencies of shared libraries.

Let's dive into **how `patchelf` works** for replacing dependencies of a shared library.

---
## Explanation:

### 1. **What is `patchelf`?**
   - `patchelf` is a UNIX/Linux tool used to modify ELF binaries.
   - It can change properties like the `RPATH` (runtime library search path), `SONAME` (shared object name), and replace or remove library dependencies.
   - It's mostly used where recompilation of binaries is not feasible but modifications to dynamically linked libraries are needed.

---

### 2. **Identifying Dependencies in a Shared Library**
   Before modifying anything, you need to check the current dependencies of a shared library to confirm what needs to be replaced. You can use the `ldd` command to list the dependencies of a shared library.

   ```bash
   ldd <shared_library.so>
   ```
   Example Output:
   ```
   libdependency.so.1 => /usr/lib/libdependency.so.1 (0x00007f91c3f52000)
   libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f91c3bb5000)
   ```
   This output shows which libraries the shared library depends on.

---

### 3. **Using `patchelf` to Replace Dependencies**
   Once you've identified the current dependencies, you can use `patchelf` to replace or update them.

   #### Syntax:
   ```bash
   patchelf --replace-needed <old_dependency> <new_dependency> <shared_library>
   ```
   - **`<old_dependency>`**: The name of the dependency to replace (e.g., `libdependency.so.1`).
   - **`<new_dependency>`**: The new dependency to use (e.g., `/opt/custom/libdependency.so.2`).
   - **`<shared_library>`**: The shared library you want to modify.

   #### Example:
   Suppose you have a shared library named `mylibrary.so` that relies on `libdependency.so.1`, and you want to replace this dependency with `libcustomdependency.so.2` located in `/home/user/custom-libs`.

   Run:
   ```bash
   patchelf --replace-needed libdependency.so.1 /home/user/custom-libs/libcustomdependency.so.2 mylibrary.so
   ```
   After running this command, `mylibrary.so` will now load `libcustomdependency.so.2` instead of `libdependency.so.1`.

---

### 4. **Specifying a Custom Search Path (`RPATH`)**
   Shared libraries use the `RPATH` to find their dependencies at runtime. If your new dependency is located in a non-standard directory, you may also need to update the `RPATH`.

   #### Syntax:
   ```bash
   patchelf --set-rpath <new_rpath> <shared_library>
   ```
   - **`<new_rpath>`**: Path to the directory containing the new dependencies (e.g., `/home/user/custom-libs`).

   #### Example:
   To modify the `RPATH` of `mylibrary.so` so it looks for dependencies in `/home/user/custom-libs`, run:
   ```bash
   patchelf --set-rpath /home/user/custom-libs mylibrary.so
   ```

   You can verify the change with:
   ```bash
   patchelf --print-rpath mylibrary.so
   ```

---

### 5. **Verifying Changes**
   After making modifications, you can verify the shared library's dependencies again using the `ldd` command to ensure that the new dependency paths have been applied successfully:
   ```bash
   ldd mylibrary.so
   ```

---

## Example: Practical Workflow
### Consider this Scenario:
- Original shared library: `mylibrary.so`
- Original dependency: `/usr/lib/liboriginaldependency.so.1`
- New dependency: `/home/user/libs/libnewdependency.so.2`

#### Step-by-step Actions:
1. **Check the current dependencies**:
   ```bash
   ldd mylibrary.so
   ```
   Output:
   ```
   liboriginaldependency.so.1 => /usr/lib/liboriginaldependency.so.1
   ```

2. **Replace the dependency**:
   ```bash
   patchelf --replace-needed liboriginaldependency.so.1 /home/user/libs/libnewdependency.so.2 mylibrary.so
   ```

3. **Update the `RPATH`** (if required):
   ```bash
   patchelf --set-rpath /home/user/libs mylibrary.so
   ```

4. **Verify the changes**:
   ```bash
   ldd mylibrary.so
   ```
   Updated output:
   ```
   libnewdependency.so.2 => /home/user/libs/libnewdependency.so.2
   ```

---

## References
## https://github.com/NixOS/patchelf 
