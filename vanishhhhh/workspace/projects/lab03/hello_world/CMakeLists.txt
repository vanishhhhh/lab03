cmake_minimum_required(VERSION 3.4)

project(hello_world)

include_directories(../formatter_lib)
include_directories(../formatter_ex_lib)

link_directories(../formatter_lib/_build)
link_directories(../formatter_ex_lib/_build)

add_executable(hello_world hello_world.cpp)

target_link_libraries(hello_world formatter_ex formatter)

