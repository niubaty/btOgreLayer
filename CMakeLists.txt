cmake_minimum_required(VERSION 2.8.6)

set(Bullet_DIR F:/ogre0112/bullet3-master/build)
set(OGRE_DIR F:/ogre0112/ogre_master/build36/sdk/CMake)
project(BtOgre)

find_package(Bullet REQUIRED)
find_package(OGRE REQUIRED)

option(BUILD_DEMO "build demo application" TRUE)

include_directories(
    ${PROJECT_SOURCE_DIR}/btogre/include/
    ${PROJECT_SOURCE_DIR}
    ${BULLET_INCLUDE_DIRS}
    ${OGRE_INCLUDE_DIRS}
)
link_directories(${OGRE_LIBRARY_DIRS})

#set(BTOGRELAYER_SRC ${CMAKE_SOURCE_DIR}/btOgreLayer.cpp)
#add_definitions(-DIMGUI_DISABLE_OBSOLETE_FUNCTIONS)
#include_directories(${CMAKE_SOURCE_DIR})
#add_library(btOgreLayer ${BTOGRELAYER_SRC})
#set_property(TARGET btOgreLayer PROPERTY POSITION_INDEPENDENT_CODE ON)
#target_link_libraries(btOgreLayer PUBLIC ${OGRE_LIBRARIES})




add_library(BtOgre ${PROJECT_SOURCE_DIR}/btogre/BtOgre.cpp ${PROJECT_SOURCE_DIR}/btOgreLayer.cpp)
target_link_libraries(BtOgre ${BULLET_LIBRARIES} ${OGRE_LIBRARIES})

INSTALL(TARGETS BtOgre
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib)

if(BUILD_DEMO)
    add_executable(BtOgreLayerTest newdemo/main2.cpp)
    target_link_libraries(BtOgreLayerTest BtOgre)

    configure_file(btogre/demo/data/resources.cfg.in ${CMAKE_BINARY_DIR}/resources.cfg @ONLY)

    # copy essential config files next to our binary where OGRE autodiscovers them
    file(COPY ${OGRE_CONFIG_DIR}/plugins.cfg DESTINATION ${CMAKE_BINARY_DIR})
endif()

# Python Bindings
option(BUILD_PYTHON "build python bindings" ON)
if(BUILD_PYTHON)
    add_subdirectory(python)
endif()