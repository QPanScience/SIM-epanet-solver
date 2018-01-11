# CMakeLists.txt - CMake configuration file for EPANET 2.0 
# 
# CMake is a cross-platform build tool. CMake generates platform native 
# makefiles that can be used with your compiler of choice. CMake uses a 
# generator concept to represent different build tooling. CMake automatically 
# detects the platform it is running on and generates the appropriate makefiles
# for the platform default compiler. Different generators can also be specified. 
# 
# Note: CMake requires that your platform build system and compiler are 
# properly installed. Build using Visual Studio requires msbuild shell.  
#  
# Example Usage: 
#   cd build/cmake
#   mkdir buildproducts
#   cd buildproducts
#   cmake ..
#   make 
#    
# Building MSYS on Windows:
#   ...
#   cmake -G "MSYS Makefiles" ..
#   make 
# 
# Building Visual Studio on Windows: 
#   ...
#   cmake -G "Visual Studio 10 2010" ..
#   msbuild /p:Configuration=Release ALL_BUILD.vcxproj
#
# More information:
#   cmake --help
# 
#   CMake is available at https://cmake.org/download/
# 

cmake_minimum_required (VERSION 2.6)

SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

project(EPANET)

IF(APPLE)
  SET(CMAKE_INSTALL_NAME_DIR @executable_path)
  SET(CMAKE_BUILD_WITH_INSTALL_RPATH ON)
ENDIF(APPLE)

# the library
include_directories(include)
file(GLOB EPANET_SOURCES src/*.c)
file(GLOB EPANET_HEADERS src/*.h)
file(GLOB EPANET_DATA src/*.dat)
set(EPANET_API_HEADER "include/epanet2.h")

add_library(epanet STATIC ${EPANET_SOURCES} ${EPANET_HEADERS} ${EPANET_DATA} ${EPANET_API_HEADER})

# the standalone executable
include_directories(src)
add_executable(runepanet run/main.c)
target_link_libraries (runepanet LINK_PUBLIC epanet m)