# SDL3 Template projects

SDL3 C99 Starter Project

A modern C99 project template using SDL3 as a git submodule and CMake for build management.

## Prerequisites

- **CMake**: Version 3.16 or newer.
- **C Compiler**: GCC, Clang, or MSVC (Visual Studio).
- **Git**: For managing the project and submodules.

## How to Build and Run

### 1. Start a New Project

**Option A: Use as GitHub Template (Recommended)**

1. Click the green **"Use this template"** button at the top of this page to create a new repository under your own account.

2. Clone your newly created repository (don't forget the `--recursive` flag to grab SDL3!):

```bash
git clone --recursive https://github.com/YOUR_USERNAME/YOUR_PROJECT_NAME.git

```

**Option B: Manual Clone If you are cloning this repository directly to contribute or test:**

```bash
git clone --recursive <this-repo-url>

```

Note: If you forgot `--recursive` when cloning, run this inside the folder:

```bash
git submodule update --init --recursive
```

### 2. Configure the Project

Run CMake to generate the build files. We use -B build to keep build artifacts in a separate folder.

```bash
cmake -B build
```

### 3. Build the Project

Compile the code.

```

cmake --build build
```

### 4. Run the Executable

The executable will be placed in build/bin (or build/Debug/bin on Windows/Visual Studio).

```bash
# Linux / macOS

./build/bin/Game

# Windows

.\build\bin\Game.exe
```

## How to Add More Vendor Libraries

This project uses a "Vendor Interface" pattern. All third-party libraries are aggregated into a single CMake target called vendor. To add a new library (e.g., cglm for math or nuklear for UI), follow these steps:

### Step 1: Add the Git Submodule

Open our terminal in the project root and add the library to the vendor folder.

#### Example: Adding cglm (OpenGL Math library for C)

```bash
git submodule add [https://github.com/recp/cglm.git](https://github.com/recp/cglm.git) vendor/cglm
```

### Step 2: Update `vendor/CMakeLists.txt`

Open `vendor/CMakeLists.txt` and add the new directory and link it to the interface.

```CMake
# ... existing SDL3 setup ...

# 1. Add the subdirectory
add_subdirectory(cglm)

# 2. Link the new library to the 'vendor' interface
# (Check the library's documentation to find its specific target name.
#  For cglm, it is usually 'cglm')
target_link_libraries(vendor INTERFACE cglm)
```

### Step 3: Use it in our code

Because our main executable links to `vendor`, it automatically gets access to the include paths and binaries of the new library.

In `src/main.c`:

```C
#include <SDL3/SDL.h>
#include <cglm/cglm.h> // Now we can use it

int main(int argc, char\* argv[]) {
vec3 v = {1.0f, 2.0f, 3.0f}; // Using cglm types
// ... rest of our code
}

```

### Step 4: Re-run CMake

Whenever we add a new subdirectory, we must re-generate the CMake cache.

```bash
cmake -B build
```

### Tips for this workflow:

1.  **Finding Target Names**: When adding a new vendor (Step 2), the hardest part is often guessing the library name. we can usually find this by looking at the library's own `CMakeLists.txt` file and looking for `add_library(NAME ...)`. That `NAME` is what we put in `target_link_libraries`.

2.  **Header Only Libraries**: If we add a header-only library (like `stb_image`), we don't need `add_subdirectory`. we can create an interface library in our vendor CMake file that just points to the include folder.
