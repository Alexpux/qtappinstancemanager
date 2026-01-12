cmake_minimum_required(VERSION 3.21.0)

# Enable parallel build (not enabled by default on Windows).
set(CMAKE_BUILD_PARALLEL_LEVEL $ENV{NUMBER_OF_PROCESSORS})

enable_testing()

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Set project information.
project("QtAppInstanceManager"
  LANGUAGES CXX
  VERSION 1.3.1.0
  DESCRIPTION "A tool to communicate between the instances of your Qt application."
  HOMEPAGE_URL "https://github.com/oclero/QtAppInstanceManager"
)
set(PROJECT_NAMESPACE "oclero")

# Global flags.
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
if(NOT CMAKE_OSX_DEPLOYMENT_TARGET)
  set(CMAKE_OSX_DEPLOYMENT_TARGET "13.6")
endif()
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake")

# Include GNUInstallDirs for standard installation paths.
include(GNUInstallDirs)

# Find Qt.
find_package(Qt6 REQUIRED COMPONENTS Core Network)

# qt_standard_project_setup() is only available in Qt >= 6.3.
if(COMMAND qt_standard_project_setup)
  qt_standard_project_setup()
else()
  set(CMAKE_AUTOMOC ON)
  set(CMAKE_AUTOUIC ON)
  set(CMAKE_AUTORCC ON)
endif()

# The library.
add_subdirectory(src)

# Tests.
if(QTAPPINSTANCEMANAGER_TESTS)
  add_subdirectory(tests)
endif()

# Examples.
if(QTAPPINSTANCEMANAGER_EXAMPLES)
  add_subdirectory(examples/single)
  add_subdirectory(examples/multiple)
endif()
