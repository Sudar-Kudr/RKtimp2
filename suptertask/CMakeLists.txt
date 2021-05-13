cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)
option(BUILD_TESTS "Build tests" ON)
######
include("cmake/HunterGate.cmake")

HunterGate(
  URL "https://github.com/ruslo/hunter/archive/v0.23.214.tar.gz"
  SHA1 "e14bc153a7f16d6a5eeec845fb0283c8fad8c358"
)
######
//project(RKtimp2)
project(suptertask)

add_library(suptertask STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/sqrt.cpp)

add_executable(demo ${CMAKE_CURRENT_SOURCE_DIR}/examples/demo.cpp)

target_include_directories(suptertask PUBLIC
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
                           $<INSTALL_INTERFACE:include>
                           )

target_link_libraries(demo ${PROJECT_NAME})

if (BUILD_EXAMPLES)
file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
foreach (EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
target_link_libraries(${EXAMPLE_NAME} suptertask)
install(TARGETS ${EXAMPLE_NAME}
        RUNTIME DESTINATION bin
        )
endforeach (EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif ()

install(TARGETS suptertask
        EXPORT suptertask-config
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        )

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT suptertask-config DESTINATION cmake)

if (BUILD_TESTS)
enable_testing()
add_subdirectory(teatea/googletest)
file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
                                             add_executable(tests ${${PROJECT_NAME}_TEST_SOURCES})
                                             target_link_libraries(tests ${PROJECT_NAME} gtest_main)
                                             add_test(NAME tests COMMAND tests)
                                             endif ()

