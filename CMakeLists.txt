# http://ros.org/doc/groovy/api/catkin/html/user_guide/supposed.html
cmake_minimum_required(VERSION 2.8.3)
project(camera_aravis)
add_compile_options(-g)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules)
find_package(catkin REQUIRED COMPONENTS roscpp sensor_msgs image_transport camera_info_manager dynamic_reconfigure driver_base tf dynamic_reconfigure)
find_package(GLIB2 REQUIRED)
find_package(PkgConfig REQUIRED)                                                                                                            
                                                                                                                                            
set(DEPENDENCIES "aravis-0.8")                                                                                                            
pkg_check_modules(DEPS REQUIRED IMPORTED_TARGET ${DEPENDENCIES})   

generate_dynamic_reconfigure_options(cfg/CameraAravisConfig.cfg)
catkin_package(
    DEPENDS Aravis GLIB2
    CATKIN_DEPENDS roscpp sensor_msgs image_transport camera_info_manager dynamic_reconfigure driver_base tf
    INCLUDE_DIRS
    LIBRARIES
)

include_directories(cfg/cpp ${catkin_INCLUDE_DIRS} ${ARAVIS_INCLUDE_PATH} ${GLIB2_INCLUDE_DIRS})

#message(${ARAVIS_LIBRARY})

add_executable(camnode src/camnode.cpp)

target_link_libraries(camnode ${catkin_LIBRARIES}  glib-2.0 gmodule-2.0 gobject-2.0   PkgConfig::DEPS)
add_dependencies(camnode ${PROJECT_NAME}_gencfg)
