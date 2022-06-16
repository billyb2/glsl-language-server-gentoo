cmake_minimum_required(VERSION 3.0.0)
LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

project(glsl-language-server)


find_package(GLSLang COMPONENTS GLSLang REQUIRED)
find_package(Threads REQUIRED)
find_package(fmt REQUIRED)
find_package(nlohmann_json REQUIRED)
find_package(CLI11 REQUIRED)

add_library(mongoose externals/mongoose/mongoose.c)
include_directories(
    externals/mongoose
)

set(CMAKE_CXX_STANDARD 17)

file(GLOB SOURCES
    src/*.cpp
)

include_directories(src)
add_executable(glslls
    ${SOURCES}
)

if (CMAKE_SYSTEM_NAME MATCHES Darwin)
    set(stdfs)
else()
    set(stdfs stdc++fs)
endif()

target_link_libraries(glslls
    ${CMAKE_THREAD_LIBS_INIT}
	GLSLang::GLSLang	
    nlohmann_json
    mongoose
    ${stdfs}
    SPIRV
    fmt::fmt-header-only
)

install(TARGETS glslls
        RUNTIME DESTINATION bin)
