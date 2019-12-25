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

#*******************************************************************************
# MQTTSNPacketClient library
#*******************************************************************************

add_library(MQTTSNPacketClient
  MQTTSNConnectClient
  MQTTSNPacket
  MQTTSNSearchClient
  MQTTSNSubscribeClient
  MQTTSNUnsubscribeClient
  MQTTSNSerializePublish
  MQTTSNDeserializePublish
)

# Allow build-tree library usage
add_library(${MQTTSNPACKET_PKG_EXPORT_NAME}::MQTTSNPacketClient
  ALIAS MQTTSNPacketClient
)
generate_export_header(MQTTSNPacketClient)
target_compile_features(MQTTSNPacketClient PUBLIC c_std_99)
set_target_properties(MQTTSNPacketClient
  PROPERTIES
    C_EXTENSIONS OFF
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    INTERFACE_MQTTSNPacketClient_SOVERSION ${PROJECT_VERSION_MAJOR}
)
set_property(
  TARGET MQTTSNPacketClient
  APPEND PROPERTY
    COMPATIBLE_INTERFACE_STRING INTERFACE_MQTTSNPacketClient_SOVERSION
)

if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(MQTTSNPacketClient PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)

target_include_directories(MQTTSNPacketClient
  PUBLIC
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
)

#*******************************************************************************
# MQTTSNPacketServer library
#*******************************************************************************

add_library(MQTTSNPacketServer
  MQTTSNConnectServer
  MQTTSNPacket
  MQTTSNSearchServer
  MQTTSNSubscribeServer
  MQTTSNUnsubscribeServer
  MQTTSNSerializePublish
  MQTTSNDeserializePublish
)

# Allow build-tree library usage
add_library(${MQTTSNPACKET_PKG_EXPORT_NAME}::MQTTSNPacketServer
  ALIAS MQTTSNPacketServer
)

generate_export_header(MQTTSNPacketServer)
target_compile_features(MQTTSNPacketServer PUBLIC c_std_99)
set_target_properties(MQTTSNPacketServer
  PROPERTIES
    C_EXTENSIONS OFF
    VERSION "${PahoMQTTSNPacket_VERSION}"
    SOVERSION 1
    INTERFACE_MQTTSNPacketServer_MAJOR_VERSION 1
)
set_property(
  TARGET MQTTSNPacketServer
  APPEND PROPERTY
    COMPATIBLE_INTERFACE_STRING MQTTSNPacketServer_MAJOR_VERSION
)

if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(MQTTSNPacketServer PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)

target_include_directories(MQTTSNPacketServer
  PUBLIC
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
)

#*******************************************************************************
# Library installation
#*******************************************************************************

install(
  TARGETS
    MQTTSNPacketClient
    MQTTSNPacketServer
  EXPORT ${MQTTSNPACKET_PKG_EXPORT_NAME}
  ARCHIVE
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PERMISSIONS
      OWNER_READ OWNER_WRITE
      GROUP_READ
      WORLD_READ
  LIBRARY
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PERMISSIONS
      OWNER_READ OWNER_WRITE OWNER_EXECUTE
      GROUP_READ GROUP_EXECUTE
      WORLD_READ WORLD_EXECUTE
  RUNTIME
    DESTINATION ${CMAKE_INSTALL_BINDIR}
    PERMISSIONS
      OWNER_READ OWNER_WRITE OWNER_EXECUTE
      GROUP_READ GROUP_EXECUTE
      WORLD_READ WORLD_EXECUTE
)

install(
  FILES
    MQTTSNConnect.h
    MQTTSNPacket.h
    MQTTSNPublish.h
    MQTTSNSearch.h
    MQTTSNSubscribe.h
    MQTTSNUnsubscribe.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    PERMISSIONS
      OWNER_READ OWNER_WRITE
      GROUP_READ
      WORLD_READ
)
