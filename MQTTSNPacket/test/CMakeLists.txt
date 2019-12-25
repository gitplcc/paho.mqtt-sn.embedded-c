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

add_executable(
  test1
  test1
)

target_compile_features(test1 PRIVATE c_std_99)

target_include_directories(test1
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(test1
  PRIVATE
    MQTTSNPacketClient
    MQTTSNPacketServer
)

add_test(NAME test1 COMMAND test1)

add_executable(
  test2
  test2
)

target_compile_features(test2 PRIVATE c_std_99)

target_include_directories(test2
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(test2
  PRIVATE
    MQTTSNPacketClient
    MQTTSNPacketServer
)

target_compile_features(test2 PRIVATE c_std_99)
set_target_properties(test2
  PROPERTIES
    CXX_EXTENSIONS OFF
)

add_test(NAME test2 COMMAND test2)
