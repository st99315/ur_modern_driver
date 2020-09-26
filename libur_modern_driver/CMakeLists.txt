cmake_minimum_required(VERSION 2.8.12)
project(ur_modern_driver)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  message("${PROJECT_NAME}: You did not request a specific build type: selecting 'RelWithDebInfo'.")
  set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif()

# check c++11 / c++0x
include(CheckCXXCompilerFlag)
check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
check_cxx_compiler_flag("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
  add_compile_options(-std=c++11)
elseif(COMPILER_SUPPORTS_CXX0X)
  add_compile_options(-std=c++0x)
else()
  message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler. Suggested solution: update the pkg build-essential ")
endif()

add_compile_options(-Wall)
add_compile_options(-Wextra)
add_compile_options(-Wno-unused-parameter)

include_directories(
  ../include
)

set(${PROJECT_NAME}_LIB_SOURCES
    ../src/tcp_socket.cpp
    ../src/ur/commander.cpp
    ../src/ur/master_board.cpp
    ../src/ur/messages.cpp
    ../src/ur/robot_mode.cpp
    ../src/ur/rt_state.cpp
    ../src/ur/server.cpp
    ../src/ur/stream.cpp)

add_library(ur_driver_lib SHARED
  ${${PROJECT_NAME}_LIB_SOURCES}
)

install(TARGETS ur_driver_lib
  DESTINATION lib
)

install(DIRECTORY ../include/${PROJECT_NAME}
  DESTINATION include
  FILES_MATCHING PATTERN "*.h"
)
