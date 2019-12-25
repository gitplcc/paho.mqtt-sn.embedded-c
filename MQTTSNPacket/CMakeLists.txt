#*******************************************************************************
#  Copyright (c) 2017 IBM Corp.
#
#  All rights reserved. This program and the accompanying materials
#  are made available under the terms of the Eclipse Public License v1.0
#  and Eclipse Distribution License v1.0 which accompany this distribution.
#
#  The Eclipse Public License is available at
#     http://www.eclipse.org/legal/epl-v10.html
#  and the Eclipse Distribution License is available at
#    http://www.eclipse.org/org/documents/edl-v10.php.
#
#  Contributors:
#     Ian Craggs - initial version
#     Pedro Luis Castedo Cepeda
#*******************************************************************************/

cmake_minimum_required(VERSION 3.9...3.14)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
  cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(
  PahoMQTTSNPacket
  VERSION 1.0.0
  DESCRIPTION "Simple de/serialization of MQTT-SN packets & helper functions"
  LANGUAGES C
)

include(CMakeDependentOption)
include(GNUInstallDirs)
include(GenerateExportHeader)
include(CMakePackageConfigHelpers)

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

# Select between building shared or static libraries
option(BUILD_SHARED_LIBS "Build shared libraries" ON)

# Optional targets
if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  include(CTest)
  option(BUILD_SAMPLES "Enable sample building" OFF)
endif()

# Search for tools
find_program(CCACHE_PROGRAM ccache)
cmake_dependent_option(
  CCACHE_ENABLED "Enable using CCache"
  ON "CCACHE_PROGRAM" OFF
)
if(CCACHE_ENABLED)
  set(CMAKE_CXX_COMPILER_LAUNCHER ${CCACHE_PROGRAM})
endif()

set(MQTTSNPACKET_PKG_EXPORT_NAME ${PROJECT_NAME})

# Add subdirectories
add_subdirectory(src)
if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  if(BUILD_SAMPLES)
    add_subdirectory(samples)
  endif()
  if(BUILD_TESTING)
    add_subdirectory(test)
  endif()
endif()

#*******************************************************************************
# CMake build system integration
#*******************************************************************************

write_basic_package_version_file(
  ${MQTTSNPACKET_PKG_EXPORT_NAME}Version.cmake
  VERSION ${PACKAGE_VERSION}
  COMPATIBILITY AnyNewerVersion
)
install(
  FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${MQTTSNPACKET_PKG_EXPORT_NAME}Version.cmake
  DESTINATION ${CMAKE_INSTALL_DATADIR}/cmake/${MQTTSNPACKET_PKG_EXPORT_NAME}
  PERMISSIONS
    OWNER_READ OWNER_WRITE
    GROUP_READ
    WORLD_READ
)

install(
  EXPORT ${MQTTSNPACKET_PKG_EXPORT_NAME}
  DESTINATION ${CMAKE_INSTALL_DATADIR}/cmake/${MQTTSNPACKET_PKG_EXPORT_NAME}
  NAMESPACE ${MQTTSNPACKET_PKG_EXPORT_NAME}::
  FILE ${MQTTSNPACKET_PKG_EXPORT_NAME}Config.cmake
    PERMISSIONS
      OWNER_READ OWNER_WRITE
      GROUP_READ
      WORLD_READ
)

# Export targets
export(
  TARGETS
    MQTTSNPacketClient
    MQTTSNPacketServer
  NAMESPACE ${MQTTSNPACKET_PKG_EXPORT_NAME}::
  FILE ${MQTTSNPACKET_PKG_EXPORT_NAME}.cmake
)
set(CMAKE_EXPORT_PACKAGE_REGISTRY ON)
export(PACKAGE ${MQTTSNPACKET_PKG_EXPORT_NAME})
