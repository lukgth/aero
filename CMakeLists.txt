cmake_minimum_required(VERSION 3.16)

project(aero_windows7_virtio_common C)

option(AERO_VIRTIO_BUILD_TESTS "Build host-side virtio common tests" ON)
option(AERO_AEROGPU_BUILD_TESTS "Build host-side AeroGPU command stream tests" ON)

if (AERO_AEROGPU_BUILD_TESTS)
    enable_language(CXX)
endif ()

if (AERO_VIRTIO_BUILD_TESTS OR AERO_AEROGPU_BUILD_TESTS)
    enable_testing()
endif ()

if (AERO_VIRTIO_BUILD_TESTS)
    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows7/virtio/common/tests/CMakeLists.txt")
        add_subdirectory(drivers/windows7/virtio/common/tests)
    endif ()

    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows7/virtio-snd/tests/CMakeLists.txt")
        add_subdirectory(drivers/windows7/virtio-snd/tests)
    endif ()

    # virtio-input tests include <ntddk.h> (kernel-mode headers) unconditionally
    # on Windows via the source files' include chains.  A regular CMake/MSVC build
    # cannot compile kernel-mode headers without the full WDK project system
    # (WDK property sheets, _AMD64_/_X86_ defines, kernel-mode CRT, etc.), so
    # these tests are skipped on Windows host builds.
    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows7/virtio-input/tests/CMakeLists.txt" AND NOT WIN32)
        add_subdirectory(drivers/windows7/virtio-input/tests)
    endif ()
    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows7/virtio-net/tests/CMakeLists.txt")
        add_subdirectory(drivers/windows7/virtio-net/tests)
    endif ()

    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows/virtio/common/tests/CMakeLists.txt")
        add_subdirectory(drivers/windows/virtio/common/tests)
    endif ()

    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows/virtio/pci-modern/tests/CMakeLists.txt")
        add_subdirectory(drivers/windows/virtio/pci-modern/tests)
    endif ()

    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/windows/virtio/kmdf/tests/CMakeLists.txt")
        add_subdirectory(drivers/windows/virtio/kmdf/tests)
    endif ()

    if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/drivers/win7/virtio/tests/CMakeLists.txt")
        add_subdirectory(drivers/win7/virtio/tests)
    endif ()
endif ()

if (AERO_AEROGPU_BUILD_TESTS)
    add_subdirectory(drivers/aerogpu/umd/d3d9/tests)
    add_subdirectory(drivers/aerogpu/umd/d3d10_11/tests)
endif ()
