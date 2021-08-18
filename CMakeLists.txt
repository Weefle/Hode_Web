project(hode)

cmake_minimum_required(VERSION 3.5.1)

option(JS_ONLY "Compiles to native JS (No WASM)" OFF)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(CMAKE_CXX_FLAGS "-g -Wall -pedantic -MMD ${CMAKE_CXX_FLAGS}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/public")

include_directories(include)

file(GLOB SRC src/*.cpp 3p/inih/ini.c 3p/libxbr-standalone/xbr.c)
list(FILTER SRC EXCLUDE REGEX ".*android.cpp|system_psp.cpp|system_wii.cpp")
add_executable(hode ${SRC})

message(STATUS "Setting compilation target to WASM")
set(CMAKE_EXECUTABLE_SUFFIX ".html")
set_target_properties(hode PROPERTIES LINK_FLAGS "-O3 -s WASM=1 -s TOTAL_MEMORY=78643200 -s FORCE_FILESYSTEM=1 -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s USE_SDL_GFX=2 -s ALLOW_MEMORY_GROWTH=1 -s FULL_ES3=1 -s ASYNCIFY -s GL_PREINITIALIZED_CONTEXT=1 --preload-file ../res")
