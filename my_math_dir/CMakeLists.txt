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


# Q1: Does child project (the library here  "my_math" o "my_print") need the include dir? 
# Q2: Does parent project (here calculator) dependig for chidl project (library my_math) need the include dir?
# Answer1	Answer 2 |  KEYWORD (de la propiedad (aqui INTERFACE_INCLUDE_DIRECTORY) asociada al target (aqui my_math)

# A1      A2         OUTPUT
# ------  -------- |--------
# Yes		Yes			PUBLIC       => main.cpp SI podra encontrar el "include" directory (y por tanto buildear ok haciendo #include addition.h y addition.cpp SI podra encontrarlo (buildear ok) tb haciendo #include addition.h    
  
# No		Yes			INTERFACE    => main.cpp SI podra encontrarlo (buildear ok) haciendo #include addition.h. addition.cpp no necesita hacer #include addition.h y buildeara ok.
	# al compilar addition.cpp dara este error de que addition.cpp no puede encontrar el addition.h que trata de incluir:
	# ...adition.cpp ... fatal error: addition.h: No such file or directory
	# # include "addition.h"
	            #^----------
	# diciendonos que no puede encontrar  addition.h
	
# Yes		No			PRIVATE      => main.cpp NO podra encontarlo  haciendo #include addition.h pero addition.cpp  SI podra encontrarlo (y por tanto buildear ok) haciendo #include addition.h 
                   # al especificar un value de KEYWORD (PUBLIC, INTERFACE o PRIVATE) a un comando cmake (target_include_directories) una property (aqui INTERFACE_INCLUDE_DIRECTORIES) para su target (aqui my_math) es automáticamente establecida.
	# al compilar main.cpp dara este error diciendonos que no puede encontrar  addition.h
	# ...main.cpp ... fatal error: addition.h: No such file or directory
	# # include "addition.h"	
	            #^----------			   
				   
# donde: 	
	# A1: es la answer a la question Q1 y la respondes mirando si el fuente ("main.cpp") del nivel padre (el  del ejecutable "calculator" necesita hacer "#include .h" de algun .h de esta dependencia (library "my_math")	


	# A2: es la anwser a la question Q2 y la respondes mirando si el fuente ("addition.cpp") de la dependencia  (library "my_math")  necesita hacer "#include .h"  de algun .h de esta dependencia (library "my_math")	

	

	# OUTPUT: es el value (KEYWORD) de la property 

# Mis preguntas equivalentes:
# Q1: los cpp de este nivel necesitan .h de este nivel (libreria)?
# Q2: los cpp del nivel padre  (ejecutable) necesitan hacer hacer #include de algun .h de este nivel (libreria)?


# Mi Gpreguntas equivalentes para este nuestro ejemplo
# Q1: addition.cpp necesita hacer #include addition.h? Yes
# Q2: main.cpp necesita hacer #include addition.h? Yes

# Si hubieramos cambiado la KEYWORD de PUBLIC a:
# target_include_directories(my_math INTERFACE include) 
                                  ********



# Otros comandos comunes que requieren scope: PUBLIC, PRIVATE or INTERFACE  y sus properties que establecen cuando el comando se ejecuta
# Command							Propertie that set
# --------------------------		-----------------------------------

# target_compile_definitions		INTERFACE_COMPILE_DEFINITIONS
# target_sources 					INTERFACE_SOURCES
# target_compile_features			INTERFACE_COMPILE_FEATURES
# target_compile_options			INTERFACE_COMPILE_OPTIONS
# target_link_directories			INTERFACE_LINK_DIRECTORIES
# target_link_libraries			INTERFACE_LINK_LIBRARIES
# target_link_options				INTERFACE_LINK_OPTIONS
# target_precompile_headers		INTERFACE_PRECOMPILE_HEADERS

Su formatoes el mismo
target_... 						   (TargetName SCOPE Arguments)

As seen the comman set the property to espcifiend target & using the "scope specifier" (PUBLIC, INTERFACE) allow us to propagate the requirements to another higher-level-targets

If our target has both PUBLIC & PRIVATE requierements, both can be clubbed together in one comman or reapeated calls can be made with different scopes:
target_include_directories(my_math PRIVATE xxx)
target_include_directories(my_math PUBLIC yyy)