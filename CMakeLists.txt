cmake_minimum_required(VERSION 3.17)

project(Project)

set(CMAKE_FIND_ROOT_PATH ${CMAKE_BINARY_DIR})

find_package(cmake_utils REQUIRED)
find_package(zlib REQUIRED)

include(${cmake_utils_PACKAGE_FOLDER_DEBUG}${cmake_utils_PACKAGE_FOLDER_RELEASE}/global_settings_thirdparty.cmake)

# Force find_package() to look in the Conan package directories first
set(CMAKE_FIND_ROOT_PATH ${CONAN_CMAKE_MODULE_PATH} ${CMAKE_FIND_ROOT_PATH})

option(PNG_SHARED "Build shared lib" OFF)

# Workaround for this issue:
# https://sourceforge.net/p/libpng/bugs/281/
if(IOS AND CMAKE_OSX_ARCHITECTURES MATCHES arm)
    set(CMAKE_SYSTEM_PROCESSOR arm)
    set(PNG_ARM_NEON off CACHE STRING "")
endif()

add_subdirectory(source)

target_link_libraries(png_static
    zlib::zlib
)

set_target_properties(png_static PROPERTIES DEBUG_POSTFIX d)
