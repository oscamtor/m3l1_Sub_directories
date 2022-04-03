cmake_minimum_required(VERSION 3.0.0)

project(Calculator_Project VERSION 1.0.0)

#  sets the Release With Debug Information mode (change the default build mode and see it reflected in the GUI)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING
      "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
endif()

add_library(my_print
src/print_result.cpp)



target_include_directories(my_print PUBLIC include)
