cmake_minimum_required(VERSION 3.5)

set(DESKTOP_NANOVGDEMO_TARGET nanovgdemo)

set(DESKTOP_NANOVGDEMO_SOURCES
        demo.c
        perf.c
        example_gl2.c
        )

file(GLOB_RECURSE RESOURCE_IMAGES images/*.jpg)

set_source_files_properties(
        ${RESOURCE_IMAGES}
        PROPERTIES
        MACOSX_PACKAGE_LOCATION "example/images"
)

file(GLOB EXTRA_RESOURCES
        entypo.ttf
        NotoEmoji-Regular.ttf
        Roboto-Bold.ttf
        Roboto-Light.ttf
        Roboto-Regular.ttf
        )

set_source_files_properties(
        ${EXTRA_RESOURCES}
        PROPERTIES
        MACOSX_PACKAGE_LOCATION "example"
)

add_executable(${DESKTOP_NANOVGDEMO_TARGET} MACOSX_BUNDLE ${DESKTOP_NANOVGDEMO_SOURCES} ${EXTRA_RESOURCES} ${RESOURCE_IMAGES})
target_link_libraries(${DESKTOP_NANOVGDEMO_TARGET} ${NAME})
target_link_libraries(${DESKTOP_NANOVGDEMO_TARGET} glfw)
target_link_libraries(${DESKTOP_NANOVGDEMO_TARGET} freetype)
# Make full path includes work for project headers
target_include_directories(${DESKTOP_NANOVGDEMO_TARGET} PRIVATE ${PROJECT_SOURCE_DIR}/src)
target_include_directories(${NAME} PUBLIC ${PROJECT_SOURCE_DIR}/../freetype2/include)


if (APPLE)
    set_target_properties(${DESKTOP_NANOVGDEMO_TARGET} PROPERTIES MACOSX_BUNDLE_BUNDLE_NAME "NanoVG Demo")

    set_target_properties(${DESKTOP_NANOVGDEMO_TARGET} PROPERTIES
            MACOSX_BUNDLE_SHORT_VERSION_STRING ${GLFW_VERSION}
            MACOSX_BUNDLE_LONG_VERSION_STRING ${GLFW_VERSION_FULL}
            MACOSX_BUNDLE_INFO_PLIST "${GLFW_SOURCE_DIR}/CMake/MacOSXBundleInfo.plist.in")
endif()
