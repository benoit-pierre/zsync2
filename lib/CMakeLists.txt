set(CMAKE_MINIMUM_REQUIRED_VERSION 3.2)

include_directories(.)

# include subdirectories in the right order
add_subdirectory(zlib)
add_subdirectory(librcksum)
add_subdirectory(libzsync)

# external dependencies used as submodules

# using system cURL as it has proper SSL support etc., and reduces the build time
set(USE_SYSTEM_CURL ON CACHE BOOL "" FORCE)
# disable CPR tests to save compile time and avoid clashes with own tests
set(BUILD_CPR_TESTS OFF CACHE BOOL "" FORCE)

# for some reason we don't know, cpr wants to build shared libs by default
# this is not the default behavior of CMake, so we force this option off again
# unfortunately, we have to do this globally...
# long term goal is to replace the submodule in some way, e.g., by using prebuilt cpr binaries
set(BUILD_SHARED_LIBS OFF)

# there is no need to build our own libcurl
# in fact, building it as a static lib creates a whole circus of problems, and also takes time
set(CPR_USE_SYSTEM_CURL ON)

add_subdirectory(cpr)
# since we want to have static builds of cpr, we have to enable -fPIC manually
set_property(TARGET cpr PROPERTY POSITION_INDEPENDENT_CODE ON)

# it's quite simple dealing with the args library directly from CMake
add_library(args INTERFACE)
target_sources(args INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/args/args.hxx")
target_include_directories(args INTERFACE args)

# minimal zlib-licensed hash library
add_subdirectory(hashlib)
