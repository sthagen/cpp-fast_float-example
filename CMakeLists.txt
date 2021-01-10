cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

# ---- Project ----

project(fast_float_example LANGUAGES CXX)

# ---- Fetch CPM ----

set(CPM_DOWNLOAD_VERSION 0.27.2)
set(CPM_DOWNLOAD_LOCATION "${CMAKE_BINARY_DIR}/cmake/CPM_${CPM_DOWNLOAD_VERSION}.cmake")

if(NOT (EXISTS ${CPM_DOWNLOAD_LOCATION}))
    message(STATUS "Downloading CPM.cmake v${CPM_DOWNLOAD_VERSION}")
    file(DOWNLOAD https://github.com/TheLartians/CPM.cmake/releases/download/v${CPM_DOWNLOAD_VERSION}/CPM.cmake ${CPM_DOWNLOAD_LOCATION})
endif()

include(${CPM_DOWNLOAD_LOCATION})

# ---- Add dependencies via CPM ----
# See https://github.com/TheLartians/CPM.cmake for details and examples

CPMAddPackage(
  NAME fast_float
  GITHUB_REPOSITORY fastfloat/fast_float
  GIT_TAG v0.7.0
)

CPMAddPackage(
  NAME doctest
  GITHUB_REPOSITORY sthagen/doctest
  GIT_TAG 2020.12
)

if(fast_float_ADDED)
  add_library(fast_float INTERFACE)
  target_include_directories(fast_float INTERFACE ${fast_float_SOURCE_DIR}/include/)
  message("Added ${fast_float_SOURCE_DIR}/include/ as include")
endif()


# ---- Create binary ----

add_executable(fast_float_example main.cpp)
set_target_properties(fast_float_example PROPERTIES CXX_STANDARD 17)

# ---- Enable testing ----

enable_testing() 
add_test(fast_float_example fast_float_example)
