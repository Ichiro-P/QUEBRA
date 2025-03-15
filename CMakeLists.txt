cmake_minimum_required(VERSION 3.20)
project(QUEBRA LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(OpenGL REQUIRED)

find_package(glfw3 3.3 REQUIRED)

add_library(glad STATIC src/glad/glad.c)
set_source_files_properties(src/glad/glad.c PROPERTIES LANGUAGE C)
target_include_directories(glad PUBLIC include)

add_executable(QUEBRA
    src/main.cpp
)

target_link_libraries(QUEBRA
    OpenGL::GL
    glfw
    glad
)   

#file(COPY resources/shaders DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/resources)


if(UNIX AND NOT APPLE AND NOT WIN32)
    target_link_libraries(QUEBRA
        X11
        Xrandr
        Xinerama
        Xi
        Xxf86vm
        Xcursor
        pthread
        dl
    )
elseif(APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(IOKIT_LIBRARY IOKit)
    target_link_libraries(QUEBRA
        ${COCOA_LIBRARY}
        ${IOKIT_LIBRARY}
    )
elseif(WIN32)
    target_link_libraries(QUEBRA
        opengl32
    )
endif()

#file(COPY resources DESTINATION ${CMAKE_CURRENT_BINARY_DIR})