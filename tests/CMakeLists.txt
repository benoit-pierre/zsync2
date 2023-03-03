set(CMAKE_MINIMUM_REQUIRED_VERSION 3.2)

enable_testing()
include(CTest)

set(CMAKE_CXX_STANDARD 17)

find_package(GTest REQUIRED)

add_executable(test_zsutil test_zsutil.cpp)
target_link_libraries(test_zsutil GTest::gtest ${CPR_LIBRARIES})

add_test(NAME test_zsutil COMMAND test_zsutil)
