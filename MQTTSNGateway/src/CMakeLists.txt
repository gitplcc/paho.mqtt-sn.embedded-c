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

#*******************************************************************************
# Configure version header
#*******************************************************************************

configure_file(MQTTSNGWVersion.h.in MQTTSNGWVersion.h @ONLY)

#*******************************************************************************
# Utility object library target
#*******************************************************************************

add_library(common
  OBJECT
    MQTTGWConnectionHandler
    MQTTGWPacket
    MQTTGWPublishHandler
    MQTTGWSubscribeHandler
    MQTTSNGateway
    MQTTSNGWBrokerRecvTask
    MQTTSNGWBrokerSendTask
    MQTTSNGWClient
    MQTTSNGWClientRecvTask
    MQTTSNGWClientSendTask
    MQTTSNGWConnectionHandler
    MQTTSNGWLogmonitor
    MQTTSNGWPacket
    MQTTSNGWPacketHandleTask
    MQTTSNGWProcess
    MQTTSNGWPublishHandler
    MQTTSNGWSubscribeHandler
    MQTTSNGWEncapsulatedPacket
    MQTTSNGWForwarder
    MQTTSNGWQoSm1Proxy
    MQTTSNGWAdapter
    MQTTSNGWAggregater
    MQTTSNGWClientList
    MQTTSNGWTopic
    MQTTSNGWAdapterManager
    MQTTSNAggregateConnectionHandler
    MQTTSNGWMessageIdTable
    MQTTSNGWAggregateTopicTable
    linux/Timer
    linux/Network
    linux/Threading
    linux/udp/SensorNetwork
)

target_compile_features(common PUBLIC cxx_std_11)
set_target_properties(common
  PROPERTIES
    CXX_EXTENSIONS ON
    INTERPROCEDURAL_OPTIMIZATION ${IPO_ENABLED}
)

target_include_directories(common
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/linux
    ${CMAKE_CURRENT_SOURCE_DIR}/linux/udp
    $<TARGET_PROPERTY:PahoMQTTSNPacket::MQTTSNPacketClient,INTERFACE_INCLUDE_DIRECTORIES>
)

#*******************************************************************************
# MQTT-SNGateway target
#*******************************************************************************

add_executable(MQTT-SNGateway
  mainGateway
  $<TARGET_OBJECTS:common>
)

target_compile_features(MQTT-SNGateway PUBLIC cxx_std_11)
set_target_properties(MQTT-SNGateway
  PROPERTIES
    CXX_EXTENSIONS ON
    INTERPROCEDURAL_OPTIMIZATION ${IPO_ENABLED}
)

if(CMAKE_COMPILER_IS_GNUCXX)
  target_compile_options(MQTT-SNGateway PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUCXX)

target_include_directories(MQTT-SNGateway
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/linux
    ${CMAKE_CURRENT_SOURCE_DIR}/linux/udp
)

target_link_libraries(MQTT-SNGateway
  PRIVATE
    OpenSSL::Crypto
    OpenSSL::SSL
    PahoMQTTSNPacket::MQTTSNPacketClient
    PahoMQTTSNPacket::MQTTSNPacketServer
    Threads::Threads
)

#*******************************************************************************
# MQTT-SNLogmonitor target
#*******************************************************************************

add_executable(MQTT-SNLogmonitor
  mainLogmonitor
  $<TARGET_OBJECTS:common>
)

target_compile_features(MQTT-SNLogmonitor PUBLIC cxx_std_11)
set_target_properties(MQTT-SNLogmonitor
  PROPERTIES
    CXX_EXTENSIONS ON
    INTERPROCEDURAL_OPTIMIZATION ${IPO_ENABLED}
)

if(CMAKE_COMPILER_IS_GNUCXX)
  target_compile_options(MQTT-SNLogmonitor PRIVATE -march=native)
endif()

target_include_directories(MQTT-SNLogmonitor
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/linux
    ${CMAKE_CURRENT_SOURCE_DIR}/linux/udp
)

target_link_libraries(MQTT-SNLogmonitor
  PRIVATE
    OpenSSL::Crypto
    OpenSSL::SSL
    PahoMQTTSNPacket::MQTTSNPacketClient
    PahoMQTTSNPacket::MQTTSNPacketServer
    Threads::Threads
)

install(
  TARGETS MQTT-SNGateway MQTT-SNLogmonitor
  RUNTIME
    DESTINATION ${CMAKE_INSTALL_BINDIR}
    PERMISSIONS
      OWNER_READ OWNER_WRITE OWNER_EXECUTE
      GROUP_READ GROUP_EXECUTE
      WORLD_READ
)
