cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)

include_directories(.)
include_directories(../formatter_lib)

add_library(formatter_ex STATIC formatter_ex.cpp)

target_link_libraries(formatter_ex PUBLIC formatter)
