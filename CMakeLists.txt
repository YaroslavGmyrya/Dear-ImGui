cmake_minimum_required(VERSION 3.28.0)
project(main VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 20)

set(IMGUI_DEP ${PROJECT_SOURCE_DIR}/dependencies/imgui)
set(IMPLOT_DEP ${PROJECT_SOURCE_DIR}/dependencies/implot)


set(IMGUI
    ${IMGUI_DEP}/imgui.cpp
    ${IMGUI_DEP}/imgui_draw.cpp
    ${IMGUI_DEP}/imgui_tables.cpp
    ${IMGUI_DEP}/imgui_widgets.cpp
    ${IMGUI_DEP}/imgui_demo.cpp
)

set(IMPLOT
    ${IMPLOT_DEP}/implot_demo.cpp
    ${IMPLOT_DEP}/implot_items.cpp
    ${IMPLOT_DEP}/implot.cpp
)

if(APPLE)

add_executable(main
    ${IMGUI}
    ${IMPLOT}
    dependencies/backend/glfw_adapter.mm
    dependencies/imgui/backends/imgui_impl_glfw.cpp
    dependencies/imgui/backends/imgui_impl_metal.mm

    src/macOS.mm
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
        ${IMGUI}
        ${IMPLOT}
        dependencies/imgui/backends/imgui_impl_sdl2.cpp
        dependencies/imgui/backends/imgui_impl_opengl3.cpp

        src/linux.cpp
    )

    find_package(SDL2 REQUIRED)
    find_package(OpenGL REQUIRED)

    target_include_directories(main PUBLIC
        dependencies/imgui
        dependencies/imgui/backends
    )

    target_link_libraries(main PUBLIC
        SDL2::SDL2
        OpenGL::GL
    )
endif()

