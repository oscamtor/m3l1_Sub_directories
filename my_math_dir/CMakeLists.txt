cmake_minimum_required(VERSION 3.0.0)

project(Calculator_Project VERSION 1.0.0)

#  sets the Release With Debug Information mode (change the default build mode and see it reflected in the GUI)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING
      "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
endif()

# Note src/ prefix to allow find the .cpp in their new src subfolder!
add_library(my_math
src/addition.cpp
src/division.cpp)

#[[Mandatory after added the target. Notar que no se usa en el CMakeLists.txt top y sin embargo main.cpp podra hacer #include "addition.h"  sin tener que proveer ninguan relative path. ¡¡CMake se encarga!! y lo puede hacer gracias a este scope PUBLIC]]
target_include_directories(my_math PUBLIC include)


