cmake_minimum_required(VERSION 2.8)

project(qt5json CXX)

string(TOUPPER ${PROJECT_NAME} PROJECT_NAME_UPPER)

find_package(Qt4 COMPONENTS QtCore REQUIRED)
include(${QT_USE_FILE})

set(sources
  qjson.cpp
  qjsonarray.cpp
  qjsondocument.cpp
  qjsonobject.cpp
  qjsonparser.cpp
  qjsonvalue.cpp
  qjsonwriter.cpp
)

include(GenerateExportHeader)
add_compiler_export_flags()

add_library(${PROJECT_NAME} ${sources})

GENERATE_EXPORT_HEADER(${PROJECT_NAME}
  EXPORT_MACRO_NAME QJSON_EXPORT  #stay compatible to json from qtjsonrcp
  EXPORT_FILE_NAME ${CMAKE_CURRENT_BINARY_DIR}/qjson_export.h  #stay compatible to json from qtjsonrcp
)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

set(CMAKE_AUTOMOC TRUE)

set(BIN_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/bin" CACHE PATH "The directory where binaries will install")
set(LIB_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/lib" CACHE PATH "The directory where lib will install")

install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}-targets
  RUNTIME DESTINATION ${BIN_INSTALL_DIR}
  LIBRARY DESTINATION ${LIB_INSTALL_DIR}
  ARCHIVE DESTINATION ${LIB_INSTALL_DIR})

target_link_libraries(${PROJECT_NAME} ${QT_LIBRARIES})

set(${PROJECT_NAME_UPPER}_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR} CACHE INTERNAL "${PROJECT_NAME} include directories")
set(${PROJECT_NAME_UPPER}_LIBRARIES ${PROJECT_NAME} CACHE INTERNAL "${PROJECT_NAME} libraries")
