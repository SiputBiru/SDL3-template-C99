cmake_minimum_required(VERSION 3.16)

# Initialize project with C language support only
project(SDL3_Tutorial LANGUAGES C)

# Force C99 Standard
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Optional: Output binaries to a single folder to fix DLL issues on Windows
# Video reference [00:15:48]
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_subdirectory(vendor)
add_subdirectory(src)
