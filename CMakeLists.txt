# setup the minimum allowed CMake version
cmake_minimum_required(VERSION 2.8)

# set the default build type (this needs to be done *before* project command)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "RelWithDebInfo" "MinSizeRel")
endif()

# restrict configuration types to the selected build type
set(CMAKE_CONFIGURATION_TYPES ${CMAKE_BUILD_TYPE} CACHE INTERNAL "" FORCE)

option(CONSOLE_APP "creates exe with console subsystem" ON)

# set the project name
project("luaexe")

# set the top-level project directory relative to CMakeLists.txt location
set(TOPDIR .)


if(WIN32)
  # add the executable file to our project
  if (CONSOLE_APP)
    add_definitions(-D_CONSOLE)
    add_executable(luaexe ${TOPDIR}/win32_starter.cpp ${TOPDIR}/luaexe.rc)
  else()
    add_executable(luaexe WIN32 ${TOPDIR}/win32_starter.cpp ${TOPDIR}/luaexe.rc)
  endif()
  

  # link to the static multi-threaded CRT under MSVC
  if(MSVC)
    string(TOUPPER ${CMAKE_BUILD_TYPE} BUILD_TYPE)
    foreach(FLAGS_VAR CMAKE_C_FLAGS CMAKE_CXX_FLAGS CMAKE_C_FLAGS_${BUILD_TYPE} CMAKE_CXX_FLAGS_${BUILD_TYPE})
      string(REGEX REPLACE "/MD" "/MT" ${FLAGS_VAR} ${${FLAGS_VAR}})
      string(REGEX REPLACE "/MDd" "/MTd" ${FLAGS_VAR} ${${FLAGS_VAR}})
    endforeach()
  endif()

endif()
