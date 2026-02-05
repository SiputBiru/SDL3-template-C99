# Add the SDL3 subdirectory from the git submodule
# I disable SDL tests to speed up the build
set(SDL_TEST OFF CACHE BOOL "" FORCE)
add_subdirectory(sdl)

# Create a clean interface target for our project to link against
add_library(vendor INTERFACE)

# Link SDL3 to our vendor interface.
# SDL3::SDL3 is the alias target provided by SDL's CMake.
target_link_libraries(vendor INTERFACE SDL3::SDL3)
