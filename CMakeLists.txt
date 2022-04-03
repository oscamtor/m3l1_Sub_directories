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
add_subdirectory(my_math)
add_subdirectory(my_print)

add_executable(calculator main.cpp)

# OCT de internet no soluciona "undefined reference" to `addition(float, float)'
#include_directories(my_math_dir/)
#link_directories(my_print_dir/)

# Agregamos las dependencias (librerias "my_math" y "my_math") de las que va a depender nuestro target ("calculator") con un comando target_link_libraries con todas las dependencisa (librerias) de las que va a necesitar para llegar a construirse . 
# Con esto cmake sabe que tiene que leer las prperties de ambas dependencias (my:math y my_print)
# Properties of my_math:
# Property A = aaa
# Property B = bbb
# ...
# Properties of my _print:
# Property A = xxx
# Property B = yyy
# ...
# Un ejemplo real de propiedad de dependencia (en lugar de A) sería INTERFACE_INCLUDE_DIRECTORY que al ser de dependencia explicaré donde se establece es decir en el CMakeList.txt de p.e. la libreria my_math. Si ambas la tienen a PUBLIC significara para cmake que los directorios incluidos por ambas libraries serán visibles tambien al "calculator" target es decir podré hacer #include "addition.h" (sin necesidad de especificar el path relativo de los .h en el .cpp (main.cpp) de este nivel (el del ejecutable). Esto es ha si porque se ha puesto PUBLIC al target_include_directories de la libreria (my_math o my_print)

target_link_libraries(calculator PUBLIC my_math my_print)







