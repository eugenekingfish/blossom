cmake_minimum_required(VERSION 3.10)
project(OpenGLExamples CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SOURCES src/window.cpp src/shader.cpp)

add_executable(multi_cube ${SOURCES} examples/multi_cube.cpp)
add_executable(triangles ${SOURCES} examples/triangles.cpp)
add_executable(waves ${SOURCES} examples/waves.cpp)

add_executable(shader_test tests/shader_test.cpp src/shader.cpp)
find_package(GTest REQUIRED)
target_link_libraries(shader_test PRIVATE GTest::gtest_main GLEW OpenGL::GL OpenGL::GLU glfw GLEW::GLEW pthread)

add_custom_target(copy_shaders ALL
   COMMAND ${CMAKE_COMMAND} -E copy_directory
   ${CMAKE_SOURCE_DIR}/shaders ${CMAKE_BINARY_DIR}/shaders
   COMMENT "Copying shaders to build directory"
)

add_dependencies(multi_cube copy_shaders)
add_dependencies(triangles copy_shaders)
add_dependencies(waves copy_shaders)

cmake_policy(SET CMP0072 NEW)
set(OpenGL_GL_PREFERENCE GLVND)
find_package(OpenGL REQUIRED)

find_package(GLEW REQUIRED)
find_package(glfw3 REQUIRED)

target_include_directories(multi_cube PRIVATE include)
target_include_directories(triangles PRIVATE include)
target_include_directories(waves PRIVATE include)

target_link_libraries(multi_cube PRIVATE OpenGL::GL glfw GLEW::GLEW)
target_link_libraries(triangles PRIVATE OpenGL::GL glfw GLEW::GLEW)
target_link_libraries(waves PRIVATE OpenGL::GL glfw GLEW::GLEW)

enable_testing()
add_test(NAME ShaderTest COMMAND shader_test)
