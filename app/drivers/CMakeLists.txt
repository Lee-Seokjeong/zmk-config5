# Copyright (c) 2020 The ZMK Contributors
# SPDX-License-Identifier: MIT

add_subdirectory(gpio)
add_subdirectory(kscan)
add_subdirectory(sensor)
add_subdirectory(display)

# 추가: trackball 드라이버를 포함합니다.
add_subdirectory(trackball)
