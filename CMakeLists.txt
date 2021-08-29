# only for cmake --version >= 3.5.1
cmake_minimum_required(VERSION 3.5.1)

# project name
project(binancecpp VERSION 0.5.0 DESCRIPTION "Binance APIs c++ dependencies")

# set (CMAKE_C_COMPILER             "/usr/bin/clang")
set (CMAKE_C_COMPILER             "/usr/bin/gcc")
set (CMAKE_C_FLAGS                "-Wall -std=c11")
set (CMAKE_C_FLAGS_DEBUG          "-g")
set (CMAKE_C_FLAGS_MINSIZEREL     "-Os -DNDEBUG")
set (CMAKE_C_FLAGS_RELEASE        "-O4 -DNDEBUG")
set (CMAKE_C_FLAGS_RELWITHDEBINFO "-O2 -g")

# set (CMAKE_CXX_COMPILER             "/usr/bin/clang++")
set (CMAKE_CXX_COMPILER             "/usr/bin/c++")
set (CMAKE_CXX_FLAGS                "-Wall -pthread")
# set (CMAKE_CXX_FLAGS_DEBUG          "-g -fsanitize=address")
set (CMAKE_CXX_FLAGS_DEBUG          "-g")
set (CMAKE_CXX_FLAGS_MINSIZEREL     "-Os -DNDEBUG")
set (CMAKE_CXX_FLAGS_RELEASE        "-O4 -DNDEBUG")
set (CMAKE_CXX_FLAGS_RELWITHDEBINFO "-O2 -g")

set (CMAKE_LINKER_FLAGS             "")
set (CMAKE_LINKER_FLAGS_DEBUG       "-fsanitize=address")

set (CMAKE_AR      "/usr/bin/llvm-ar")
set (CMAKE_LINKER  "/usr/bin/llvm-ld")
set (CMAKE_NM      "/usr/bin/llvm-nm")
set (CMAKE_OBJDUMP "/usr/bin/llvm-objdump")
set (CMAKE_RANLIB  "/usr/bin/llvm-ranlib")

list(APPEND
  CMAKE_MODULE_PATH
  "${CMAKE_SOURCE_DIR}/cmake-modules"
)

# Enable cURLpp
find_package(CURLpp REQUIRED)
include_directories(${CURLPP_INCLUDE_DIRS})

# Enable JSONcpp
find_package(jsoncpp REQUIRED)
include_directories(${JSONCPP_INCLUDE_DIRS})

# Enable libwebsocket
find_package(LibWebsockets REQUIRED)
include_directories(${LIBWEBSOCKETS_INCLUDE_DIRS})

# Enable OpenSSL
find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIRS})


# creates the variable EXEC and sets it to hello
set(EXEC binancecpp)

# set the C++14 standard
set( CMAKE_CXX_STANDARD 11 )


# puts all .cpp files inside src to the SOURCES_FILES variable
FILE( GLOB SOURCES_FILES src/*.cpp )
# FILE( GLOB TEST_FILES tests/*.cpp )

include(GNUInstallDirs)


# compiles the files defined by SOURCES_FILES to generante the executable defined by EXEC
# add_executable( ${EXEC} ${SOURCES_FILES} )
add_library(binancecpp SHARED ${SOURCES_FILES} )

set_target_properties(binancecpp PROPERTIES VERSION ${PROJECT_VERSION})

# I../includes
# target_include_directories(binancecpp PRIVATE include)
target_include_directories(binancecpp PRIVATE src)

install(TARGETS binancecpp
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# target_link_libraries( ${EXEC} ${CURLPP_LIBRARIES} ${jsoncpp_LIBRARY} ${LIBWEBSOCKETS_LIBRARIES} OpenSSL::SSL )

# --------------------------------
# Test Framework
# find_package(Catch2 REQUIRED)
# include(CTest)
# include(Catch)
#
# add_executable( tests
#     src/logger.h
#     src/logger.cpp
#     ${TEST_FILES}
# )
# target_link_libraries( tests PRIVATE Catch2::Catch2 ${CMAKE_THREAD_LIBS_INIT} ${CURLPP_LIBRARIES} ${jsoncpp_LIBRARY} ${GTK3_LIBRARY} ${GTKMM_LIBRARIES} OpenSSL::SSL SQLite::SQLite3 )
#
# include( Catch )
# enable_testing()
# catch_discover_tests( tests )
