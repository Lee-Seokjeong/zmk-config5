zephyr_include_directories(.)

# Include the device tree bindings
zephyr_library()

zephyr_library_sources(
  sensor/pimoroni_trackball.c  # 여기에 적절한 드라이버 소스 파일을 추가합니다
)
