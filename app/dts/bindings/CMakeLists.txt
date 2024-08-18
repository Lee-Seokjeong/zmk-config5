zephyr_include_directories(.)

# Include the device tree bindings
zephyr_library()

# Include all header files in the zmk directory
zephyr_include_directories(zmk)
