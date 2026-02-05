# Create the executable from main.c
add_executable(Game main.c)

# Link to the vendor interface we created
target_link_libraries(Game PRIVATE vendor)
