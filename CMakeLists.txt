cmake_minimum_required(VERSION 2.8)
SET(CMAKE_PREFIX_PATH "C:/Program Files (x86)/Microsoft SDKs/Windows/v7.1A/Lib/x64")
project(osgQtQuick CXX)

set(osgQtQuick_VERSION_MAJOR 2)
set(osgQtQuick_VERSION_MINOR 0)
set(osgQtQuick_VERSION_PATCH 0)
set(osgQtQuick_VERSION_TWEAK 7)

IF(WIN32)
  IF(MSVC)
        # This option is to enable the /MP switch for Visual Studio 2005 and above compilers
        OPTION(WIN32_USE_MP "Set to ON to build osgEarth with the /MP option (Visual Studio 2005 and above)." OFF)
        MARK_AS_ADVANCED(WIN32_USE_MP)
        IF(WIN32_USE_MP)
            SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")
        ENDIF(WIN32_USE_MP)

        # turn off various warnings
        # foreach(warning 4244 4251 4267 4275 4290 4786 4305 4996)
        #     SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd${warning}")
        # endforeach(warning)

        # More MSVC specific compilation flags
        ADD_DEFINITIONS(-D_SCL_SECURE_NO_WARNINGS)
        ADD_DEFINITIONS(-D_CRT_SECURE_NO_DEPRECATE)
    ENDIF(MSVC)
ENDIF(WIN32)

set(osgQtQuick_BUILD_DOCS OFF
  CACHE BOOL "Set to ON to build reference documentation (By default is OFF)")

set(osgQtQuick_BUILD_DEBUGGER ON
  CACHE BOOL "Set to ON to build debugger application (By default is OFF)")

set(osgQtQuick_WITH_EARTH ON
  CACHE BOOL "Set to ON to build with osgEarth library support (By default is OFF)")

set(osgQtQuick_VERSION
  ${osgQtQuick_VERSION_MAJOR}
  ${osgQtQuick_VERSION_MINOR}
  ${osgQtQuick_VERSION_PATCH}
  ${osgQtQuick_VERSION_TWEAK})
string(REPLACE ";" "." osgQtQuick_VERSION "${osgQtQuick_VERSION}")

if(NOT DEFINED CMAKE_DEBUG_POSTFIX)
  set(CMAKE_DEBUG_POSTFIX  "d")
endif()

set(osgQtQuick_ROOT_DIR "${CMAKE_CURRENT_LIST_DIR}")

configure_file("${CMAKE_CURRENT_LIST_DIR}/cmake/version.hpp.in"
    "${CMAKE_BINARY_DIR}/include/osgQtQuick/Version" @ONLY)
install(FILES "${CMAKE_BINARY_DIR}/include/osgQtQuick/Version"
  DESTINATION "include/osgQtQuick")

configure_file("${CMAKE_CURRENT_LIST_DIR}/cmake/osgQtQuickConfig.cmake.in"
    "${CMAKE_BINARY_DIR}/lib/cmake/osgQtQuick/osgQtQuickConfig.cmake" @ONLY)
install(FILES "${CMAKE_BINARY_DIR}/lib/cmake/osgQtQuick/osgQtQuickConfig.cmake"
  DESTINATION "lib/cmake/osgQtQuick")

configure_file("${CMAKE_CURRENT_LIST_DIR}/cmake/osgQtQuickConfigVersion.cmake.in"
    "${CMAKE_BINARY_DIR}/lib/cmake/osgQtQuick/osgQtQuickConfigVersion.cmake" @ONLY)
install(FILES "${CMAKE_BINARY_DIR}/lib/cmake/osgQtQuick/osgQtQuickConfigVersion.cmake"
  DESTINATION "lib/cmake/osgQtQuick")

include_directories("${CMAKE_BINARY_DIR}/include")

if(DEFINED DYNAMIC_OPENSCENEGRAPH)
  set(DYNAMIC_OSGQTQUICK ${DYNAMIC_OPENSCENEGRAPH})
else()
  set(DYNAMIC_OSGQTQUICK ON)
endif()

if(DYNAMIC_OSGQTQUICK)
  set(OSGQTQUICK_USER_DEFINED_DYNAMIC_OR_STATIC "SHARED")
else()
  set(OSGQTQUICK_USER_DEFINED_DYNAMIC_OR_STATIC "STATIC")
endif()

add_subdirectory(src)

if(osgQtQuick_BUILD_DOCS)
  add_subdirectory(doc)
endif()
