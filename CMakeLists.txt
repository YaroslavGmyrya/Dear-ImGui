cmake_minimum_required(VERSION 3.28.0)
project(main VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 20)

find_package(glfw3 3.3 REQUIRED)

set(IMGUI
    dependencies/imgui/imgui.cpp
    dependencies/imgui/imgui_draw.cpp
    dependencies/imgui/imgui_tables.cpp
    dependencies/imgui/imgui_widgets.cpp
    dependencies/imgui/imgui_demo.cpp
)

if(APPLE)

add_executable(main
    src/macOS.mm
    dependencies/backend/glfw_adapter.mm
    ${IMGUI}
    dependencies/imgui/backends/imgui_impl_glfw.cpp
    dependencies/imgui/backends/imgui_impl_metal.mm
)

target_include_directories(main
    PRIVATE
    dependencies
)

target_link_libraries(main PRIVATE
    glfw
    "-framework Metal"
    "-framework Foundation"
    "-framework QuartzCore"
    "-framework AppKit"
    objc
)

elseif(UNIX)
    add_executable(main
        src/linux.cpp
        ${IMGUI}
        dependencies/imgui/backends/imgui_impl_sdl2.cpp
        dependencies/imgui/backends/imgui_impl_opengl3.cpp
    )

    find_package(SDL2 REQUIRED)
    find_package(OpenGL REQUIRED)

    target_link_libraries(main PRIVATE
        SDL2::SDL2
        OpenGL::GL
    )

    target_include_directories(main PUBLIC
        ${DEPENDENCIES_DIR}/imgui
        ${DEPENDENCIES_DIR}/imgui/backends
    )

    target_link_libraries(main PUBLIC
        SDL2::SDL2
        OpenGL::GL
    )
endif()

