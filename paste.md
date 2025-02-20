## Summary: How to Fix the Error: "CANNOT LINK EXECUTABLE: cannot locate symbol..." <br>
---<br>
This error typically occurs when a symbol or function required by your application cannot be resolved (or linked) to the underlying libraries during runtime. Specifically, in this case, the error message suggests that the missing symbol is related to the C++ Standard Library (specifically for `std::basic_istringstream`), which is used for stream operations in C++.

Below, I explain the causes, related factors, and steps to resolve this error in detail.

---

## Explanation

1. **Understanding What the Error Means**
   - The error message `CANNOT LINK EXECUTABLE` occurs when the dynamic linker/loader cannot find the proper symbols or functions that are expected to be present in the provided libraries.
   - `_ZTTNSt6__ndk119basic_istringstreamIcNS_11char_traitsIcEENS_9allocatorIcEEEE` represents a **mangled name** (a standardized way compilers encode type and function names). This mangled symbol translates to `std::istringstream`, which is part of the C++ Standard Library and is responsible for handling input streams.
   - `ndk` in the namespace refers to the **Android NDK** (Native Development Kit), which provides libraries for C++ development on Android platforms.

2. **Possible Causes for the Error**
   - **Mismatch in C++ Runtime Library Versions**:
     - If your application was compiled with a version of the C++ Standard Library that is not compatible with the version present on the target device, the linker cannot find the required symbols.
   - **Missing or Incorrectly Linked Libraries**:
     - The required C++ library (`libc++_shared.so` or `libstdc++.so`) is either missing or not being linked correctly during runtime.
   - **NDK Version Differences**:
     - An older Android NDK was used during development, which may not support certain modern C++ features required by your app’s code or dependencies.
   - **Platform API Level Issues**:
     - The app may be targeting an Android API level that doesn't support certain symbols in the C++ Standard Library or NDK libraries.

---

## Steps to Fix the Error

Here are the steps to resolve this issue:

1. **Verify Linking to the Proper C++ Standard Library**
   - Ensure you are building the application with the correct C++ Standard Library.
   - In your `CMakeLists.txt` or build system configuration, you should explicitly link against the **C++ shared library** (`libc++_shared.so`) provided by the Android NDK.
     ```cmake
     set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
     set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -stdlib=libc++")
     ```
   - If you're using Gradle, you must set the runtime library to `c++_shared` in the `externalNativeBuild` configuration of your `build.gradle` file:
     ```gradle
     externalNativeBuild {
         cmake {
             arguments "-DANDROID_STL=c++_shared"
         }
     }
     ```

2. **Use the Latest Android NDK**
   - Download and use the latest version of the Android NDK from the official [Android NDK page](https://developer.android.com/ndk).
     - Make sure the version being used is compatible with your Android API level and the features utilized in your application code (e.g., C++17 or higher).
   - To avoid compatibility issues, make sure your build tools align with the latest libraries provided by the NDK.

3. **Set the Correct `minSdkVersion` and `targetSdkVersion`**
   - In your `build.gradle` file, ensure that the `minSdkVersion` and `targetSdkVersion` are set to values supported by your application (use API level 21 or higher for better C++ library support):
     ```gradle
     android {
         defaultConfig {
             minSdkVersion 21
             targetSdkVersion 31
         }
     }
     ```

4. **Ensure Proper Runtime Packaging**
   - If your application uses a shared C++ library (e.g., `libc++_shared.so`), ensure it is packaged correctly during APK or bundle creation.
     - Include the `.so` file in the `libs` directory or through your Gradle configuration to be part of the `jniLibs` folder:
       ```gradle
       sourceSets {
           main {
               jniLibs.srcDirs = ['libs']
           }
       }
       ```

5. **Analyze the Source of the Issue with `nm` or `readelf`**
   - Use the `nm` or `readelf` tools to examine your libraries (`.so`) and verify if the required symbol (`_ZTT...`) exists in the C++ runtime library you provide.
     - Example command:
       ```sh
       nm -C libc++_shared.so | grep std::istringstream
       ```
     If the symbol does not appear, it indicates that the library version is incompatible, and you should replace it with a matching version.

6. **Clean and Rebuild the Project**
   - Sometimes, library conflicts arise from stale build files. Clean your project and rebuild everything from scratch:
     - In Gradle: `./gradlew clean build`
     - In CMake: Delete your build directory and reconfigure CMake.

---

## Example: Updated CMakeLists.txt for Correct Linking

Here’s an example of how your CMake configuration might look to ensure correct linking to the C++ Standard Library:

```cmake
# Set the required C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Configure the Android toolchain and target
set(CMAKE_TOOLCHAIN_FILE ${ANDROID_NDK}/build/cmake/android.toolchain.cmake)
set(ANDROID_STL c++_shared)  # Use the shared C++ library

# Include source files
add_library(my_library SHARED my_code.cpp)

# Link against the C++ shared library
target_link_libraries(my_library
    c++_shared
    log           # Android platform log library (if needed)
)
```

---

## References:

- Android NDK Docs: https://developer.android.com/ndk
- C++ ABI Compatibility Issues: https://gcc.gnu.org/onlinedocs/libstdc++/manual/using.html