cmake_minimum_required(VERSION 3.10)
project(
  src
  VERSION 2.0
  DESCRIPTION "Threading assignment CMPT201")
file(GLOB SOURCES "src/*.c")
include_directories(include)
add_executable(candykids ${SOURCES})
target_link_libraries(candykids PRIVATE rt)
