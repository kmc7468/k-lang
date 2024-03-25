cmake_minimum_required(VERSION 3.28)
project(k-lang)

# Includes
include(CheckIPOSupported)

# Variables
## C++ Standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

## Build System
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "bin")

## LTO
check_ipo_supported(RESULT IS_LTO_SUPPORTED)
if(IS_LTO_SUPPORTED)
	set(CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE ON)
endif()

# Targets
## k-lang
add_executable(k-lang)

file(GLOB_RECURSE KLANG_CPP_FILES CONFIGURE_DEPENDS "src/*.cpp")
target_sources(k-lang PRIVATE ${KLANG_CPP_FILES})

file(GLOB_RECURSE KLANG_CPPM_FILES CONFIGURE_DEPENDS "src/*.cppm")
target_sources(k-lang PRIVATE
	FILE_SET CXX_MODULES
	BASE_DIRS src
	FILES ${KLANG_CPPM_FILES}
)

# Installation
install(TARGETS k-lang DESTINATION "bin")