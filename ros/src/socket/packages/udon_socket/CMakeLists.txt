cmake_minimum_required(VERSION 2.8.3)
project(udon_socket)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  tablet_socket
)

set(CMAKE_CXX_FLAGS "-std=c++11 -O2 -Wall ${CMAKE_CXX_FLAGS}")

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES udon
  CATKIN_DEPENDS tablet_socket
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(udon lib/udon_socket/udon.cpp)

add_executable(udon_receiver nodes/udon_receiver/udon_receiver.cpp)
target_link_libraries(udon_receiver udon ${catkin_LIBRARIES})

add_executable(udon_sender nodes/udon_sender/udon_sender.cpp)
target_link_libraries(udon_sender udon ${catkin_LIBRARIES})
add_dependencies(udon_sender
  tablet_socket_generate_messages_cpp
)
