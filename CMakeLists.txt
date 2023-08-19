cmake_minimum_required(VERSION 3.7)
project("imgui_vulkan" VERSION 1.0.0)

add_library(imgui STATIC
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_draw.cpp
    imgui_demo.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    backends/imgui_impl_vulkan.h
    backends/imgui_impl_vulkan.cpp
    backends/imgui_impl_glfw.h
    backends/imgui_impl_glfw.cpp
)

target_include_directories(imgui PUBLIC "/")
target_include_directories(imgui PUBLIC "/backends")

target_link_libraries(imgui PRIVATE glfw)

find_package(Vulkan REQUIRED)
IF (Vulkan_FOUND)
    target_link_libraries(imgui PRIVATE ${Vulkan_LIBRARIES})
    target_include_directories(imgui PUBLIC ${Vulkan_INCLUDE_DIR})
ELSE()
    message(ERROR "Vulkan SDK has to be installed")
ENDIF()

