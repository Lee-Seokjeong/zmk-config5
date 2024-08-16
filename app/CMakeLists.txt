cmake_minimum_required(VERSION 3.13.1)

# Find Zephyr. This also loads Zephyr's build system.
find_package(Zephyr REQUIRED HINTS ${CMAKE_SOURCE_DIR}/../zephyr)
project(zmk)

# Define the paths
set(ZMK_APP_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/app/include")
set(DTS_BINDINGS_DIR "${CMAKE_SOURCE_DIR}/app/include/dt-bindings")

# Add the bindings directory to the search path
include_directories(${ZMK_APP_INCLUDE_DIR})
include_directories(${DTS_BINDINGS_DIR})

# Add custom Zephyr module and directories
list(APPEND DTS_ROOT ${CMAKE_SOURCE_DIR}/drivers/zephyr)
list(APPEND ZEPHYR_EXTRA_MODULES ${CMAKE_SOURCE_DIR}/drivers)

# trackball_pim447 소스 파일 추가
zephyr_library_sources(
    src/trackball_pim447.c
)

include(cmake/zmk_config.cmake)

# Add source files to the "app" target
target_sources(app PRIVATE src/activity.c src/kscan.c src/matrix_transform.c src/hid.c src/mouse/key_listener.c src/mouse/main.c src/mouse/tick_listener.c)

# Add the trackball source file conditionally
if(CONFIG_ZMK_TRACKBALL_PIM447)
    target_sources(app PRIVATE src/mouse/trackball_pim447.c)
endif()

target_sources(app PRIVATE src/sensors.c src/event_manager.c src/events/activity_state_changed.c src/events/position_state_changed.c src/events/layer_state_changed.c src/events/keycode_state_changed.c src/events/modifiers_state_changed.c src/events/endpoint_selection_changed.c src/events/sensor_event.c src/events/mouse_button_state_changed.c src/events/mouse_move_state_changed.c src/events/mouse_tick.c src/events/mouse_scroll_state_changed.c src/combo.c src/keymap.c)

if (CONFIG_ZMK_SPLIT OR CONFIG_ZMK_SPLIT_BLE_ROLE_CENTRAL)
    target_sources(app PRIVATE src/behaviors/behavior_key_press.c src/behaviors/behavior_reset.c src/behaviors/behavior_hold_tap.c src/behaviors/behavior_sticky_key.c src/behaviors/behavior_momentary_layer.c src/behaviors/behavior_mod_morph.c src/behaviors/behavior_outputs.c src/behaviors/behavior_toggle_layer.c src/behaviors/behavior_to_layer.c src/behaviors/behavior_transparent.c src/behaviors/behavior_none.c src/behaviors/behavior_sensor_rotate_key_press.c src/behaviors/behavior_mouse_key_press.c src/behaviors/behavior_mouse_move.c src/behaviors/behavior_mouse_scroll.c src/behaviors/behavior_ext_power.c src/combo.c src/keymap.c)
endif()

if (CONFIG_ZMK_RGB_UNDERGLOW)
    target_sources(app PRIVATE src/behaviors/behavior_rgb_underglow.c)
endif()

if (CONFIG_ZMK_BLE)
    target_sources(app PRIVATE src/behaviors/behavior_bt.c src/ble.c src/battery.c src/hog.c)
endif()

if (CONFIG_USB)
    target_sources(app PRIVATE src/usb.c)
endif()

if (CONFIG_ZMK_SPLIT_BLE AND (NOT CONFIG_ZMK_SPLIT_BLE_ROLE_CENTRAL))
    target_sources(app PRIVATE src/split_listener.c src/split/bluetooth/service.c)
endif()

if (CONFIG_ZMK_SPLIT_BLE AND CONFIG_ZMK_SPLIT_BLE_ROLE_CENTRAL)
    target_sources(app PRIVATE src/split/bluetooth/central.c)
endif()

target_sources(app PRIVATE src/endpoints.c src/hid_listener.c src/main.c)

add_subdirectory(src/display/)

zephyr_cc_option(-Wfatal-errors)
