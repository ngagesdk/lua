# Do not modify this file unless absolutely necessary.
# All project-specific settings can be found in the file:
# project_config.cmake

cmake_minimum_required(VERSION 3.10)

if(DEFINED ENV{NGAGESDK})
  set(NGAGESDK $ENV{NGAGESDK})
  set(CMAKE_TOOLCHAIN_FILE ${NGAGESDK}/cmake/ngage-toolchain.cmake)
else()
  message(FATAL_ERROR "The environment variable NGAGESDK needs to be defined.")
endif()

include(FetchContent)

if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.24)
  cmake_policy(SET CMP0135 NEW)
endif()

set(LUA_VERSION "5.4.4")

set(SRC_DIR "${CMAKE_CURRENT_BINARY_DIR}/lua5-src")
set(INC_DIR "${CMAKE_CURRENT_BINARY_DIR}/lua5-src")

FetchContent_Populate(
  lua5
  URL https://github.com/lua/lua/archive/refs/tags/v${LUA_VERSION}.zip
  URL_HASH
  SHA256=9f589ca98ea102616e1c29073882b6e6e285ed3f977a0d08c34b76451a72beba)

file(REMOVE "${SRC_DIR}/luaconf.h")

project(lua C CXX)

set(GCC_MODULE_COMN_DEFS -D__SYMBIAN32__ -D__GCC32__ -D__EPOC32__ -D__MARM__ -D__MARM_ARMI__)
set(GCC_MODULE_MODE_DEFS -DNDEBUG -D_UNICODE)
set(GCC_MODULE_DEFS      ${GCC_MODULE_COMN_DEFS} ${GCC_MODULE_MODE_DEFS})

set(lua_sources
  "${SRC_DIR}/lapi.c"
  "${SRC_DIR}/lauxlib.c"
  "${SRC_DIR}/lbaselib.c"
  "${SRC_DIR}/lcode.c"
  "${SRC_DIR}/lcorolib.c"
  "${SRC_DIR}/lctype.c"
  "${SRC_DIR}/ldblib.c"
  "${SRC_DIR}/ldebug.c"
  "${SRC_DIR}/ldo.c"
  "${SRC_DIR}/ldump.c"
  "${SRC_DIR}/lfunc.c"
  "${SRC_DIR}/lgc.c"
  "${SRC_DIR}/linit.c"
  "${SRC_DIR}/liolib.c"
  "${SRC_DIR}/llex.c"
  "${SRC_DIR}/lmathlib.c"
  "${SRC_DIR}/lmem.c"
  "${SRC_DIR}/loadlib.c"
  "${SRC_DIR}/lobject.c"
  "${SRC_DIR}/lopcodes.c"
  "${SRC_DIR}/loslib.c"
  "${SRC_DIR}/lparser.c"
  "${SRC_DIR}/lstate.c"
  "${SRC_DIR}/lstring.c"
  "${SRC_DIR}/lstrlib.c"
  "${SRC_DIR}/ltable.c"
  "${SRC_DIR}/ltablib.c"
  "${SRC_DIR}/ltests.c"
  "${SRC_DIR}/ltm.c"
  "${SRC_DIR}/lua.c"
  "${SRC_DIR}/lundump.c"
  "${SRC_DIR}/lutf8lib.c"
  "${SRC_DIR}/lvm.c"
  "${SRC_DIR}/lzio.c"
  "${SRC_DIR}/onelua.c")

add_library(lua STATIC ${lua_sources})

target_compile_definitions(
  lua
  PUBLIC
  ${GCC_MODULE_DEFS})

target_compile_options(
  lua
  PUBLIC
  -O3)

target_include_directories(
  lua
  PUBLIC
  ${INC_DIR})
