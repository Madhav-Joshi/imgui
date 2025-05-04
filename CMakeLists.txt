cmake_minimum_required(VERSION 3.10)

project(ImGui LANGUAGES CXX)

# Add the static library target
add_library(ImGui STATIC
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    imgui_demo.cpp
)

# Set the C++ standard
target_compile_features(ImGui PRIVATE cxx_std_17)

# Platform-specific settings
if(WIN32)
    target_compile_definitions(ImGui PRIVATE _CRT_SECURE_NO_WARNINGS)
    set_target_properties(ImGui PROPERTIES
        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
    )
elseif(UNIX)
    target_compile_options(ImGui PRIVATE -fPIC)
endif()

# Configuration-specific settings
target_compile_definitions(ImGui PRIVATE
    $<$<CONFIG:Debug>:IMGUI_DEBUG>
    $<$<CONFIG:Release>:IMGUI_RELEASE>
)

target_compile_options(ImGui PRIVATE
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O3>
)

# Output directories
set_target_properties(ImGui PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/ImGui"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/ImGui"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/ImGui"
)