# Include the Zephyr build system
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})

# Add source files for the module
target_sources(app PRIVATE
    src/trackball_pim447.c
)

# Add the include directories for the module
target_include_directories(app PRIVATE
    ${ZEPHYR_BASE}/include
    ${CMAKE_SOURCE_DIR}/include
    ${CMAKE_SOURCE_DIR}/include/dt-bindings
)
