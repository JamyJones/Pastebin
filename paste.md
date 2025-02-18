## Summary: Importance of Learning CMake Even While Using GitHub Actions
---  
CMake is a versatile, cross-platform tool primarily used to manage build processes in software development. While GitHub Actions automates workflows (e.g., CI/CD pipelines), CMake provides integral build configuration and compilation operations. If you're wondering why learning CMake is still relevant despite relying heavily on GitHub Actions in development workflows, this explanation will help clarify. 

---

### 1. **CMake's Primary Purpose: Build Systems vs CI/CD**  
CMake and GitHub Actions serve different purposes:
- **CMake**: A tool to **generate build files**. Its primary role is managing cross-platform builds (e.g., creating Makefiles for Unix builds or Visual Studio project files for Windows). It simplifies dealing with dependencies, modules, and custom configurations.
    - Typical use: Defining how to build, configure, and link your software.
    - Example: Creating a `CMakeLists.txt` file to define how your project should be compiled.  

- **GitHub Actions**: A tool to **automate CI/CD workflows**, which often include running CMake commands. 
    - Typical use: Automating tests, builds, or deployments as part of a pipeline.
    - Example: Running CMake commands in a workflow file to build and test the project on multiple platforms.

Even though GitHub Actions can execute build commands (like CMake or Make), understanding the build system itself (CMake) is crucial because **actionable workflows cannot exist without properly configured build systems**.  

---

### 2. **Cross-Platform Builds and Handling Complexity**  
CMake is widely used in projects that need to run across several platforms. By learning CMake, you can:
- Write cross-platform build configurations.
- Handle OS-specific systems like Windows, Linux, or macOS seamlessly.  
- Specify how dependencies (e.g., libraries or third-party modules) are introduced into the build process.

If you're unfamiliar with tools like CMake, you are limited in how you define and debug cross-platform build issues. These configurations are often written in `CMakeLists.txt`, which GitHub Actions then references.

---

### 3. **Integration with GitHub Actions**  
GitHub Actions often relies on low-level tools like CMake to execute tasks. For example:
- In a workflow file (`.github/workflows/build.yml`), you might run:
    ```yaml
    - name: Configure CMake
      run: cmake -S . -B build
    - name: Build the project
      run: cmake --build build
    ```
    Here, **CMake** is invoked to configure and compile the code before running tests.  
    **Why learn?** If you don't understand CMake, debugging pipeline failures would be harder (e.g., wrong dependency setup in `CMakeLists.txt`, incorrect flags, etc.).

---

### 4. **Mastery Improves Debugging and Customization**  
When pipelines fail in GitHub Actions, the problem often arises from the core tools used for building or testing (like CMake). For instance:
- **Issue**: Your build workflow fails with "missing library" error.  
    - **Without CMake knowledge**: You can't effectively debug the `CMakeLists.txt` file.  
    - **With CMake knowledge**: You can inspect the CMake configuration to resolve dependency issues or revise settings.
  
Additionally, advanced customizations (e.g., custom build commands or linking special libraries) are much easier when you know how CMake works.

---

### 5. **Improved Collaboration in Development Teams**  
Many development teams and projects use CMake for their build systems. If you're working in collaborative environments where CMake defines the core builds, understanding it allows you to:
- Contribute effectively.
- Reduce reliance on others to resolve CMake-related issues.
- Leverage GitHub Actions better, as you'll have full control of build systems.

---

### Practical Example  
Here’s how GitHub Actions and CMake interplay in practice:

**`CMakeLists.txt` (CMake Build File):**
```cmake
cmake_minimum_required(VERSION 3.15) # Set minimum CMake version.
project(MyApp LANGUAGES CXX)          # Define the project and programming language.

set(CMAKE_CXX_STANDARD 17)           # Specify C++ standard to use.
add_executable(MyApp main.cpp)        # Add an executable target for the main file.
```

**GitHub Actions Workflow (`build.yml`):**
```yaml
name: Build and Test
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install dependencies
        run: sudo apt-get update && sudo apt-get install cmake -y

      - name: Configure with CMake
        run: cmake -S . -B build

      - name: Build project
        run: cmake --build build

      - name: Run tests
        run: ctest --test-dir build
```

**Key Takeaway**: Without CMake knowledge, understanding or debugging this workflow would be nearly impossible. 

---

### References 
## 
- https://cmake.org/
- https://docs.github.com/en/actions
- https://cliutils.gitlab.io/modern-cmake/
## 