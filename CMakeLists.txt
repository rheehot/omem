cmake_minimum_required(VERSION 3.8)
project(omem)

add_library(omem INTERFACE)
target_include_directories(omem INTERFACE "include")

set(OMEM_POOL_SIZE 1048576 CACHE STRING "Pool size in bytes")
target_compile_definitions(omem INTERFACE OMEM_POOL_SIZE=${OMEM_POOL_SIZE})

set(OMEM_BUILD_TESTS FALSE CACHE BOOL "Whether to build a test")
if(OMEM_BUILD_TESTS)
	file(GLOB_RECURSE TEST_SRC_FILES "tests/*.cpp")
	add_executable(omem_test ${TEST_SRC_FILES})
	set_target_properties(omem_test PROPERTIES CXX_STANDARD 17)

	find_package(GTest 1.10.0 REQUIRED)
	target_link_libraries(omem_test PRIVATE omem GTest::GTest)

	enable_testing()
	add_test(NAME "omem test" COMMAND omem_test)
endif()
