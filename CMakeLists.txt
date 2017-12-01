cmake_minimum_required(VERSION 3.8)
project(skia_playground)

set(CMAKE_CXX_STANDARD 14)

file(GLOB SKIA_INCLUDE_SUBPATHS ${CMAKE_SOURCE_DIR}/include/skia/*)
foreach(SKIA_INCLUDE_SUBPATH ${SKIA_INCLUDE_SUBPATHS})
    include_directories(SYSTEM ${SKIA_INCLUDE_SUBPATH})
endforeach()

find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS})

find_package(OpenGL)
include_directories(${OPENGL_INCLUDE_DIR})

set(SOURCE_FILES main.cpp)
add_executable(skia_playground ${SOURCE_FILES})
target_link_libraries(skia_playground ${CMAKE_SOURCE_DIR}/lib/libskia.a)
target_link_libraries(skia_playground ${OPENGL_gl_LIBRARY})
target_link_libraries(skia_playground ${SDL2_LIBRARIES})
target_link_libraries(skia_playground "-framework CoreFoundation -framework CoreGraphics -framework CoreText")

add_custom_command(
        TARGET skia_playground
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/res ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Copying resources to build directory")
