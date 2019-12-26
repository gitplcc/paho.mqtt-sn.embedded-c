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

add_executable(testPFW
  mainTestProcess
  TestProcess
  TestQue
  TestTree23
  TestTopics
  TestTopicIdMap
  TestTask
  $<TARGET_OBJECTS:common>
)

target_compile_features(testPFW PUBLIC cxx_std_11)
set_target_properties(testPFW
  PROPERTIES
    CXX_EXTENSIONS ON
    INTERPROCEDURAL_OPTIMIZATION ${IPO_ENABLED}
)

if(CMAKE_COMPILER_IS_GNUCXX)
  target_compile_options(testPFW PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUCXX)

target_include_directories(testPFW
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/..
    ${CMAKE_CURRENT_SOURCE_DIR}/../linux
    ${CMAKE_CURRENT_SOURCE_DIR}/../linux/udp
)

target_link_libraries(testPFW
  PRIVATE
    OpenSSL::Crypto
    OpenSSL::SSL
    PahoMQTTSNPacket::MQTTSNPacketClient
    PahoMQTTSNPacket::MQTTSNPacketServer
    Threads::Threads
)
