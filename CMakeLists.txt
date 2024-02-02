cmake_minimum_required( VERSION 3.28 FATAL_ERROR )

project(
    imgui
    DESCRIPTION "Dear ImGui using GLFW and OpenGL3"
    HOMEPAGE_URL "https://github.com/gurki/"
    LANGUAGES CXX
)

add_library( ${PROJECT_NAME} STATIC
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
    imnodes.cpp
)

add_library( ${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME} )

target_include_directories( ${PROJECT_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/backends>
        $<INSTALL_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>
        $<INSTALL_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/backends>
)

target_link_libraries( ${PROJECT_NAME}
    PUBLIC
        glfw
)