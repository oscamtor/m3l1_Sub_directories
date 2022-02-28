cmake_minimum_required(VERSION 3.0.0)
project(Calculator_Project VERSION 1.0.0)

#  sets the Release With Debug Information mode (change the default build mode and see it reflected in the GUI)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING
      "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
endif()

# Tendran que tener su propio CMakeLists.txt porque  este dejara de ver sus fuentes al estar en otro directorio!
#[[ Add subdirectories to the build tree. We are telling CMake to go inside these subdirs & then find another CMakeList.txt file in the subdirs & thereafter run those one by one]]
add_subdirectory(my_math_dir)
add_subdirectory(my_print_dir)

add_executable(calculator main.cpp)

# OCT de internet no soluciona "undefined reference" to `addition(float, float)'
#include_directories(my_math_dir/)
#link_directories(my_print_dir/)

target_link_libraries(calculator
PUBLIC my_math  my_print)







