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

#[[Mandatory after added the target. Notar que no se usa en el CMakeLists.txt top y sin embargo main.cpp podra hacer #include "addition.h"  sin tener que proveer ninguan relative path. ¡¡CMake se encarga!! y lo puede hacer gracias a este scope PUBLIC]]. Agrega la propiedad INTERFACE_INCLUDE_DIRECTORIE y al hacerlo como public los CMakeList.txt padres (aqui el general) reciben tb la propiedad y por eso en los fuentes del ppal puedo incluir el h exactamente igual que en los fuents de las librerias (basta poner el .h sin subdirectorio include por esto ...
target_include_directories(my_math PUBLIC include)
# ... bueno y sin subdir con el de la libreria ("my_math_dir" o "my_print_dir") por la delegacion ya vista en el raiz de poner sendos add_subdirectory cada uno con el nombre del subdir en el que poner un nuevo CMakeLists.txt que se encargara de generar el .a de cada una de las libre rias

Q1: Does child project (the library here  my_path o my_print) need the include dir? 
Q2: Does parent project (here calculator) dependig for chidl project (library my_math) need the include dir?
Answer1	Answer 2 | 
------  -------- |--------
Yes		Yes			PUBLIC
No		Yes			INTERFACE_INCLUDE_DIRECTORIE
YES		No			Private

Mis preguntas equivalentes:
Q1: los cpp de este nivel necesitan .h de este nivel (libreria)?
Q2: los cpp del nivel padre  (ejecutable) necesitan hacer hacer #include de algun .h de este nivel (libreria)?


Mi Gpreguntas equivalentes para este nuestro ejemplo
Q1: addition.cpp necesita hacer #include addition.h? Yes
Q2: main.cpp necesita hacer #include addition.h? Yes

