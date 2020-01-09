cmake_minimum_required(VERSION 3.0)
project(mongoose C CXX)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

option(SHARED_LIB "Build shared lib instead of static one." ON)
if(SHARED_LIB MATCHES "ON")
  set(LIB_BUILD_TYPE SHARED)
else()
  set(LIB_BUILD_TYPE STATIC)
endif()

add_library(mongoose ${LIB_BUILD_TYPE} mongoose.c)

file(READ "mongoose.h" MONGOSE_H)
string(REGEX MATCH "#define[ \t]+MG_VERSION[ \t]+\"([0-9]+)\\.([0-9]+)\"" _ "${MONGOSE_H}")
set(MVERSION "${CMAKE_MATCH_1}.${CMAKE_MATCH_2}")
set(MABI "${CMAKE_MATCH_1}")
set_target_properties(mongoose PROPERTIES
  VERSION "${MVERSION}"
  SOVERSION "${MABI}"
)

message(STATUS "Building library ${PROJECT_NAME} ${MVERSION}: ${LIB_BUILD_TYPE}")

install(TARGETS mongoose DESTINATION lib)
install(FILES mongoose.h DESTINATION include)
