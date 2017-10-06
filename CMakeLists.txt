cmake_minimum_required(VERSION 3.5)
project(nanovg)
set(NAME nanovg)

set(HEADERS_PATH "${PROJECT_SOURCE_DIR}/src")
set(SOURCES_PATH "${PROJECT_SOURCE_DIR}/src")

file(GLOB_RECURSE headers "${HEADERS_PATH}/*.h")
file(GLOB_RECURSE sources "${SOURCES_PATH}/*.c")

add_library(${NAME}
        SHARED
        ${headers}
        ${sources}
        )

target_include_directories(${NAME} PUBLIC ${HEADERS_PATH})

# Link OpenGL with the lib
if (APPLE AND NOT IOS)
    find_library(OpenGL_LIBRARY OpenGL)
    mark_as_advanced(OpenGL_LIBRARY)
    set(EXTRA_LIBS ${OpenGL_LIBRARY})
    target_link_libraries(${NAME} ${EXTRA_LIBS})
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    target_link_libraries (${NAME} glew)
endif()

target_compile_features(${NAME} PUBLIC cxx_strong_enums)

#--------------------------------------------------------------------
# Add subdirectories
#--------------------------------------------------------------------
add_subdirectory(example)
