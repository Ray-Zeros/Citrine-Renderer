cmake_minimum_required(VERSION 3.10)
project(Citrine_Renderer VERSION 0.1)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Get CMakeLists.txt
add_subdirectory(3rd/glfw)
add_subdirectory(3rd/glm)
# add_subdirectory(3rd/imgui)
# add_subdirectory(3rd/stb)

find_package(Vulkan REQUIRED)

add_executable(Citrine_Renderer
    Citrine/src/main.cpp
    Citrine/src/citrine.cpp
)

target_include_directories(Citrine_Renderer
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/Citrine/include
    ${CMAKE_CURRENT_SOURCE_DIR}/3rd/glm
    ${CMAKE_CURRENT_SOURCE_DIR}/3rd/stb
    ${CMAKE_CURRENT_SOURCE_DIR}/3rd/tinyobjloader
)

target_link_libraries(Citrine_Renderer
    PRIVATE
    glfw
    Vulkan::Vulkan
)

add_custom_command(TARGET Citrine_Renderer POST_BUILD
COMMAND ${CMAKE_COMMAND} -E copy_directory
${CMAKE_CURRENT_SOURCE_DIR}/citrine/shaders
$<TARGET_FILE_DIR:Citrine_Renderer>/shaders
COMMAND ${CMAKE_COMMAND} -E copy_directory
${CMAKE_CURRENT_SOURCE_DIR}/citrine/textures
$<TARGET_FILE_DIR:Citrine_Renderer>/textures
COMMAND ${CMAKE_COMMAND} -E copy_directory
${CMAKE_CURRENT_SOURCE_DIR}/citrine/models
$<TARGET_FILE_DIR:Citrine_Renderer>/models
)