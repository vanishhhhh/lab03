cmake_minimum_required(VERSION 3.4)

project(solver_application)

include_directories(../formatter_lib ../formatter_ex_lib ../solver_lib)

link_directories(../formatter_lib/_build ../formatter_ex_lib/_build ../solver_lib/_build)

add_executable(solver equation.cpp)

target_link_libraries(solver formatter_ex formatter solver_lib)
