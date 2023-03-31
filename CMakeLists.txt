cmake_minimum_required(VERSION 3.10)

project(return)

add_library(return_zero STATIC return_zero.cpp)

add_library(return_one STATIC return_one.cpp)
    
add_custom_command(
  OUTPUT libreturn.a
  COMMAND ar -M < ${PROJECT_SOURCE_DIR}/return.mri
  DEPENDS return_zero return_one
  )

add_custom_target(
  return-static
  DEPENDS libreturn.a
  )

add_library(return-shared SHARED dummy.cpp)
target_link_libraries(return-shared PRIVATE "$<LINK_LIBRARY:WHOLE_ARCHIVE,${PROJECT_BINARY_DIR}/libreturn.a>")
add_dependencies(return-shared return-static)
