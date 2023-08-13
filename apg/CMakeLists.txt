# https://github.com/ldthomas/apg-7.0
cmake_minimum_required(VERSION 3.16)
set(CMAKE_C_STANDARD 11)

project(APG70)

add_compile_definitions(APG_AST)
add_compile_definitions(APG_DEBUG APG_TRACE)

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/../api DIR_API)
add_library(api STATIC ${DIR_API})
add_library(apg::api ALIAS api)

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/../library DIR_LIBRARY)
add_library(library STATIC ${DIR_LIBRARY})
add_library(apg::library ALIAS library)
target_include_directories(library PUBLIC ../library)
target_compile_definitions(library PUBLIC APG_TRACE)
target_link_libraries(library PRIVATE utilities)

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/../utilities DIR_UTILITIES)
add_library(utilities STATIC ${DIR_UTILITIES})
add_library(apg::utilities ALIAS utilities)
target_include_directories(utilities PUBLIC ../utilities)

add_executable(apg70 ${CMAKE_CURRENT_SOURCE_DIR}/main.c ${CMAKE_CURRENT_SOURCE_DIR}/config.c)
target_link_libraries(apg70
  api
  library
  utilities
)
