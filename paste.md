## Summary
The error `"CANNOT LINK EXECUTABLE: cannot locate symbol '...' referenced by ..."` typically occurs when there's a **linking problem with native libraries** in an Android application. This indicates that one or more symbols (functions, variables, or objects) expected at runtime are not present due to ABI (Application Binary Interface) mismatches, incompatible libraries, or incorrectly built native code.

In this particular case, the missing symbol `_ZTTNSt6__ndk119basic_istringstreamIcNS_11char_traitsIcEENS_9allocatorIcEEEE` is related to Standard Template Library (**STL**) objects, specifically `std::basic_istringstream` from the C++ standard library.

---

## Explanation
1. **Reason for the Error**
   - Android uses the **Bionic libc** as its standard C library instead of the GNU libc used in traditional Linux or UNIX systems. To include C++ standard concepts like `basic_istringstream` in your app, you need an appropriate, **compatible runtime library**.
   - If the symbol (_ZTT...) is missing during linking, here are the possible reasons:
     - Incorrect or mismatched **NDK versions** (e.g., using an older NDK with a newer C++ library).
     - The wrong **Standard C++ Library** is not included (e.g., libc++ vs. gnustl vs. stlport).
     - The C++ standard version (e.g., C++11, C++14, C++17) used in your project isn't compatible.

---

2. **Breakdown of Symbol Name**
   - `_ZTT` is part of the Itanium C++ ABI (Application Binary Interface) denoting the **typeinfo virtual table**.
   - `NSt6__ndk1` indicates **NDK STL implementation** (`std` namespace in libc++).
   - `basic_istringstream` is the class in the `<sstream>` header.
   - The remaining parts describe the template instantiation (e.g., `char` as `Ic`, `std::char_traits`, and allocators).

This all means that the compiled application/project expected `std::istringstream` but couldn't find it in the linked library.

---

## Steps to Resolve the Issue
Here's how to fix this issue depending on the setup you're working with:

### **1. Use the Correct C++ Standard Library**
Ensure that you're using the `libc++` standard library (modern and recommended for NDK builds):
- Open your **CMakeLists.txt** or **Android.mk** file (depending on your build system).
- Specify `libc++`:
  ```cmake
  set(CMAKE_CXX_STANDARD 17) # Ensure you're using C++17 or the appropriate version
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
  ```
  Alternatively, you can use the native toolchain:
  ```make
  APP_STL := c++_shared
  ```
- Ensure that your native code links the required `libc++_shared.so` or `libc++_static.a`.

---

### **2. Update the Android NDK**
Older NDK versions may not correctly support modern C++ libraries. Update to the latest version of the NDK. You can set the version in your project's `build.gradle` file:

```gradle
android {
    ndkVersion "25.2.9519653" // Change to your desired/latest NDK version
}
```

---

### **3. Ensure Library Compatibility**
Review your dependencies and confirm that:
- Any prebuilt libraries you're using were compiled with the same/compatible NDK, target architecture (e.g., `arm64-v8a`, `armeabi-v7a`), and C++ standard library (libc++).
- If using third-party libraries, try rebuilding them with the NDK you're using to avoid ABI mismatches.

---

### **4. Check for Missing Headers and Include Proper Files**
Ensure the required include headers (`<sstream>` and associated headers) are correctly added:
```cpp
#include <sstream>
```
Also, check for proper usage of types like `std::istringstream`.

---

### **5. Clean and Rebuild the Project**
Sometimes leftover artifacts from a dirty build cause unexpected runtime issues. Perform the following:
```bash
./gradlew clean
./gradlew build
```

---

### **6. Switch to `c++_static` If Necessary**
If you're encountering the issue with the shared library (`c++_shared`), switch to the static version to bundle the library directly into your `.so` file:
- In your `CMakeLists.txt`:
  ```cmake
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -static-libstdc++")
  ```

---

### Example:
Here's an example configuration for `CMakeLists.txt`:

```cmake
cmake_minimum_required(VERSION 3.10.2)
project("example_project")

# Adding the library
add_library(native-lib
            SHARED
            src/main/cpp/native-lib.cpp)

# Specify the NDK's C++ library (libc++)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")

# Link the native library
find_library(log-lib log)

target_link_libraries(native-lib ${log-lib} c++)
```

---

## Additional Debugging Tips
1. Enable verbose logging in the Gradle build to help analyze linking issues:
   ```bash
   ./gradlew build --info
   ```
2. Use **readelf** or other similar tools to inspect the symbols in your native libraries. You can verify whether the expected `_ZTTNSt6__ndk1...` symbols exist.
   ```bash
   readelf -Ws libnative-lib.so | grep _ZTTNSt6__ndk1
   ```
3. If debugging fails and you're using custom libraries, try substituting `std::istringstream` with alternatives for quick troubleshooting.

---

### References:
## https://developer.android.com/ndk | https://cplusplus.com/reference/sstream/istringstream