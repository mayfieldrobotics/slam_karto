cmake_minimum_required(VERSION 2.8.3)
project(slam_karto)

find_package(catkin REQUIRED
  COMPONENTS
    cmake_modules
    message_filters
    nav_msgs
    open_karto
    rosconsole
    roscpp
    sensor_msgs
    sparse_bundle_adjustment
    tf
    visualization_msgs
)
find_package(Eigen REQUIRED)

include_directories(${catkin_INCLUDE_DIRS} ${EIGEN_INCLUDE_DIRS})

add_definitions(${EIGEN_DEFINITIONS})

catkin_package(
        DEPENDS Eigen
        INCLUDE_DIRS src
        LIBRARIES spa_solver
)

add_executable(slam_karto src/slam_karto.cpp src/spa_solver.cpp)
target_link_libraries(slam_karto ${catkin_LIBRARIES})

add_library(spa_solver src/spa_solver.cpp)
target_link_libraries(spa_solver ${catkin_LIBRARIES})

install(TARGETS slam_karto spa_solver
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY src/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h"
)