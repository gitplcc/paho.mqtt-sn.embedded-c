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

#*******************************************************************************/
# qos0pub
#*******************************************************************************/

add_executable(qos0pub
  qos0pub
  transport
)
target_compile_features(qos0pub PUBLIC c_std_99)
set_target_properties(qos0pub
  PROPERTIES
    C_EXTENSIONS OFF
)
if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(qos0pub PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)
target_link_libraries(qos0pub
  PRIVATE
    MQTTSNPacketClient
)

#*******************************************************************************/
# qos0pub_register
#*******************************************************************************/

add_executable(qos0pub_register
  qos0pub_register
  transport
)
target_compile_features(qos0pub_register PUBLIC c_std_99)
set_target_properties(qos0pub_register
  PROPERTIES
    C_EXTENSIONS OFF
)
if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(qos0pub_register PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)
target_link_libraries(qos0pub_register
  PRIVATE
    MQTTSNPacketClient
)

#*******************************************************************************/
# qos-1pub
#*******************************************************************************/

add_executable(qos-1pub
  qos-1pub
  transport
)
target_compile_features(qos-1pub PUBLIC c_std_99)
set_target_properties(qos-1pub
  PROPERTIES
    C_EXTENSIONS OFF
)
if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(qos-1pub PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)
target_link_libraries(qos-1pub
  PRIVATE
    MQTTSNPacketClient
)

#*******************************************************************************/
# qos-1pub_extended
#*******************************************************************************/

add_executable(qos-1pub_extended
  qos-1pub_extended
  transport
)
target_compile_features(qos-1pub_extended PUBLIC c_std_99)
set_target_properties(qos-1pub
  PROPERTIES
    C_EXTENSIONS OFF
)
if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(qos-1pub_extended PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)
target_link_libraries(qos-1pub_extended
  PRIVATE
    MQTTSNPacketClient
)

#*******************************************************************************/
# qos1pub
#*******************************************************************************/

add_executable(qos1pub
  qos1pub
  transport
)
target_compile_features(qos1pub PUBLIC c_std_99)
set_target_properties(qos1pub
  PROPERTIES
    C_EXTENSIONS OFF
)
if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(qos1pub PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)
target_link_libraries(qos1pub
  PRIVATE
    MQTTSNPacketClient
)

#*******************************************************************************/
# pub0sub1
#*******************************************************************************/

add_executable(pub0sub1
  pub0sub1
  transport
)
target_compile_features(pub0sub1 PUBLIC c_std_99)
set_target_properties(pub0sub1
  PROPERTIES
    C_EXTENSIONS OFF
)
if(CMAKE_COMPILER_IS_GNUC)
  target_compile_options(pub0sub1 PRIVATE -march=native)
endif(CMAKE_COMPILER_IS_GNUC)
target_link_libraries(pub0sub1
  PRIVATE
    MQTTSNPacketClient
)
