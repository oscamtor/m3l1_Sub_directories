# m3l1_Sub_directories-
m3l1_Sub_directories- description

*********************************************************************************************************
Ejemplo de aplicacion de consola compilable con cmake que usa dos librerias (my_math y my_print) 


Estructura inicial
developer@LUBUNTU-DEV /mnt/SHARED/CMake_Curse/m3l1_Sub_directories- (main)$ ls -Rlf
(solo muestro fuentes)


.:
 .    .git                                                                            build            CMakeLists.txt.orig   debug    main.cpp      my_print_dir   release
 ..  '2022-02-25 11_33_44-Master CMake for Cross-Platform C++ Project Building.png'   CMakeLists.txt   CMakeLists.txt.user   my_math_dir   README.md  



./my_math_dir:
.  ..  CMakeLists.txt  CMakeLists.txt.user  include  src

./my_math_dir/include:
.  ..  addition.h  division.h

./my_math_dir/src:
.  ..  addition.cpp  division.cpp

./my_print_dir:
.  ..  CMakeLists.txt  include  src 

./my_print_dir/include:
.  ..  print_result.h

./my_print_dir/src:
.  ..  print_result.cpp


changelog
***************
20220225 Commit inicial. Sobre el original (m3l11) homonimo del curso lleva como mejoras: m3l12 (managing header files: .h en sus respectivas librerias en lugar de en el raiz) ,  m3l12 (cmake way of including header files: .h en subidr include y .cpp en subdir src gracias a crear dichos directorios include y src, mover a ellos los .h y .cpp respectivamente y en el CMakeLists.txt de cada libraria en add_library anteponer a cada cpp el prefijo con el directorio relativo desde dicho CMakeLists.txt de la libreria a. Los .h no se referencian. Si los .cpp)


Depurado ok en qtcreator tras:
*************************************************************************************************************
1. Añadir esto en cada CMakeLists.txt
***
#  sets the Release With Debug Information mode (change the default build mode and see it reflected in the GUI)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING
      "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
endif()

2.
***
Qtcreator/Projects/Build & Run/Imported Kit/Run en  command line arguments poner -DCMAKE_BUILD_TYPE=Debug

3.
**
Esto es el mismo flag que pase a cmake desde el subdirectorio debug:
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
./calculator


para resolver el problema inicial de "This does not seem to be a "Debug" build" al darle al play con bicho cuando en lugar de construir en ambos release y debug (ejecutando desde ellos respectivamente cmake -DCMAKE_BUILD_TYPE=Release .. y cmake -DCMAKE_BUILD_TYPE=Debug ..) contruía solo en build e invocando a cmake sin flag (cmake ..)

por ultimo tuve ademas que en
Qtcreator/Projects/Build & Run/Imported Kit/Build en Build Settings/Edit build configuration que inicialmente solo tenía build darle a add y seleccionar Debug y en Build directori browsear a /mnt/SHARED/CMake_Curse/m3l1_Sub_directories-/debug con lo que me 


