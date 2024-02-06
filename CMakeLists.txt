cmake_minimum_required( VERSION 3.28 FATAL_ERROR )

project(
    imgui
    DESCRIPTION "Dear ImGui using GLFW and OpenGL3"
    HOMEPAGE_URL "https://github.com/gurki/"
    LANGUAGES CXX
)

option( BUILD_IMNODES "Inlude ImNodes" ON )

add_library( ${PROJECT_NAME} STATIC
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
)

add_library( ${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME} )

target_include_directories( ${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/backends>
    $<INSTALL_INTERFACE:/>
    $<INSTALL_INTERFACE:/backends>
)


if ( BUILD_IMNODES )

    include( FetchContent )

    FetchContent_Declare( imnodes
        GIT_REPOSITORY https://github.com/Nelarius/imnodes.git
        GIT_TAG master
        EXCLUDE_FROM_ALL
    )

    FetchContent_GetProperties( imnodes )

    if( NOT imnodes_POPULATED )
        FetchContent_Populate( imnodes )
    endif()

    file(
        COPY
        ${imnodes_SOURCE_DIR}/imnodes.h
        ${imnodes_SOURCE_DIR}/imnodes_internal.h
        ${imnodes_SOURCE_DIR}/imnodes.cpp
        DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
    )

    target_sources( ${PROJECT_NAME} PRIVATE
        imnodes.cpp
    )

endif()


target_link_libraries( ${PROJECT_NAME}
    PUBLIC
        glfw
)