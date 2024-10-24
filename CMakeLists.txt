cmake_minimum_required(VERSION 3.28)
project(OpenGLTutorial)

find_package(OpenGL REQUIRED)

if( CMAKE_BINARY_DIR STREQUAL CMAKE_SOURCE_DIR )
    message( FATAL_ERROR "Please select another Build Directory ! (and give it a clever name, like bin_Visual2012_64bits/)" )
endif()
if( CMAKE_SOURCE_DIR MATCHES " " )
    message( "Your Source Directory contains spaces. If you experience problems when compiling, this can be the cause." )
endif()
if( CMAKE_BINARY_DIR MATCHES " " )
    message( "Your Build Directory contains spaces. If you experience problems when compiling, this can be the cause." )
endif()

# Compile external dependencies
add_subdirectory (external)

include_directories(
        external/glfw-3.4/include/
        external/glm-1.0.1/
        external/glew-2.1.0/include/
        .
)

set(ALL_LIBS
        ${OPENGL_LIBRARY}
        glfw
        GLEW_210
)

add_definitions(
        -DTW_STATIC
        -DTW_NO_LIB_PRAGMA
        -DTW_NO_DIRECT3D
        -DGLEW_STATIC
        -D_CRT_SECURE_NO_WARNINGS
)

file(GLOB SHADERS
        "*.glsl"
)

add_executable(OpenGLTutorial
        main.cpp
        common/shader.cpp
        common/shader.hpp

        ${SHADERS}
        common/texture.cpp
        common/texture.hpp
)

target_link_libraries(OpenGLTutorial
        ${ALL_LIBS}
)

# SOURCE_GROUP(common REGULAR_EXPRESSION ".*/common/.*" )
# SOURCE_GROUP(shaders REGULAR_EXPRESSION ".*/.*shader$" )

add_custom_target(copy_shaders ALL
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${PROJECT_SOURCE_DIR}/shaders
        ${PROJECT_BINARY_DIR}
        COMMENT "Copying shaders into binary directory"
)

add_dependencies(OpenGLTutorial copy_shaders)