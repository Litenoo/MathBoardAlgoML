cmake_minimum_required(VERSION 3.5.0)
project(mathboard VERSION 0.0.1 LANGUAGES C CXX)

include(CheckCXXCompilerFlag)

# Enable flag if supported
function(enable_cxx_compiler_flag_if_supported flag)
    string(FIND "${CMAKE_CXX_FLAGS}" "${flag}" flag_already_set)
    if(flag_already_set EQUAL -1)
        check_cxx_compiler_flag("${flag}" flag_supported)
        if(flag_supported)
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${flag}" PARENT_SCOPE)
        endif()
        unset(flag_supported CACHE)
    endif()
endfunction()

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Gather source and header files
file (GLOB_RECURSE SOURCES CONFIGURE_DEPENDS "src/*.cpp")
file (GLOB_RECURSE HEADERS CONFIGURE_DEPENDS "src/*.hpp")

# Check if OpenCV is installed
find_package(OpenCV REQUIRED)

# Enable extra warnings
enable_cxx_compiler_flag_if_supported("-Wall")
enable_cxx_compiler_flag_if_supported("-Wextra")
enable_cxx_compiler_flag_if_supported("-pedantic")

# Add binary
add_executable(${PROJECT_NAME} ${SOURCES})

# Add include directories
target_include_directories(${PROJECT_NAME}
	PRIVATE src
	PRIVATE ${OpenCV_INCLUDE_DIRS}
)

# Add libraries to link
target_link_libraries(${PROJECT_NAME}
	PRIVATE ${OpenCV_LIBS}
)

