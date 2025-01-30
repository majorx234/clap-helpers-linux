cmake_minimum_required(VERSION 3.17)
cmake_policy(SET CMP0100 NEW)  # handle .hh files

project(clap-helpers C CXX)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CLAP_HELPERS_TESTS_CXX_STANDARD 14)

option(CLAP_HELPERS_DOWNLOAD_DEPENDENCIES "Resolve CLAP Targets with CPM if not defined" FALSE)
find_package(clap REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(catch2 REQUIRED catch2)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE include)
target_link_libraries(${PROJECT_NAME} INTERFACE clap)
set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD ${CLAP_HELPERS_TESTS_CXX_STANDARD})

install(DIRECTORY include DESTINATION ".")
install(FILES "cmake/clap-helpers-config.cmake" DESTINATION "./lib/cmake/clap-helpers")
