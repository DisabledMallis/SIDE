cmake_minimum_required(VERSION 3.17)
project(side VERSION 1.0 LANGUAGES CXX)
include(ExternalProject)
include(FetchContent)
add_executable(side)

target_sources(side PRIVATE
    "main.cpp"
)
add_subdirectory(imgui)

option(USE_LEGACY_OPENGL "Use Legacy OpenGL (fixed function pipeline) or Modern OpenGL (with shaders)" OFF)
if(USE_LEGACY_OPENGL)
    add_definitions(-DUSE_LEGACY_OPENGL)
endif()
message("USE_LEGACY_OPENGL: ${USE_LEGACY_OPENGL}")

set(LFLAGS "-s USE_GLFW=3")
set(LFLAGS "${LFLAGS} -s FULL_ES3=1")
set(LFLAGS "${LFLAGS} -s EXPORTED_FUNCTIONS=_main,_native_resize")
set(LFLAGS "${LFLAGS} -s EXPORTED_RUNTIME_METHODS=cwrap")

set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "${LFLAGS}")
target_link_libraries(${PROJECT_NAME} glfw)