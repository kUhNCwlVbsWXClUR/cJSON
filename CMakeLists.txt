cmake_minimum_required(VERSION 3.0)

project(cJSON)


#cJSON
set(CJSON_LIB cjson)

file(GLOB HEADERS ${PROJECT_SOURCE_DIR}/include/cJSON/cJSON.h)
set(SOURCES src/cJSON.c)

include_directories(${PROJECT_SOURCE_DIR}/include/)

option(BUILD_SHARED_AND_STATIC_LIBS "Build both shared and static libraries" Off)

if (NOT BUILD_SHARED_AND_STATIC_LIBS)
    add_library("${CJSON_LIB}" "${CJSON_LIBRARY_TYPE}" "${HEADERS}" "${SOURCES}")
else()
    # See https://cmake.org/Wiki/CMake_FAQ#How_do_I_make_my_shared_and_static_libraries_have_the_same_root_name.2C_but_different_suffixes.3F
    add_library("${CJSON_LIB}" SHARED "${HEADERS}" "${SOURCES}")
    add_library("${CJSON_LIB}-static" STATIC "${HEADERS}" "${SOURCES}")
    set_target_properties("${CJSON_LIB}-static" PROPERTIES OUTPUT_NAME "${CJSON_LIB}")
    set_target_properties("${CJSON_LIB}-static" PROPERTIES PREFIX "lib")
endif()


add_subdirectory(tests)
