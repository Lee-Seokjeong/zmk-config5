zephyr_include_directories(.)

# Include the device tree bindings
zephyr_library()

zephyr_library_sources(
  # 바인딩 파일에 대한 헤더 파일을 포함합니다.
)

# 해당 디렉토리에서 모든 헤더 파일을 포함할 수 있습니다.
zephyr_library_sources(
  zmk/trackball_pim447.h  # 헤더 파일이 올바르게 위치하고 있습니다
)
