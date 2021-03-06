# This CmakeLists include both OpenNI and OpenCV Libraries
cmake_minimum_required(VERSION 2.8)
project( TestOpenNI )


# OpenCV
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )
MESSAGE(STATUS "The Opencv's include directory is:" ${OpenCV_INCLUDE_DIRS})

#OpenNI
FIND_PATH(OpenNI2_INCLUDE_DIRS OpenNI.h HINTS  $ENV{OPENNI2_INCLUDE} PATH_SUFFIXES openni2)
FIND_LIBRARY(OpenNI2_LIBRARY NAMES OpenNI2 HINTS  $ENV{OPENNI2_LIB} $ENV{OPENNI2_REDIST})
include_directories( ${OpenNI2_INCLUDE_DIRS} )


IF (OpenNI2_INCLUDE_DIRS AND OpenNI2_LIBRARY)
   SET(OpenNI2_FOUND TRUE)
ENDIF (OpenNI2_INCLUDE_DIRS AND OpenNI2_LIBRARY)

IF (OpenNI2_FOUND)
   # show which OpenNI2 was found only if not quiet
   SET(OpenNI2_LIBRARIES ${OpenNI2_LIBRARY})
   MESSAGE(STATUS "Found OpenNI2: ${OpenNI2_LIBRARIES}")
ELSE (OpenNI2_FOUND)
   # fatal error if OpenNI2 is required but not found
   IF (OpenNI2_FIND_REQUIRED)
      MESSAGE(FATAL_ERROR "Could not find OpenNI2. Environment variables OPENNI2_INCLUDE (directory containing OpenNI.h) and OPENNI2_LIB (directory containing OpenNI2 library) could bet set.")
   ENDIF (OpenNI2_FIND_REQUIRED)
ENDIF (OpenNI2_FOUND)

#set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
#set (OPENNI_H /usr/include/openni2/OpenNI.h)
# ---------------------------------------------------------

#PCL
find_package(PCL 1.8 REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})
add_executable(TestOpenNI test.cpp)
target_link_libraries(TestOpenNI ${OpenNI2_LIBRARIES} ${OpenCV_LIBS} ${PCL_COMMON_LIBRARIES} ${PCL_IO_LIBRARIES})
