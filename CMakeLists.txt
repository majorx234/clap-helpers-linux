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

add_executable(${PROJECT_NAME}-tests
#EXCLUDE_FROM_ALL
       tests/context-menu-builder-compiles.cc
       tests/create-an-actual-host.cc
       tests/create-an-actual-plugin.cc
       tests/hex-encoder.cc
       tests/host.cc
       tests/plugin.cc
       tests/param-queue-tests.cc
       tests/event-list-tests.cc
       tests/main.cc
       tests/preset-discovery-indexer.cc
       tests/preset-discovery-provider.cc
       tests/preset-discovery-metadata-receiver.cc
)
set_target_properties(${PROJECT_NAME}-tests PROPERTIES CXX_STANDARD ${CLAP_HELPERS_TESTS_CXX_STANDARD})
target_include_directories(${PROJECT_NAME}-tests PUBLIC ${catch2_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME}-tests ${PROJECT_NAME} ${catch2_LIBRARIES})
target_compile_definitions(${PROJECT_NAME}-tests PUBLIC -DCATCH_CONFIG_PREFIX_ALL)
target_compile_options(${PROJECT_NAME}-tests
 PRIVATE
 -Werror
)

#    add_custom_command(TARGET ${PROJECT_NAME}-tests POST_BUILD
#            COMMAND ${CMAKE_COMMAND} -E echo "Relocating $<TARGET_FILE:${PROJECT_NAME}-tests> to ${CMAKE_BINARY_DIR}"
#            COMMAND ${CMAKE_COMMAND} -E copy "$<TARGET_FILE:${PROJECT_NAME}-tests>" "${CMAKE_BINARY_DIR}"
#    )

install(DIRECTORY include DESTINATION ".")
install(FILES "cmake/clap-helpers-config.cmake" DESTINATION "./lib/cmake/clap-helpers")
