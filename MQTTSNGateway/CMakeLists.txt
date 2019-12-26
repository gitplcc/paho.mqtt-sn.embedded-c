#*******************************************************************************
# CMakeLists.txt
#
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# and Eclipse Distribution License v1.0 which accompany this distribution.
#
# The Eclipse Public License is available at
#    http://www.eclipse.org/legal/epl-v10.html
# and the Eclipse Distribution License is available at
#   http://www.eclipse.org/org/documents/edl-v10.php.
#
# Contributors:
#    Pedro Luis Castedo Cepeda - initial version
#*******************************************************************************

cmake_minimum_required(VERSION 3.9)
cmake_minimum_required(VERSION 3.9...3.14)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
  cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(
  PahoMQTTSNGateway
  VERSION 1.3.1
  DESCRIPTION "MQTT-SN transparent/aggregating gateway"
  LANGUAGES C CXX
)

include(GNUInstallDirs)
include(CMakeDependentOption)

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  set(CPACK_PACKAGE_NAME ${PROJECT_NAME})
  set(CPACK_PACKAGE_VENDOR "Eclipse Foundation")
  #set(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/ReadMe.txt")
  #set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/Copyright.txt")
  set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
  set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
  set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
  set(
    CPACK_PACKAGE_INSTALL_DIRECTORY
    ${CPACK_PACKAGE_NAME}/${CPACK_PACKAGE_VERSION}
  )
  set(CPACK_STRIP_FILES ON)

  include(CPack)
endif()

# Optional targets
if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  include(CTest)
endif()

# Search for tools
find_program(CCACHE_PROGRAM ccache)
cmake_dependent_option(
  CCACHE_ENABLED "Enable using CCache"
  ON "CCACHE_PROGRAM"
  OFF
)
if(CCACHE_ENABLED)
  set(CMAKE_CXX_COMPILER_LAUNCHER ${CCACHE_PROGRAM})
endif()

# Search for dependencies
if (
  NOT TARGET PahoMQTTSNPacket::MQTTSNPacketClient
  OR
  NOT TARGET PahoMQTTSNPacket::MQTTSNPacketServer
)
  find_package(PahoMQTTSNPacket REQUIRED)
endif()
find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)

# Check if Interprocedural Optimization supported
include(CheckIPOSupported)
check_ipo_supported(RESULT IPO_SUPPORTED LANGUAGES CXX)
cmake_dependent_option(
  IPO_ENABLED "Enable InterProcedural Optimization"
  ON "IPO_SUPPORTED;NOT CMAKE_BUILD_TYPE EQUAL Debug"
  OFF
)

# Add subdirectories
add_subdirectory(src)
if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  if(BUILD_TESTING)
    add_subdirectory(src/tests)
  endif()
endif()
