cmake_minimum_required(VERSION 3.12)

project(opengl_framework VERSION 1.0.0 LANGUAGES C CXX)

set(CMAKE_DEBUG_POSTFIX d)
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib/opengl_framework-${PROJECT_VERSION}")

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "" FORCE)
endif()

set(CMAKE_CXX_STANDARD 17)

file(GLOB SOURCES src/*.c src/*.cpp)

add_library(opengl_framework ${SOURCES})

target_include_directories(opengl_framework
PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include/opengl_framework-${PROJECT_VERSION}>
)

install(TARGETS opengl_framework
    EXPORT opengl_framework-targets
    DESTINATION lib/opengl_framework-${PROJECT_VERSION}
)

install(DIRECTORY include/
    DESTINATION include/opengl_framework-${PROJECT_VERSION}
)

install(EXPORT opengl_framework-targets
    FILE opengl_framework-targets.cmake
    NAMESPACE opengl_framework::
    DESTINATION lib/cmake/opengl_framework-${PROJECT_VERSION}
)

include(CMakePackageConfigHelpers)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/opengl_framework-config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/opengl_framework-config.cmake"
    INSTALL_DESTINATION lib/cmake/opengl_framework-${PROJECT_VERSION}
    NO_SET_AND_CHECK_MACRO
    NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/opengl_framework-config-version.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/opengl_framework-config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/opengl_framework-config-version.cmake"
    DESTINATION lib/cmake/opengl_framework-${PROJECT_VERSION}
)

export(EXPORT opengl_framework-targets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/opengl_framework-targets.cmake"
    NAMESPACE opengl_framework::
)
