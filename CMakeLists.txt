cmake_minimum_required( VERSION 3.28 FATAL_ERROR )

project(
    imgui
    DESCRIPTION "Dear ImGui using GLFW and OpenGL3"
    HOMEPAGE_URL "https://github.com/gurki/"
    LANGUAGES CXX
)

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

add_library( ${PROJECT_NAME} STATIC
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
    ${imnodes_SOURCE_DIR}/imnodes.cpp
)

add_library( ${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME} )

target_include_directories( ${PROJECT_NAME}
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/>
        ${CMAKE_CURRENT_SOURCE_DIR}/backends>
        ${imnodes_SOURCE_DIR}/
)

target_link_libraries( ${PROJECT_NAME}
    PUBLIC
        glfw
)