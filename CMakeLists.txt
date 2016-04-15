cmake_minimum_required(VERSION 2.8.3)
project(ximea_camera_opencv_example)

FIND_PACKAGE(catkin REQUIRED)
FIND_PACKAGE(roscpp REQUIRED)
FIND_PACKAGE(cv_bridge REQUIRED)
FIND_PACKAGE(image_transport REQUIRED)

IF(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/package.xml")
    MESSAGE(FATAL_ERROR "${CMAKE_CURRENT_SOURCE_DIR}/package.xml not found.")
ENDIF()

SET(OpenCV_MIN_VERSION "3.1.0")
FIND_PATH(OpenCV_DIR NAMES opencv-config.cmake OpenCVConfig.cmake PATH_SUFFIXES lib/cmake lib)
FIND_PACKAGE( OpenCV ${OpenCV_MIN_VERSION} REQUIRED COMPONENTS core video highgui)
IF(OpenCV_FOUND)
    IF(${OpenCV_VERSION} VERSION_LESS ${OpenCV_MIN_VERSION} AND (SUPPORT_VIDEO OR SUPPORT_IMAGE) )
        MESSAGE(FATAL_ERROR "OpenCV version (${OpenCV_VERSION} is present, while ${OpenCV_MIN_VERSION}+ is required")
    ENDIF()
    include_directories(${OpenCV_INCLUDE_DIRS})
    MESSAGE("Found OpenCV ${OpenCV_VERSION} libraries ${OpenCV_LIBRARIES} and headers ${OpenCV_INCLUDE_DIRS}")
    ADD_DEFINITIONS(-DUSE_OPENCV)
ELSE()
    MESSAGE(FATAL_ERROR "OpenCV not found.")
ENDIF()

catkin_package(CATKIN_DEPENDS std_msgs
               DEPENDS OpenCV
               LIBRARIES ${PROJECT_NAME})

include_directories(include ${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(SOURCE_FILES src/view_ximea_camera.cpp)
add_executable(${PROJECT_NAME} ${SOURCE_FILES})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES}
                                      ${OpenCV_LIBRARIES}
                                      ${roscpp_LIBRARIES}
                                      ${cv_bridge_LIBRARIES}
                                      ${image_transport_LIBRARIES})