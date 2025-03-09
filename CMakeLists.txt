
cmake_minimum_required(VERSION 3.31)

# Define an ImGui target that fits our use case
add_library(imgui STATIC
    # Among the different backends available, we are interested in connecting
    # the GUI to GLFW andWebGPU:
    backends/imgui_impl_wgpu.h
    backends/imgui_impl_wgpu.cpp
    backends/imgui_impl_glfw.h
    backends/imgui_impl_glfw.cpp

    # Bonus to add some C++ specific features (the core ImGUi is a C library)
    misc/cpp/imgui_stdlib.h
    misc/cpp/imgui_stdlib.cpp

    # The core ImGui files
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
)

target_include_directories(imgui PUBLIC .)
# target_link_libraries(imgui PUBLIC webgpu glfw)


if(EMSCRIPTEN)

else()
  set(DAWN_FETCH_DEPENDENCIES ON)
  set(DAWN_BUILD_SAMPLES OFF)
  set(DAWN_BUILD_NODE_BINDINGS OFF)
  set(TINT_BUILD_DOCS OFF)
  set(TINT_BUILD_TESTS OFF)

  add_subdirectory("third_party/dawn" EXCLUDE_FROM_ALL)

  target_link_libraries(imgui PRIVATE
      dawn::webgpu_dawn
      glfw
      webgpu_glfw
  )
  
endif()

target_compile_definitions(imgui PRIVATE
  IMGUI_IMPL_WEBGPU_BACKEND_DAWN
)